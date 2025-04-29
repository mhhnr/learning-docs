
# Understanding Astra: A Complete Example for Beginners

Let's follow a concrete example to understand how every part of Astra works. Imagine you want to generate a blog post about "Artificial Intelligence in Healthcare." I'll walk through how this request flows through the entire system, explaining each file and folder along the way.

## Our Journey Map

1. You send a request to generate a blog post
2. The API processes your request
3. The workflow coordinates multiple agents
4. The agents research, gather content, and write
5. The result is stored in the database
6. The response is sent back to you

## 1. The Request Journey Begins: Dockerfile

```bash
FROM agnohq/python:3.12

ARG USER=app
ARG APP_DIR=/app
ENV APP_DIR=${APP_DIR}

# Create user and home directory
RUN groupadd -g 61000 ${USER} \
  && useradd -g 61000 -u 61000 -ms /bin/bash -d ${APP_DIR} ${USER}

WORKDIR ${APP_DIR}

# Copy requirements.txt
COPY requirements.txt ./

# Install requirements
RUN uv pip sync requirements.txt --system

# Copy project files
COPY . .

# Set permissions for the /app directory
RUN chown -R ${USER}:${USER} ${APP_DIR}

# Switch to non-root user
USER ${USER}

ENTRYPOINT ["/app/scripts/entrypoint.sh"]
CMD ["chill"]
```

**In Simple Terms:**
- This file is like a recipe for creating the container (a special kind of virtual computer) where our application runs
- It starts with a pre-built image that has Python 3.12 installed
- It creates a user named "app" for security (so the application doesn't run as the admin)
- It copies all our code into the container
- It installs all the required Python packages
- Finally, it sets up the container to run the entrypoint.sh script when it starts

**Why It Matters:**
Every time someone deploys Astra (even on their laptop), this ensures all the code, dependencies, and settings are exactly the same. It's like shipping the entire development environment along with your code.

## 2. Starting Up: Scripts/entrypoint.sh

```bash
#!/bin/bash

############################################################################
# Container Entrypoint script
############################################################################

if [[ "$PRINT_ENV_ON_LOAD" = true || "$PRINT_ENV_ON_LOAD" = True ]]; then
  echo "=================================================="
  printenv
  echo "=================================================="
fi

############################################################################
# Wait for Services
############################################################################

if [[ "$WAIT_FOR_DB" = true || "$WAIT_FOR_DB" = True ]]; then
  dockerize \
    -wait tcp://$DB_HOST:$DB_PORT \
    -timeout 300s
fi

############################################################################
# Migrate database
############################################################################

if [[ "$MIGRATE_DB" = true || "$MIGRATE_DB" = True ]]; then
  echo "++++++++++++++++++++++++++++++++++++++++++++++++++++++++"
  echo "Migrating Database"
  alembic -c db/alembic.ini upgrade head
  echo "++++++++++++++++++++++++++++++++++++++++++++++++++++++++"
fi

############################################################################
# Start App
############################################################################

case "$1" in
  chill)
    ;;
  *)
    echo "Running: $@"
    exec "$@"
    ;;
esac

echo ">>> Hello World!"
while true; do sleep 18000; done
```

**In Simple Terms:**
- This is the first script that runs when the container starts
- It does several important setup tasks:
  - It can display environment variables (useful for debugging)
  - It waits for the database to be available before proceeding
  - It updates the database structure if needed (using "migrations")
  - It either runs a specific command passed to it or just keeps the container alive

**Why It Matters:**
This script ensures that everything is properly set up before the application starts. It's like making sure all the equipment is working before opening a restaurant.

## 3. Setting Up the Environment: Workspace/dev_resources.py

```python
from os import getenv

from agno.docker.app.fastapi import FastApi
from agno.docker.app.postgres import PgVectorDb
from agno.docker.resource.image import DockerImage
from agno.docker.resources import DockerResources

from workspace.settings import ws_settings

# -*- Dev image
dev_image = DockerImage(
    name=f"{ws_settings.image_repo}/{ws_settings.image_name}",
    tag=ws_settings.dev_env,
    enabled=ws_settings.build_images,
    path=str(ws_settings.ws_root),
    # Do not push images after building
    push_image=ws_settings.push_images,
)

# -*- Dev database running on port 5432:5432
dev_db = PgVectorDb(
    name=f"{ws_settings.ws_name}-db",
    pg_user="ai",
    pg_password="ai",
    pg_database="ai",
    # Connect to this db on port 5432
    host_port=5432,
)

# -*- Container environment
container_env = {
    "RUNTIME_ENV": "dev",
    # Get the OpenAI API key and Exa API key from the local environment
    "OPENAI_API_KEY": getenv("OPENAI_API_KEY"),
    # Enable monitoring
    "AGNO_MONITOR": "True",
    "AGNO_API_KEY": getenv("AGNO_API_KEY"),
    # Database configuration
    "DB_HOST": dev_db.get_db_host(),
    "DB_PORT": dev_db.get_db_port(),
    "DB_USER": dev_db.get_db_user(),
    "DB_PASS": dev_db.get_db_password(),
    "DB_DATABASE": dev_db.get_db_database(),
    # Wait for database to be available before starting the application
    "WAIT_FOR_DB": dev_db.enabled,
    # Migrate database on startup using alembic
    "MIGRATE_DB": dev_db.enabled,
}

# -*- FastApi running on port 8000:8000
dev_fastapi = FastApi(
    name=f"{ws_settings.ws_name}-api",
    image=dev_image,
    command="uvicorn api.main:app --reload",
    port_number=8000,
    debug_mode=True,
    mount_workspace=True,
    env_vars=container_env,
    use_cache=True,
    # Read secrets from secrets/dev_api_secrets.yml
    secrets_file=ws_settings.ws_root.joinpath("workspace/secrets/dev_api_secrets.yml"),
    depends_on=[dev_db],
)

# -*- Dev DockerResources
dev_docker_resources = DockerResources(
    env=ws_settings.dev_env,
    network=ws_settings.ws_name,
    apps=[dev_db, dev_fastapi],
)
```

**In Simple Terms:**
- This file defines the development environment setup, specifically:
  - The Docker image to use
  - The database configuration (a PostgreSQL database with vector support)
  - Environment variables for the application (like API keys and database settings)
  - The FastAPI web server configuration (running on port 8000)
  - How all these components connect together

**Why It Matters:**
This configuration lets developers run the entire system on their computer consistently. It's like having a miniature version of the production environment on your laptop.

## 4. Setting Environment Variables: example.env

```
# AGNO_API_KEY=***
# OPENAI_API_KEY=sk-***
# EXA_API_KEY=***
```

**In Simple Terms:**
- This is a template showing what environment variables the application needs
- Developers copy this to a `.env` file and fill in their own API keys
- API keys are like passwords that let the application use services like OpenAI

**Why It Matters:**
By keeping API keys separate from the code and configuration, we:
1. Keep sensitive information secure
2. Allow each developer to use their own API keys
3. Can use different keys in different environments

## 5. Project Configuration: pyproject.toml

```toml
[project]
name = "astra"
version = "0.1.0"
description = "agno adventures"
readme = "README.md"
authors = [{ name = "Agno", email = "hello@agno.com" }]
requires-python = ">=3.12"

dependencies = [
  "agno[aws]",
  "aiofiles",
  "alembic",
  "beautifulsoup4",
  "duckduckgo-search",
  "exa_py",
  "fastapi[standard]",
  "googlesearch-python",
  "lxml_html_clean",
  "newspaper4k",
  "openai",
  "pgvector",
  "psycopg[binary]",
  "pycountry",
  "pypdf",
  "sqlalchemy",
  "streamlit",
  "tiktoken",
  "typer",
  "yfinance",
]

[dependency-groups]
dev = [
  "poethepoet",
  "pyright",
  "pytest",
  "ruff",
  "types-requests",
  "types-beautifulsoup4",
]

# ... more configuration for tools ...
```

**In Simple Terms:**
- This is the main configuration file for the Python project
- It defines:
  - Basic information (name, version, description)
  - What Python packages the project needs to run
  - Extra packages needed for development
  - Settings for various development tools (code formatters, linters, etc.)

**Why It Matters:**
This file makes the project easy to set up - when a new developer joins, they can install all the dependencies with a single command (`uv sync --all-packages`). It's like a shopping list for everything the project needs.

## 6. Starting the API: api/main.py

```python
from fastapi import FastAPI
from starlette.middleware.cors import CORSMiddleware

from api.routes.v1_router import v1_router
from api.settings import api_settings


def create_app() -> FastAPI:
    """Create a FastAPI App"""

    # Create FastAPI App
    app: FastAPI = FastAPI(
        title=api_settings.title,
        version=api_settings.version,
        docs_url="/docs" if api_settings.docs_enabled else None,
        redoc_url="/redoc" if api_settings.docs_enabled else None,
        openapi_url="/openapi.json" if api_settings.docs_enabled else None,
    )

    # Add v1 router
    app.include_router(v1_router)

    # Add Middlewares
    app.add_middleware(
        CORSMiddleware,
        allow_origins=api_settings.cors_origin_list,
        allow_credentials=True,
        allow_methods=["*"],
        allow_headers=["*"],
    )

    return app


# Create FastAPI app
app = create_app()
```

**In Simple Terms:**
- This file creates the web server (API) that receives and responds to requests
- It sets up:
  - The API title and version
  - Documentation pages (at `/docs` and `/redoc`)
  - The routes (URL paths) that the API will respond to
  - CORS settings (which websites are allowed to call this API)

**Why It Matters:**
This is the entry point for all requests to the application. When your blog post generation request comes in, it starts here.

## 7. API Configuration: api/settings.py

```python
from typing import List, Optional

from pydantic import Field, field_validator
from pydantic_core.core_schema import FieldValidationInfo
from pydantic_settings import BaseSettings


class ApiSettings(BaseSettings):
    """Api settings that can be set using environment variables."""

    # Api title and version
    title: str = "astra"
    version: str = "1.0"

    # Api runtime_env
    # Please set value to "dev", "stg" or "prd" in the container environment.
    runtime_env: str = "dev"

    # Set to False to disable docs at /docs and /redoc
    docs_enabled: bool = True

    # Cors origin list to allow requests from.
    cors_origin_list: Optional[List[str]] = Field(None, validate_default=True)

    @field_validator("cors_origin_list", mode="before")
    def set_cors_origin_list(cls, cors_origin_list, info: FieldValidationInfo):
        valid_cors = cors_origin_list or []

        # Add app.agno.com to cors to allow requests from the Agno playground.
        valid_cors.append("https://app.agno.com")
        # Add localhost to cors to allow requests from the local environment.
        valid_cors.append("http://localhost")
        # Add localhost:3000 to cors to allow requests from local Agent UI.
        valid_cors.append("http://localhost:3000")

        return valid_cors


# Create ApiSettings object
api_settings = ApiSettings()
```

**In Simple Terms:**
- This file defines settings for the API server
- It sets things like:
  - The API title and version
  - Whether documentation is enabled
  - Which websites can send requests to the API (CORS settings)
- It has smart defaults but can be overridden with environment variables

**Why It Matters:**
This centralized configuration makes it easy to change API settings without modifying code.

## 8. API Routes: api/routes/v1_router.py and Associated Files

```python
from fastapi import APIRouter

from api.routes.agents import agents_router
from api.routes.playground import playground_router
from api.routes.status import status_router

v1_router = APIRouter(prefix="/v1")
v1_router.include_router(status_router)
v1_router.include_router(agents_router)
v1_router.include_router(playground_router)
```

**In Simple Terms:**
- This file acts as a traffic controller for the API
- It sets up the main paths (routes) that the API responds to:
  - `/v1/status` for checking if the API is working
  - `/v1/agents` for interacting with agents
  - `/v1/playground` for testing agent functionality

**Why It Matters:**
This organized structure makes it easy to add new features without changing existing code.

## 9. Database Setup: db/session.py

```python
from typing import Generator

from sqlalchemy.engine import Engine, create_engine
from sqlalchemy.orm import Session, sessionmaker

from db.settings import db_settings

# Create SQLAlchemy Engine using a database URL
db_url: str = db_settings.get_db_url()
db_engine: Engine = create_engine(db_url, pool_pre_ping=True)

# Create a SessionLocal class
SessionLocal: sessionmaker[Session] = sessionmaker(autocommit=False, autoflush=False, bind=db_engine)


def get_db() -> Generator[Session, None, None]:
    """
    Dependency to get a database session.

    Yields:
        Session: An SQLAlchemy database session.
    """
    db: Session = SessionLocal()
    try:
        yield db
    finally:
        db.close()
```

**In Simple Terms:**
- This file sets up the connection to the database
- It creates:
  - A database engine that knows how to talk to the database
  - A session maker that creates database sessions
  - A function that provides database sessions to other parts of the code

**Why It Matters:**
This ensures efficient and safe database connections. When handling your blog post request, this is how the application reads and writes data to the database.

## 10. Database Configuration: db/settings.py

```python
from os import getenv
from typing import Optional

from pydantic_settings import BaseSettings


class DbSettings(BaseSettings):
    """Database settings that can be set using environment variables."""

    # Database configuration
    db_host: Optional[str] = None
    db_port: Optional[int] = None
    db_user: Optional[str] = None
    db_pass: Optional[str] = None
    db_database: Optional[str] = None
    db_driver: str = "postgresql+psycopg"
    # Create/Upgrade database on startup using alembic
    migrate_db: bool = False

    def get_db_url(self) -> str:
        db_url = "{}://{}{}@{}:{}/{}".format(
            self.db_driver,
            self.db_user,
            f":{self.db_pass}" if self.db_pass else "",
            self.db_host,
            self.db_port,
            self.db_database,
        )
        # Use local database if RUNTIME_ENV is not set
        if "None" in db_url and getenv("RUNTIME_ENV") is None:
            from workspace.dev_resources import dev_db

            local_db_url = dev_db.get_db_connection_local()
            if local_db_url:
                db_url = local_db_url

        # Validate database connection
        if "None" in db_url or db_url is None:
            raise ValueError("Could not build database connection")
        return db_url


# Create DbSettings object
db_settings = DbSettings()
```

**In Simple Terms:**
- This file defines the settings for connecting to the database
- It includes the database host, port, username, password, and database name
- It has a smart function to create the database connection URL
- If running locally (not in a container), it uses local database settings

**Why It Matters:**
This allows the application to connect to different databases in different environments without changing code.

## 11. Logging Setup: utils/log.py

```python
import logging

from rich.logging import RichHandler


def get_logger(logger_name: str) -> logging.Logger:
    # https://rich.readthedocs.io/en/latest/reference/logging.html#rich.logging.RichHandler
    rich_handler = RichHandler(
        show_time=False,
        rich_tracebacks=False,
        show_path=True,
        tracebacks_show_locals=False,
    )
    rich_handler.setFormatter(
        logging.Formatter(
            fmt="%(message)s",
            datefmt="[%X]",
        )
    )

    _logger = logging.getLogger(logger_name)
    _logger.addHandler(rich_handler)
    _logger.setLevel(logging.DEBUG)
    _logger.propagate = False
    return _logger


logger: logging.Logger = get_logger("agent-app")
```

**In Simple Terms:**
- This file sets up logging (how the application records what it's doing)
- It creates a pretty, colorful log output using the "rich" library
- It configures what information is included in log messages

**Why It Matters:**
Good logging is critical for understanding what's happening in the application, especially when debugging problems.

## 12. Utility Functions: utils/dttm.py

```python
from datetime import datetime, timezone


def get_current_datetime_utc() -> str:
    """Get the current datetime in UTC as an ISO format string."""
    return datetime.now(timezone.utc).isoformat()
```

**In Simple Terms:**
- This file contains utilities for working with dates and times
- It has a function to get the current time in UTC format

**Why It Matters:**
Having a consistent way to get and format timestamps makes it easier to track when things happen.

## 13. Agent Definition: agents/sage.py

```python
from textwrap import dedent
from typing import Optional

from agno.agent import Agent, AgentKnowledge
from agno.models.openai import OpenAIChat
from agno.storage.agent.postgres import PostgresAgentStorage
from agno.tools.duckduckgo import DuckDuckGoTools
from agno.vectordb.pgvector import PgVector, SearchType

from db.session import db_url


def get_sage(
    model_id: str = "gpt-4.1",
    user_id: Optional[str] = None,
    session_id: Optional[str] = None,
    debug_mode: bool = True,
) -> Agent:
    additional_context = ""
    if user_id:
        additional_context += "<context>"
        additional_context += f"You are interacting with the user: {user_id}"
        additional_context += "</context>"

    return Agent(
        name="Sage",
        agent_id="sage",
        user_id=user_id,
        session_id=session_id,
        model=OpenAIChat(id=model_id),
        # Tools available to the agent
        tools=[DuckDuckGoTools()],
        # Storage for the agent
        storage=PostgresAgentStorage(table_name="sage_sessions", db_url=db_url),
        # Knowledge base for the agent
        knowledge=AgentKnowledge(
            vector_db=PgVector(table_name="sage_knowledge", db_url=db_url, search_type=SearchType.hybrid)
        ),
        # Description of the agent
        description=dedent("""\
            You are Sage, an advanced Knowledge Agent designed to deliver accurate, context-rich, engaging responses.
            You have access to a knowledge base full of user-provided information and the capability to search the web if needed.

            Your responses should be clear, concise, and supported by citations from the knowledge base and/or the web.\
        """),
        # Instructions for the agent
        instructions=dedent("""\
            Respond to the user by following the steps below:

            1. Always search your knowledge base for relevant information
            - First, analyze the user's message and identify 1-3 precise search terms to search your knowledge base.
            - Then, search your knowledge base for relevant information using the `search_knowledge_base` tool.
            - Note: You must always search your knowledge base unless you are sure that the user's query is not related to the knowledge base.

            2. Search the web if no relevant information is found in your knowledge base
            - If knowledge base search yields insufficient results, use the `duckduckgo_search` tool to find relevant information from the web.
            - Focus on reputable sources and recent information.
            - Cross-reference information from multiple sources when possible.

            3. Memory & Context Management:
            - You will be provided the last 3 messages from the chat history.
            - If needed, use the `get_chat_history` tool to retrieve more messages from the chat history.
            - Reference previous interactions when relevant and maintain conversation continuity.
            - Keep track of user preferences and prior clarifications.

            4. Construct Your Response
            - **Start** with a succinct, clear and direct answer that immediately addresses the user's query.
            - **Then expand** the answer by including:
                - A clear explanation with context and definitions.
                - Supporting evidence such as statistics, real-world examples, and data points.
                - Clarifications that address common misconceptions.
            - Expand the answer only if the query requires more detail. Simple questions like: "What is the weather in Tokyo?" or "What is the capital of France?" don't need an in-depth analysis.
            - Ensure the response is structured so that it provides quick answers as well as in-depth analysis for further exploration.
            - Avoid hedging phrases like 'based on my knowledge' or 'depending on the information'
            - Always include citations from the knowledge base and/or the web.

            5. Enhance Engagement
            - After generating your answer, ask the user follow-up questions and suggest related topics to explore.

            6. Final Quality Check & Presentation ✨
            - Review your response to ensure clarity, depth, and engagement.
            - Strive to be both informative for quick queries and thorough for detailed exploration.

            7. In case of any uncertainties, clarify limitations and encourage follow-up queries.\
        """),
        additional_context=additional_context,
        # Format responses using markdown
        markdown=True,
        # Add the current date and time to the instructions
        add_datetime_to_instructions=True,
        # Send the last 3 messages from the chat history
        add_history_to_messages=True,
        num_history_responses=3,
        # Add a tool to read the chat history if needed
        read_chat_history=True,
        # Show debug logs
        debug_mode=debug_mode,
    )
```

**In Simple Terms:**
- This file defines the "Sage" agent, which is a knowledge-focused AI assistant
- It sets up:
  - What AI model to use (GPT-4.1 by default)
  - What tools the agent can use (DuckDuckGo for web search)
  - How to store conversation history (in a PostgreSQL database)
  - A knowledge base for the agent (using vector embeddings)
  - Detailed instructions for how the agent should respond to questions

**Why It Matters:**
This configuration determines how the agent behaves, what information it has access to, and how it formats its responses. For your blog post request, the agents will follow these instructions to research and write content.

## 14. Agent Factory: agents/operator.py

```python
from enum import Enum
from typing import List, Optional

from agents.sage import get_sage
from agents.scholar import get_scholar


class AgentType(Enum):
    SAGE = "sage"
    SCHOLAR = "scholar"


def get_available_agents() -> List[str]:
    """Returns a list of all available agent IDs."""
    return [agent.value for agent in AgentType]


def get_agent(
    model_id: str = "gpt-4.1",
    agent_id: Optional[AgentType] = None,
    user_id: Optional[str] = None,
    session_id: Optional[str] = None,
    debug_mode: bool = True,
):
    if agent_id == AgentType.SAGE:
        return get_sage(model_id=model_id, user_id=user_id, session_id=session_id, debug_mode=debug_mode)
    else:
        return get_scholar(model_id=model_id, user_id=user_id, session_id=session_id, debug_mode=debug_mode)
```

**In Simple Terms:**
- This file is a factory that creates different types of agents
- It defines:
  - An enumeration of available agent types (SAGE and SCHOLAR)
  - A function to list all available agent types
  - A function to create an agent of a specific type

**Why It Matters:**
This factory pattern makes it easy to add new agent types without changing the code that uses them.

## 15. Team Definition: teams/finance_researcher_team.py

```python
from textwrap import dedent

from agno.agent import Agent
from agno.models.openai import OpenAIChat
from agno.storage.postgres import PostgresStorage
from agno.team.team import Team
from agno.tools.duckduckgo import DuckDuckGoTools
from agno.tools.yfinance import YFinanceTools

from db.session import db_url
from teams.settings import team_settings

finance_agent = Agent(
    name="Finance Agent",
    role="Analyze financial data",
    agent_id="finance-agent",
    model=OpenAIChat(
        id=team_settings.gpt_4,
        max_tokens=team_settings.default_max_completion_tokens,
        temperature=team_settings.default_temperature,
    ),
    tools=[YFinanceTools(enable_all=True, cache_results=True)],
    instructions=dedent("""\
        You are a seasoned Wall Street analyst with deep expertise in market analysis! 📊

        Follow these steps for comprehensive financial analysis:
        1. Market Overview
        - Latest stock price
        - 52-week high and low
        2. Financial Deep Dive
        - Key metrics (P/E, Market Cap, EPS)
        3. Professional Insights
        - Analyst recommendations breakdown
        - Recent rating changes

        4. Market Context
        - Industry trends and positioning
        - Competitive analysis
        - Market sentiment indicators

        Your reporting style:
        - Begin with an executive summary
        - Use tables for data presentation
        - Include clear section headers
        - Add emoji indicators for trends (📈 📉)
        - Highlight key insights with bullet points
        - Compare metrics to industry averages
        - Include technical term explanations
        - End with a forward-looking analysis

        Risk Disclosure:
        - Always highlight potential risk factors
        - Note market uncertainties
        - Mention relevant regulatory concerns
    """),
    storage=PostgresStorage(table_name="finance_agent", db_url=db_url, auto_upgrade_schema=True),
    add_history_to_messages=True,
    num_history_responses=5,
    add_datetime_to_instructions=True,
    markdown=True,
)

web_agent = Agent(
    name="Web Agent",
    role="Search the web for information",
    model=OpenAIChat(id=team_settings.gpt_4),
    tools=[DuckDuckGoTools(cache_results=True)],
    agent_id="web-agent",
    instructions=[
        "You are an experienced web researcher and news analyst!",
    ],
    show_tool_calls=True,
    markdown=True,
    storage=PostgresStorage(table_name="web_agent", db_url=db_url, auto_upgrade_schema=True),
)


def get_finance_researcher_team(debug_mode: bool = False):
    return Team(
        name="Finance Researcher Team",
        team_id="financial-researcher-team",
        mode="route",
        members=[web_agent, finance_agent],
        instructions=[
            "You are a team of finance researchers!",
        ],
        description="You are a team of finance researchers!",
        model=OpenAIChat(id=team_settings.gpt_4),
        success_criteria="A good financial research report.",
        enable_agentic_context=True,
        expected_output="A good financial research report.",
        storage=PostgresStorage(
            table_name="finance_researcher_team",
            db_url=db_url,
            mode="team",
            auto_upgrade_schema=True,
        ),
        debug_mode=debug_mode,
    )
```

**In Simple Terms:**
- This file defines a team of agents that work together to research financial topics
- It creates:
  - A finance agent with tools for analyzing financial data
  - A web agent for searching the internet
  - A team that coordinates these agents

**Why It Matters:**
Teams allow specialized agents to work together, each focusing on what they do best. This is similar to how the blog post generator coordinates multiple agents for content creation.

## 16. Workflow Definition: workflows/blog_post_generator.py

```python
"""🎨 Blog Post Generator - Your AI Content Creation Studio!

This advanced example demonstrates how to build a sophisticated blog post generator that combines
web research capabilities with professional writing expertise. The workflow uses a multi-stage
approach:
1. Intelligent web research and source gathering
2. Content extraction and processing
3. Professional blog post writing with proper citations

Key capabilities:
- Advanced web research and source evaluation
- Content scraping and processing
- Professional writing with SEO optimization
- Automatic content caching for efficiency
- Source attribution and fact verification
"""

import json
from textwrap import dedent
from typing import Dict, Iterator, Optional

from agno.agent import Agent
from agno.models.openai import OpenAIChat
from agno.storage.workflow.postgres import PostgresWorkflowStorage
from agno.tools.duckduckgo import DuckDuckGoTools
from agno.tools.newspaper4k import Newspaper4kTools
from agno.utils.log import logger
from agno.workflow import RunEvent, RunResponse, Workflow
from pydantic import BaseModel, Field

from db.session import db_url
from workflows.settings import workflow_settings


class NewsArticle(BaseModel):
    title: str = Field(..., description="Title of the article.")
    url: str = Field(..., description="Link to the article.")
    summary: Optional[str] = Field(..., description="Summary of the article if available.")


class SearchResults(BaseModel):
    articles: list[NewsArticle]


class ScrapedArticle(BaseModel):
    title: str = Field(..., description="Title of the article.")
    url: str = Field(..., description="Link to the article.")
    summary: Optional[str] = Field(..., description="Summary of the article if available.")
    content: Optional[str] = Field(
        ...,
        description="Full article content in markdown format. None if content is unavailable.",
    )


class BlogPostGenerator(Workflow):
    """Advanced workflow for generating professional blog posts with proper research and citations."""

    description: str = dedent("""\
    An intelligent blog post generator that creates engaging, well-researched content.
    This workflow orchestrates multiple AI agents to research, analyze, and craft
    compelling blog posts that combine journalistic rigor with engaging storytelling.
    The system excels at creating content that is both informative and optimized for
    digital consumption.
    """)

    # Search Agent: Handles intelligent web searching and source gathering
    searcher: Agent = Agent(
        model=OpenAIChat(id=workflow_settings.gpt_4_mini),
        tools=[DuckDuckGoTools()],
        description=dedent("""\
        You are BlogResearch-X, an elite research assistant specializing in discovering
        high-quality sources for compelling blog content. Your expertise includes:

        - Finding authoritative and trending sources
        - Evaluating content credibility and relevance
        - Identifying diverse perspectives and expert opinions
        - Discovering unique angles and insights
        - Ensuring comprehensive topic coverage\
        """),
        instructions=dedent("""\
        1. Search Strategy 🔍
           - Find 10-15 relevant sources and select the 5-7 best ones
           - Prioritize recent, authoritative content
           - Look for unique angles and expert insights
        2. Source Evaluation 📊
           - Verify source credibility and expertise
           - Check publication dates for timeliness
           - Assess content depth and uniqueness
        3. Diversity of Perspectives 🌐
           - Include different viewpoints
           - Gather both mainstream and expert opinions
           - Find supporting data and statistics\
        """),
        response_model=SearchResults,
        structured_outputs=True,
    )

    # Content Scraper: Extracts and processes article content
    article_scraper: Agent = Agent(
        model=OpenAIChat(id=workflow_settings.gpt_4_mini),
        tools=[Newspaper4kTools()],
        description=dedent("""\
        You are ContentBot-X, a specialist in extracting and processing digital content
        for blog creation. Your expertise includes:

        - Efficient content extraction
        - Smart formatting and structuring
        - Key information identification
        - Quote and statistic preservation
        - Maintaining source attribution\
        """),
        instructions=dedent("""\
        1. Content Extraction 📑
           - Extract content from the article
           - Preserve important quotes and statistics
           - Maintain proper attribution
           - Handle paywalls gracefully
        2. Content Processing 🔄
           - Format text in clean markdown
           - Preserve key information
           - Structure content logically
        3. Quality Control ✅
           - Verify content relevance
           - Ensure accurate extraction
           - Maintain readability\
        """),
        response_model=ScrapedArticle,
        structured_outputs=True,
    )

    # Content Writer Agent: Crafts engaging blog posts from research
    writer: Agent = Agent(
        model=OpenAIChat(id=workflow_settings.gpt_4_mini),
        description=dedent("""\
        You are BlogMaster-X, an elite content creator combining journalistic excellence
        with digital marketing expertise. Your strengths include:

        - Crafting viral-worthy headlines
        - Writing engaging introductions
        - Structuring content for digital consumption
        - Incorporating research seamlessly
        - Optimizing for SEO while maintaining quality
        - Creating shareable conclusions\
        """),
        instructions=dedent("""\
        1. Content Strategy 📝
           - Craft attention-grabbing headlines
           - Write compelling introductions
           - Structure content for engagement
           - Include relevant subheadings
        2. Writing Excellence ✍️
           - Balance expertise with accessibility
           - Use clear, engaging language
           - Include relevant examples
           - Incorporate statistics naturally
        3. Source Integration 🔍
           - Cite sources properly
           - Include expert quotes
           - Maintain factual accuracy
        4. Digital Optimization 💻
           - Structure for scanability
           - Include shareable takeaways
           - Optimize for SEO
           - Add engaging subheadings\
        """),
        expected_output=dedent("""\
        # {Viral-Worthy Headline}

        ## Introduction
        {Engaging hook and context}

        ## {Compelling Section 1}
        {Key insights and analysis}
        {Expert quotes and statistics}

        ## {Engaging Section 2}
        {Deeper exploration}
        {Real-world examples}

        ## {Practical Section 3}
        {Actionable insights}
        {Expert recommendations}

        ## Key Takeaways
        - {Shareable insight 1}
        - {Practical takeaway 2}
        - {Notable finding 3}

        ## Sources
        """),
    )

    def run(
        self,
        topic: str,
        use_search_cache: bool = True,
        use_scrape_cache: bool = True,
        use_cached_report: bool = True,
    ) -> Iterator[RunResponse]:
        """Run the blog post generation workflow.

        Args:
            topic: The blog post topic to generate content for
            use_search_cache: Whether to use cached search results
            use_scrape_cache: Whether to use cached article content
            use_cached_report: Whether to use a cached blog post if available

        Yields:
            RunResponse: Updates as the workflow progresses
        """
        # ... implementation details ...
```

**In Simple Terms:**
- This file defines a workflow for generating blog posts
- It creates three specialized agents:
  1. A searcher that finds relevant articles on the topic
  2. A scraper that extracts content from those articles
  3. A writer that creates a well-structured blog post from the research
- The workflow coordinates these agents to:
  1. Search for sources about the topic
  2. Extract content from those sources
  3. Generate a complete blog post with proper citations

**Why It Matters:**
This workflow shows how to break down a complex task (blog post generation) into manageable steps handled by specialized agents. For your "AI in Healthcare" request, this workflow would:
1. Search for recent, credible sources about AI in healthcare
2. Extract and process content from those sources
3. Write a comprehensive, well-structured blog post on the topic

## 17. Workflow Settings: workflows/settings.py

```python
from pathlib import Path
from typing import Optional

from pydantic_settings import BaseSettings


class WorkflowSettings(BaseSettings):
    """Configuration settings for workflows"""

    # ID of the GPT-4 model to use
    gpt_4: str = "gpt-4.1"
    # ID of the GPT-4 mini model to use
    gpt_4_mini: str = "gpt-4-mini"


# Create WorkflowSettings object
workflow_settings = WorkflowSettings()
```

**In Simple Terms:**
- This file defines settings used by all workflows
- It specifies which AI models to use:
  - A full GPT-4.1 model for complex tasks
  - A smaller, faster GPT-4-mini model for simpler tasks

**Why It Matters:**
Centralizing these settings makes it easy to update models or configurations for all workflows at once.

## Our Complete Example: Generating a Blog Post on "AI in Healthcare"

Now that we've explored all the components, let's trace how your request to generate a blog post about "AI in Healthcare" would flow through the system:

1. **Request Received**: The API receives a POST request to `/v1/workflows/blog-post-generator/runs` with the topic "AI in Healthcare"


2. **API Processing**:
   - `api/main.py` receives the request and routes it to the appropriate handler
   - `api/routes/workflows.py` processes the request and calls the workflow

3. **Workflow Orchestration**:
   - `workflows/blog_post_generator.py` creates a new workflow run
   - It first checks if a cached blog post exists for this topic
   - If not, it begins the multi-step process

4. **Research Phase**:
   - The searcher agent (`BlogResearch-X`) is activated
   - It uses the `DuckDuckGoTools` from the Agno framework to search for sources
   - It evaluates sources based on relevance, credibility, and recency
   - It returns a structured list of the 5-7 best articles about AI in healthcare

5. **Content Extraction Phase**:
   - For each article found, the article_scraper agent (`ContentBot-X`) is activated
   - It uses the `Newspaper4kTools` to extract content from each webpage
   - It cleans and formats the extracted content
   - It preserves important quotes, statistics, and attributions
   - It returns a structured representation of each article's content

6. **Content Creation Phase**:
   - The writer agent (`BlogMaster-X`) is activated
   - It analyzes all the extracted content
   - It identifies key themes, insights, and perspectives
   - It drafts a complete blog post with:
     - An attention-grabbing headline
     - An engaging introduction
     - Well-structured sections with subheadings
     - Expert quotes and statistics
     - Practical takeaways
     - Proper source citations

7. **Database Storage**:
   - The completed blog post is stored in the database using `db/session.py`
   - The workflow state and results are saved using `PostgresWorkflowStorage`
   - This allows for caching and retrieval of the blog post for future requests

8. **Response Delivery**:
   - The API streams the generated blog post back to the user
   - Progress updates may be sent during the generation process

Throughout this process, various utilities and configurations are used:

- **Logging**: `utils/log.py` records each step and any issues
- **Environment Variables**: Values from `.env` are used for API keys
- **Date/Time Utilities**: `utils/dttm.py` timestamps various operations

## The Output: A Sample Blog Post

The result would be a comprehensive blog post structured like this:

```markdown
# The Revolutionary Impact of AI in Healthcare: Transforming Patient Care in 2024

## Introduction
Artificial Intelligence is revolutionizing healthcare delivery, diagnosis, and treatment planning across the medical field. From powering early disease detection to optimizing hospital operations, AI technologies are creating new possibilities for improved patient outcomes while addressing critical healthcare challenges.

## How AI is Transforming Medical Diagnosis
Recent breakthroughs in medical imaging AI have demonstrated capabilities that match or exceed human specialists in detecting conditions like cancer and diabetic retinopathy. According to a 2023 study published in the New England Journal of Medicine, "AI-assisted diagnosis improved detection rates by 29% while reducing false positives by 8% compared to traditional methods" (Johnson et al., 2023).

The Mayo Clinic has implemented AI systems that analyze thousands of retinal images to detect diabetic retinopathy with 97% accuracy, allowing for earlier intervention and treatment.

## AI-Powered Personalized Treatment Plans
Healthcare AI is enabling truly personalized medicine by analyzing vast datasets of patient information. "Machine learning algorithms can now predict patient responses to specific treatments with 85% accuracy by analyzing genetic markers, medical history, and lifestyle factors," notes Dr. Sarah Chen, Director of AI Health Initiatives at Stanford (Chen, 2023).

Companies like Tempus are combining AI analysis with genetic sequencing to develop precision oncology treatments tailored to individual cancer patients, dramatically improving response rates for traditionally difficult-to-treat cancers.

## Ethical Considerations and Implementation Challenges
Despite promising advances, the healthcare industry faces significant challenges in AI adoption. Privacy concerns, algorithm bias, and integration with existing systems remain critical hurdles.

Dr. Michael Roberts from Harvard Medical School emphasizes, "We must ensure AI systems are trained on diverse datasets that represent all populations to avoid perpetuating healthcare disparities" (Healthcare AI Summit, 2024).

Additionally, healthcare providers must navigate the complex regulatory landscape surrounding AI medical devices, with the FDA developing new frameworks for AI/ML-based Software as a Medical Device (SaMD).

## Key Takeaways
- AI diagnostic tools are matching or exceeding specialist performance in certain medical imaging applications
- Personalized medicine is becoming reality through AI analysis of patient data
- Ethical implementation requires addressing bias, privacy, and regulatory challenges
- Healthcare AI adoption is expected to grow by 38% annually through 2027

## Sources
- Johnson, T. et al. (2023). "Comparative Analysis of AI-Assisted Diagnostic Platforms." New England Journal of Medicine.
- Chen, S. (2023). "The Future of Personalized Medicine." Stanford AI Health Initiative.
- Healthcare AI Summit Proceedings. (2024). Harvard Medical School.
- FDA. (2023). "Artificial Intelligence and Machine Learning in Software as a Medical Device."
- McKinsey Global Institute. (2023). "The Economic Impact of AI in Healthcare."
```

## How This All Works Together

The beauty of the Astra system is how it separates concerns while maintaining consistency:

1. **Configuration**: Through `pyproject.toml`, `.env`, and various settings files, the system is highly configurable without code changes

2. **Infrastructure**: Docker and workspace configuration ensure consistent environments for development and production

3. **Database**: The PostgreSQL database with pgvector provides powerful storage for both regular data and vector embeddings

4. **API Layer**: FastAPI provides a modern, high-performance interface for external communication

5. **Agent Layer**: Well-defined agents with clear responsibilities handle specific tasks

6. **Workflow Layer**: Complex processes are broken down into manageable steps coordinated by workflows

7. **Utilities**: Common functions are centralized for consistency and reusability

This modular design makes the system:
- **Maintainable**: Changes to one component don't affect others
- **Extensible**: New agents, workflows, or features can be added easily
- **Scalable**: Components can be scaled independently as needed
- **Reliable**: Issues in one area don't crash the entire system

For a beginner, understanding this structure provides a solid foundation for modern AI application development. While the individual components may seem complex at first, they each serve a clear purpose in the overall system. As you build your own applications, you can start with simpler versions of these components and gradually add complexity as needed.
