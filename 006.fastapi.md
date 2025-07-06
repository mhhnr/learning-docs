# Complete FastAPI Fundamentals Guide

## Table of Contents
1. [Introduction to FastAPI](#introduction-to-fastapi)
2. [Getting Started](#getting-started)
3. [Path Operations (Routes)](#path-operations-routes)
4. [Path Parameters](#path-parameters)
5. [Query Parameters](#query-parameters)
6. [Request Body](#request-body)
7. [Response Models](#response-models)
8. [Data Validation](#data-validation)
9. [Dependency Injection](#dependency-injection)
10. [Authentication and Authorization](#authentication-and-authorization)
11. [Database Integration](#database-integration)
12. [Background Tasks](#background-tasks)
13. [Middleware](#middleware)
14. [CORS (Cross-Origin Resource Sharing)](#cors-cross-origin-resource-sharing)
15. [Testing](#testing)
16. [Deployment](#deployment)
17. [Best Practices](#best-practices)
18. [Real-World Applications](#real-world-applications)

## Introduction to FastAPI

FastAPI is a modern, high-performance web framework for building APIs with Python. Created by Sebastián Ramírez and first released in 2018, FastAPI has quickly gained popularity due to its speed, ease of use, and automatic documentation generation.

### Key Features:

- **Fast**: Very high performance, on par with NodeJS and Go
- **Easy**: Designed to be easy to use and learn
- **Pythonic**: Based on standard Python type hints
- **Validation**: Automatic data validation
- **Documentation**: Automatic interactive API documentation
- **Standards-based**: Based on open standards OpenAPI and JSON Schema

### Why Companies Use FastAPI:

1. **Performance**: FastAPI is built on Starlette and Pydantic, making it one of the fastest Python frameworks available.
2. **Developer Productivity**: Automatic validation and documentation save development time.
3. **Type Safety**: Built-in type checking helps catch errors before runtime.
4. **Modern Features**: Support for async/await, WebSockets, GraphQL, etc.
5. **Microservices**: Ideal for building microservices architectures.
6. **Machine Learning Deployment**: Efficiently serves ML models as APIs.

```python
# Hello World in FastAPI
from fastapi import FastAPI

# Create a FastAPI instance
app = FastAPI()

# Define a path operation (route)
@app.get("/")  # Decorator specifies HTTP method and path
def read_root():
    # This function will be called when a client requests the root path "/"
    return {"Hello": "World"}  # Return a JSON response
```

## Getting Started

### Installation

First, you need to install FastAPI and an ASGI server like Uvicorn:

```python
# Installation commands (run in terminal)
# pip install fastapi
# pip install uvicorn[standard]

# Basic FastAPI application
from fastapi import FastAPI

# Create a FastAPI application instance
app = FastAPI(
    title="My API",  # API title for documentation
    description="This is a sample FastAPI application",  # API description
    version="0.1.0",  # API version
)

# Define a simple route
@app.get("/")
def read_root():
    return {"message": "Welcome to FastAPI!"}

# Run the server (in terminal):
# uvicorn main:app --reload
# What is Uvicorn?
# Uvicorn is an ASGI (Asynchronous Server Gateway Interface) web server implementation for Python. In simpler terms, it's the software that actually runs your FastAPI application and handles web requests. It acts as the bridge between your FastAPI code and the internet, receiving HTTP requests and sending responses back.
# main: the Python file name (without .py)
# app: the FastAPI instance name
# --reload: auto-reload when code changes (for development)
```

### Accessing Documentation

FastAPI automatically generates interactive documentation:

- **Swagger UI**: Available at `/docs` endpoint (e.g., http://localhost:8000/docs)
- **ReDoc**: Available at `/redoc` endpoint (e.g., http://localhost:8000/redoc)

```python
# You can customize the docs URL or disable documentation
app = FastAPI(
    docs_url="/documentation",  # Change Swagger UI URL
    redoc_url=None  # Disable ReDoc
)
```

## Path Operations (Routes)

Path operations (often called routes) define how your API responds to different HTTP methods and paths.

```python
from fastapi import FastAPI

app = FastAPI()

# Different HTTP methods for the same path
@app.get("/items")  # HTTP GET
def read_items():
    # This function handles GET requests to /items
    return {"items": ["item1", "item2"]}

@app.post("/items")  # HTTP POST
def create_item():
    # This function handles POST requests to /items
    return {"message": "Item created"}

@app.put("/items/{item_id}")  # HTTP PUT
def update_item(item_id: int):
    # This function handles PUT requests to /items/{item_id}
    return {"item_id": item_id, "message": "Item updated"}

@app.delete("/items/{item_id}")  # HTTP DELETE
def delete_item(item_id: int):
    # This function handles DELETE requests to /items/{item_id}
    return {"item_id": item_id, "message": "Item deleted"}

# HTTP methods correspond to CRUD operations:
# POST: Create
# GET: Read
# PUT/PATCH: Update
# DELETE: Delete
```

## Path Parameters

Path parameters are variable parts of a URL path.

```python
from fastapi import FastAPI

app = FastAPI()

# Single path parameter
@app.get("/items/{item_id}")
def read_item(item_id: int):  # Path parameter with type annotation
    # FastAPI will convert the path parameter to the specified type
    # If conversion fails, it returns a validation error
    return {"item_id": item_id}

# Multiple path parameters
@app.get("/users/{user_id}/items/{item_id}")
def read_user_item(user_id: int, item_id: str):
    # Multiple path parameters with different types
    return {"user_id": user_id, "item_id": item_id}

# Path parameter with Enum
from enum import Enum

class ModelName(str, Enum):
    # Define valid options as enum
    alexnet = "alexnet"
    resnet = "resnet"
    lenet = "lenet"

@app.get("/models/{model_name}")
def get_model(model_name: ModelName):
    # Path parameter validated against enum values
    if model_name == ModelName.alexnet:
        return {"model_name": model_name, "message": "Deep Learning FTW!"}
    
    if model_name.value == "lenet":
        return {"model_name": model_name, "message": "LeCNN all the images"}
    
    return {"model_name": model_name, "message": "Have some residuals"}
```

## Query Parameters

Query parameters are key-value pairs added to the URL after a question mark.

```python
from fastapi import FastAPI
from typing import Optional

app = FastAPI()

# Optional query parameters
@app.get("/items/")
def read_items(skip: int = 0, limit: int = 10, q: Optional[str] = None):
    # skip and limit have default values
    # q is optional (None if not provided)
    results = {"skip": skip, "limit": limit}
    if q:
        results.update({"q": q})
    return results

# Required query parameters
@app.get("/items/{item_id}")
def read_item(item_id: str, needy: str):
    # needy is a required query parameter (no default value)
    return {"item_id": item_id, "needy": needy}

# Query parameter validation
from fastapi import Query

@app.get("/products/")
def read_products(
    q: Optional[str] = Query(
        None,  # Default value
        min_length=3,  # Minimum length validation
        max_length=50,  # Maximum length validation
        regex="^[a-zA-Z0-9_-]*$",  # Regex pattern validation
        title="Query string",  # Title for docs
        description="Query string for searching products",  # Description for docs
    )
):
    results = {"products": ["Product1", "Product2"]}
    if q:
        results.update({"q": q})
    return results
```

## Request Body

Request bodies are data sent by the client to the API.

```python
from fastapi import FastAPI
from pydantic import BaseModel
from typing import Optional

app = FastAPI()

# Define a Pydantic model for the request body
class Item(BaseModel):
    name: str  # Required field
    description: Optional[str] = None  # Optional field with default value
    price: float  # Required field
    tax: Optional[float] = None  # Optional field with default value

# Endpoint with request body
@app.post("/items/")
def create_item(item: Item):
    # FastAPI will:
    # - Read the request body as JSON
    # - Convert it to the Item model
    # - Validate the data
    # - Make it available in the `item` parameter
    
    # You can access model attributes directly
    item_dict = item.dict()  # Convert model to dictionary
    if item.tax:
        price_with_tax = item.price + item.tax
        item_dict.update({"price_with_tax": price_with_tax})
    return item_dict

# Request body + path parameters
@app.put("/items/{item_id}")
def update_item(item_id: int, item: Item):
    # Combine path parameters and request body
    return {"item_id": item_id, **item.dict()}

# Request body + path parameters + query parameters
@app.put("/items/{item_id}")
def update_item(item_id: int, item: Item, q: Optional[str] = None):
    result = {"item_id": item_id, **item.dict()}
    if q:
        result.update({"q": q})
    return result
```

## Response Models

Response models define the structure of the response data.

```python
from fastapi import FastAPI
from pydantic import BaseModel
from typing import List, Optional

app = FastAPI()

# Define Pydantic models for responses
class ItemBase(BaseModel):
    name: str
    description: Optional[str] = None
    price: float
    tax: Optional[float] = None

class ItemOut(ItemBase):
    # Output model may contain additional fields
    id: int
    rating: Optional[float] = None

# Use response_model parameter to define response structure
@app.post("/items/", response_model=ItemOut)
def create_item(item: ItemBase):
    # FastAPI will:
    # - Take the returned dict/model
    # - Validate it against the response_model
    # - Filter out extra data
    # - Convert it to JSON
    
    # Simulate creating an item in database
    return {
        **item.dict(),  # Include input data
        "id": 1,  # Add id field
        "rating": 4.5,  # Add rating field
        "secret_data": "you won't see this"  # This will be filtered out
    }

# List of items response
class ItemList(BaseModel):
    items: List[ItemOut]
    count: int

@app.get("/items/", response_model=ItemList)
def read_items():
    # Return a list of items
    items = [
        {
            "name": "Item 1",
            "price": 50.2,
            "id": 1,
            "rating": 4.5
        },
        {
            "name": "Item 2",
            "price": 30,
            "id": 2,
            "description": "A nice item",
            "tax": 3.2
        }
    ]
    return {"items": items, "count": len(items)}
```

## Data Validation

FastAPI provides extensive data validation capabilities through Pydantic models.

```python
from fastapi import FastAPI, Path, Query, Body
from pydantic import BaseModel, Field, validator
from typing import List, Optional
from datetime import datetime
import re

app = FastAPI()

# Basic validation with Pydantic model
class Item(BaseModel):
    name: str = Field(..., min_length=1, max_length=50)  # Required, length 1-50
    price: float = Field(..., gt=0)  # Required, greater than 0
    is_offer: Optional[bool] = None  # Optional boolean
    
    # Custom validator method
    @validator('name')
    def name_must_be_alphanumeric(cls, v):
        if not re.match(r'^[a-zA-Z0-9 ]+$', v):
            raise ValueError('must be alphanumeric')
        return v.title()  # Capitalize each word

# Path parameter validation
@app.get("/items/{item_id}")
def read_item(
    item_id: int = Path(
        ...,  # ... means required
        title="The ID of the item to get",
        description="The unique identifier for the item",
        ge=1,  # greater than or equal to 1
        le=1000  # less than or equal to 1000
    )
):
    return {"item_id": item_id}

# Request body validation
@app.post("/items/")
def create_item(
    item: Item = Body(
        ...,  # ... means required
        embed=True,  # Embed in a key with model name
        example={  # Example for docs
            "name": "Sample Item",
            "price": 29.99,
            "is_offer": True
        }
    )
):
    return item

# Nested models and complex validation
class User(BaseModel):
    username: str
    email: str
    
    @validator('email')
    def email_must_be_valid(cls, v):
        if not re.match(r'^[a-zA-Z0-9_.+-]+@[a-zA-Z0-9-]+\.[a-zA-Z0-9-.]+$', v):
            raise ValueError('must be a valid email')
        return v

class Comment(BaseModel):
    text: str
    created_at: datetime
    author: User

@app.post("/comments/")
def create_comment(comment: Comment):
    return comment
```

## Dependency Injection

Dependencies allow you to share logic, enforce security, and handle database connections.

```python
from fastapi import Depends, FastAPI, Header, HTTPException
from typing import Optional

app = FastAPI()

# Simple dependency (function)
async def get_token_header(x_token: str = Header(...)):
    if x_token != "fake-super-secret-token":
        raise HTTPException(status_code=400, detail="X-Token header invalid")
    return x_token

# Dependency using a dependency
async def get_query_token(
    token: str = Depends(get_token_header),
    query_token: Optional[str] = None
):
    if query_token is None:
        return token
    return query_token

# Use dependency in a path operation
@app.get("/items/", dependencies=[Depends(get_token_header)])
async def read_items():
    return [{"item": "Foo"}, {"item": "Bar"}]

# Class-based dependency
class CommonQueryParams:
    def __init__(self, q: Optional[str] = None, skip: int = 0, limit: int = 100):
        self.q = q
        self.skip = skip
        self.limit = limit

@app.get("/users/")
async def read_users(commons: CommonQueryParams = Depends()):
    return {"q": commons.q, "skip": commons.skip, "limit": commons.limit}

# Sub-dependencies
def verify_token(token: str):
    # In a real app, verify token with your auth backend
    if token != "allowed-token":
        raise HTTPException(status_code=401, detail="Invalid token")
    return token

def verify_key(key: str):
    # In a real app, verify API key with your auth backend
    if key != "allowed-key":
        raise HTTPException(status_code=401, detail="Invalid key")
    return key

def get_current_user(token: str = Depends(verify_token), key: str = Depends(verify_key)):
    # Both dependencies must be satisfied
    return {"token": token, "key": key}

@app.get("/protected")
async def protected_route(user: dict = Depends(get_current_user)):
    return {"user": user}

# Global dependencies (applied to all routes)
app = FastAPI(dependencies=[Depends(get_token_header)])
```

## Authentication and Authorization

FastAPI provides tools to implement authentication and authorization.

```python
from fastapi import Depends, FastAPI, HTTPException, status
from fastapi.security import OAuth2PasswordBearer, OAuth2PasswordRequestForm
from pydantic import BaseModel
from typing import Optional
from datetime import datetime, timedelta
import jwt

# JWT configuration
SECRET_KEY = "your-secret-key"  # In production, use a secure random key
ALGORITHM = "HS256"
ACCESS_TOKEN_EXPIRE_MINUTES = 30

app = FastAPI()

# Define Token model
class Token(BaseModel):
    access_token: str
    token_type: str

# Define User models
class User(BaseModel):
    username: str
    email: Optional[str] = None
    full_name: Optional[str] = None
    disabled: Optional[bool] = None

class UserInDB(User):
    hashed_password: str

# Fake users database
fake_users_db = {
    "johndoe": {
        "username": "johndoe",
        "full_name": "John Doe",
        "email": "johndoe@example.com",
        "hashed_password": "fakehashedsecret",  # In production, use proper hashing
        "disabled": False,
    }
}

# OAuth2 password flow
oauth2_scheme = OAuth2PasswordBearer(tokenUrl="token")

# Password verification (simplified)
def verify_password(plain_password, hashed_password):
    # In production, use proper password hashing (e.g., bcrypt)
    return plain_password + "fakehashed" == hashed_password

def get_user(db, username: str):
    if username in db:
        user_dict = db[username]
        return UserInDB(**user_dict)

def authenticate_user(db, username: str, password: str):
    user = get_user(db, username)
    if not user:
        return False
    if not verify_password(password, user.hashed_password):
        return False
    return user

# Create access token
def create_access_token(data: dict, expires_delta: Optional[timedelta] = None):
    to_encode = data.copy()
    if expires_delta:
        expire = datetime.utcnow() + expires_delta
    else:
        expire = datetime.utcnow() + timedelta(minutes=15)
    to_encode.update({"exp": expire})
    encoded_jwt = jwt.encode(to_encode, SECRET_KEY, algorithm=ALGORITHM)
    return encoded_jwt

# Get current user from token
async def get_current_user(token: str = Depends(oauth2_scheme)):
    credentials_exception = HTTPException(
        status_code=status.HTTP_401_UNAUTHORIZED,
        detail="Could not validate credentials",
        headers={"WWW-Authenticate": "Bearer"},
    )
    try:
        payload = jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])
        username: str = payload.get("sub")
        if username is None:
            raise credentials_exception
    except jwt.PyJWTError:
        raise credentials_exception
    user = get_user(fake_users_db, username=username)
    if user is None:
        raise credentials_exception
    return user

# Get current active user
async def get_current_active_user(current_user: User = Depends(get_current_user)):
    if current_user.disabled:
        raise HTTPException(status_code=400, detail="Inactive user")
    return current_user

# Login endpoint
@app.post("/token", response_model=Token)
async def login_for_access_token(form_data: OAuth2PasswordRequestForm = Depends()):
    user = authenticate_user(fake_users_db, form_data.username, form_data.password)
    if not user:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Incorrect username or password",
            headers={"WWW-Authenticate": "Bearer"},
        )
    access_token_expires = timedelta(minutes=ACCESS_TOKEN_EXPIRE_MINUTES)
    access_token = create_access_token(
        data={"sub": user.username}, expires_delta=access_token_expires
    )
    return {"access_token": access_token, "token_type": "bearer"}

# Protected route
@app.get("/users/me/", response_model=User)
async def read_users_me(current_user: User = Depends(get_current_active_user)):
    return current_user
```

## Database Integration

FastAPI works well with any database through Python libraries and ORMs.

```python
from fastapi import Depends, FastAPI, HTTPException
from sqlalchemy import create_engine, Column, Integer, String, Boolean
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker, Session
from pydantic import BaseModel
from typing import List, Optional

# SQLAlchemy setup
SQLALCHEMY_DATABASE_URL = "sqlite:///./test.db"  # For SQLite
# SQLALCHEMY_DATABASE_URL = "postgresql://user:password@postgresserver/db"  # For PostgreSQL

engine = create_engine(
    SQLALCHEMY_DATABASE_URL, connect_args={"check_same_thread": False}  # Only needed for SQLite
)
SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)

Base = declarative_base()

# SQLAlchemy model
class ItemDB(Base):
    __tablename__ = "items"
    
    id = Column(Integer, primary_key=True, index=True)
    name = Column(String, index=True)
    description = Column(String, index=True)
    price = Column(Integer)
    is_available = Column(Boolean, default=True)

# Create tables
Base.metadata.create_all(bind=engine)

# Pydantic models
class ItemBase(BaseModel):
    name: str
    description: Optional[str] = None
    price: float
    is_available: Optional[bool] = True

class ItemCreate(ItemBase):
    pass

class Item(ItemBase):
    id: int
    
    class Config:
        orm_mode = True  # Allow converting SQLAlchemy model to Pydantic model

# Dependency to get the database session
def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()

app = FastAPI()

# Create item
@app.post("/items/", response_model=Item)
def create_item(item: ItemCreate, db: Session = Depends(get_db)):
    db_item = ItemDB(**item.dict())
    db.add(db_item)
    db.commit()
    db.refresh(db_item)
    return db_item

# Read items
@app.get("/items/", response_model=List[Item])
def read_items(skip: int = 0, limit: int = 100, db: Session = Depends(get_db)):
    items = db.query(ItemDB).offset(skip).limit(limit).all()
    return items

# Read item by ID
@app.get("/items/{item_id}", response_model=Item)
def read_item(item_id: int, db: Session = Depends(get_db)):
    db_item = db.query(ItemDB).filter(ItemDB.id == item_id).first()
    if db_item is None:
        raise HTTPException(status_code=404, detail="Item not found")
    return db_item

# Update item
@app.put("/items/{item_id}", response_model=Item)
def update_item(item_id: int, item: ItemCreate, db: Session = Depends(get_db)):
    db_item = db.query(ItemDB).filter(ItemDB.id == item_id).first()
    if db_item is None:
        raise HTTPException(status_code=404, detail="Item not found")
    
    for key, value in item.dict().items():
        setattr(db_item, key, value)
    
    db.commit()
    db.refresh(db_item)
    return db_item

# Delete item
@app.delete("/items/{item_id}", response_model=Item)
def delete_item(item_id: int, db: Session = Depends(get_db)):
    db_item = db.query(ItemDB).filter(ItemDB.id == item_id).first()
    if db_item is None:
        raise HTTPException(status_code=404, detail="Item not found")
    
    db.delete(db_item)
    db.commit()
    return db_item
```

## Background Tasks

FastAPI supports background tasks for operations that don't need to happen during request handling.

```python
from fastapi import BackgroundTasks, FastAPI
import time
import logging

app = FastAPI()

# Setup logging
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

# Background task function
def process_item(item_id: int):
    # Simulate a long-running process
    logger.info(f"Processing item {item_id}")
    time.sleep(5)  # Simulate work
    logger.info(f"Finished processing item {item_id}")

# Simple background task
@app.post("/items/{item_id}")
async def create_item(item_id: int, background_tasks: BackgroundTasks):
    # Add task to background
    background_tasks.add_task(process_item, item_id)
    return {"message": f"Item {item_id} will be processed in the background"}

# Multiple background tasks
def send_email(email: str, message: str):
    logger.info(f"Sending email to {email}: {message}")
    # Implement email sending logic here
    time.sleep(2)  # Simulate work
    logger.info(f"Email sent to {email}")

def update_stats(item_id: int):
    logger.info(f"Updating stats for item {item_id}")
    # Implement stats updating logic here
    time.sleep(1)  # Simulate work
    logger.info(f"Stats updated for item {item_id}")

@app.post("/orders/{order_id}")
async def create_order(
    order_id: int,
    email: str,
    background_tasks: BackgroundTasks
):
    # Add multiple tasks to background
    background_tasks.add_task(send_email, email, f"Order {order_id} confirmed")
    background_tasks.add_task(update_stats, order_id)
    return {"message": f"Order {order_id} confirmed, email will be sent to {email}"}

# Chain of background tasks
def generate_report(report_id: int, background_tasks: BackgroundTasks):
    logger.info(f"Generating report {report_id}")
    time.sleep(3)  # Simulate work
    logger.info(f"Report {report_id} generated")
    # Add another task to the chain
    background_tasks.add_task(send_report, report_id)

def send_report(report_id: int):
    logger.info(f"Sending report {report_id}")
    time.sleep(2)  # Simulate work
    logger.info(f"Report {report_id} sent")

@app.post("/reports/{report_id}")
async def create_report(report_id: int, background_tasks: BackgroundTasks):
    # Start the chain of background tasks
    background_tasks.add_task(generate_report, report_id, background_tasks)
    return {"message": f"Report {report_id} creation started"}
```

## Middleware

Middleware allows you to process requests before they reach your route handlers and responses before they're returned to clients.

```python
from fastapi import FastAPI, Request, Response
from starlette.middleware.base import BaseHTTPMiddleware
import time

app = FastAPI()

# Simple middleware function
@app.middleware("http")
async def add_process_time_header(request: Request, call_next):
    # Process the request
    start_time = time.time()
    response = await call_next(request)
    process_time = time.time() - start_time
    
    # Add custom header to response
    response.headers["X-Process-Time"] = str(process_time)
    return response

# Class-based middleware
class LoggingMiddleware(BaseHTTPMiddleware):
    async def dispatch(self, request: Request, call_next):
        # Log request
        print(f"Request: {request.method} {request.url}")
        
        # Process request
        response = await call_next(request)
        
        # Log response
        print(f"Response: {response.status_code}")
        return response

# Add class-based middleware
app.add_middleware(LoggingMiddleware)

# Error handling middleware
class ErrorHandlingMiddleware(BaseHTTPMiddleware):
    async def dispatch(self, request: Request, call_next):
        try:
            return await call_next(request)
        except Exception as e:
            # In production, you'd want to log the error
            print(f"Error: {str(e)}")
            
            # Return a custom error response
            return Response(
                content={"detail": "Internal Server Error"},
                status_code=500,
                media_type="application/json"
            )

# Add error handling middleware
app.add_middleware(ErrorHandlingMiddleware)

# Simple route for testing
@app.get("/")
async def read_root():
    return {"Hello": "World"}

# Route that raises an error
@app.get("/error")
async def cause_error():
    # This will be caught by the ErrorHandlingMiddleware
    raise ValueError("This is a test error")
```

## CORS (Cross-Origin Resource Sharing)

CORS middleware allows you to control which domains can access your API.

```python
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware

app = FastAPI()

# Add CORS middleware
app.add_middleware(
    CORSMiddleware,
    # Allow requests from these origins
    allow_origins=[
        "http://localhost",
        "http://localhost:8080",
        "https://yourdomain.com",
    ],
    # Allow cookies to be included in requests
    allow_credentials=True,
    # Allow these HTTP methods
    allow_methods=["*"],  # All methods
    # Allow these headers in requests
    allow_headers=["*"],  # All headers
)

# Alternatively, allow all origins (not recommended for production)
"""
app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],  # All origins
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
"""

@app.get("/")
async def main():
    return {"message": "Hello World"}
```

## Testing

FastAPI provides tools for testing your API endpoints.

```python
# main.py
from fastapi import FastAPI, HTTPException, Depends
from pydantic import BaseModel

app = FastAPI()

# Models
class Item(BaseModel):
    id: int
    name: str
    description: str = None
    price: float

# Database simulation
fake_db = {}

# Dependencies
def get_item_or_404(item_id: int):
    if item_id not in fake_db:
        raise HTTPException(status_code=404, detail="Item not found")
    return fake_db[item_id]

# Routes
@app.post("/items/", status_code=201)
async def create_item(item: Item):
    if item.id in fake_db:
        raise HTTPException(status_code=400, detail="Item already exists")
    fake_db[item.id] = item
    return item

@app.get("/items/{item_id}")
async def read_item(item: Item = Depends(get_item_or_404)):
    return item

@app.put("/items/{item_id}")
async def update_item(item_id: int, item_data: Item):
    stored_item = get_item_or_404(item_id)
    fake_db[item_id] = item_data
    return item_data

@app.delete("/items/{item_id}")
async def delete_item(item_id: int):
    item = get_item_or_404(item_id)
    del fake_db[item_id]
    return {"detail": "Item deleted"}
```

# test_main.py (Testing file)
```python
from fastapi.testclient import TestClient
from main import app
import pytest

# Create test client
client = TestClient(app)

# Test creating an item
def test_create_item():
    response = client.post(
        "/items/",
        json={"id": 1, "name": "Test Item", "description": "Test description", "price": 10.5}
    )
    assert response.status_code == 201
    assert response.json() == {
        "id": 1,
        "name": "Test Item",
        "description": "Test description",
        "price": 10.5
    }

# Test reading an item
def test_read_item():
    # First create an item
    client.post(
        "/items/",
        json={"id": 2, "name": "Test Item 2", "price": 20.5}
    )
    
    # Then read it
    response = client.get("/items/2")
    assert response.status_code == 200
    assert response.json() == {
        "id": 2,
        "name": "Test Item 2",
        "description": None,
        "price": 20.5
    }

# Test reading a non-existent item
def test_read_nonexistent_item():
    response = client.get("/items/999")
    assert response.status_code == 404
    assert response.json() == {"detail": "Item not found"}

# Test updating an item
def test_update_item():
    # First create an item
    client.post(
        "/items/",
        json={"id": 3, "name": "Original Name", "price": 30.5}
    )
    
    # Then update it
    response = client.put(
        "/items/3",
        json={"id": 3, "name": "Updated Name", "price": 35.0}
    )
    assert response.status_code == 200
    assert response.json()["name"] == "Updated Name"
    
    # Verify the update
    response = client.get("/items/3")
    assert response.json()["price"] == 35.0

# Test deleting an item
def test_delete_item():
    # First create an item
    client.post(
        "/items/",
        json={"id": 4, "name": "Item to Delete", "price": 40.5}
    )
    
    # Then delete it
    response = client.delete("/items/4")
    assert response.status_code == 200
    
    # Verify it's gone
    response = client.get("/items/4")
    assert response.status_code == 404
```

## Deployment

FastAPI applications can be deployed in various ways.

```python
# Production server setup
# You'll need to use a production ASGI server like Uvicorn or Hypercorn

# Terminal command to run with Uvicorn:
# uvicorn main:app --host 0.0.0.0 --port 8000

# For higher performance, you can use Gunicorn with Uvicorn workers:
# gunicorn main:app -w 4 -k uvicorn.workers.UvicornWorker -b 0.0.0.0:8000

# Docker deployment
"""
# Dockerfile
FROM python:3.9

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
"""

# docker-compose.yml
"""
version: '3'

services:
  api:
    build: .
    ports:
      - "8000:8000"
    environment:
      - DATABASE_URL=postgresql://user:password@db/dbname
    depends_on:
      - db
  
  db:
    image: postgres:13
    environment:
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=password
      - POSTGRES_DB=dbname
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
"""

# Deploy to Heroku
"""
# Procfile
web: uvicorn main:app --host=0.0.0.0 --port=${PORT:-8000}

# runtime.txt
python-3.9.6
"""

# Deploy to AWS Lambda with Mangum
from fastapi import FastAPI
from mangum import Mangum

app = FastAPI()

@app.get("/")
def read_root():
    return {"Hello": "World"}

# Handler for AWS Lambda
handler = Mangum(app)
```

## Middleware

FastAPI provides tools for implementing middleware.

```python
from fastapi import FastAPI, Request
import time

app = FastAPI()

@app.middleware("http")
async def add_process_time_header(request: Request, call_next):
    start_time = time.time()
    response = await call_next(request)
    process_time = time.time() - start_time
    response.headers["X-Process-Time"] = str(process_time)
    return response

# Custom middleware
from starlette.middleware.base import BaseHTTPMiddleware

class LoggingMiddleware(BaseHTTPMiddleware):
    async def dispatch(self, request: Request, call_next):
        # Log the request
        print(f"Request: {request.method} {request.url}")
        
        # Process the request
        response = await call_next(request)
        
        # Log the response
        print(f"Response status: {response.status_code}")
        return response

app.add_middleware(LoggingMiddleware)
```

## Best Practices

Here are some best practices for working with FastAPI:

```python
# Project structure for larger applications
"""
my_fastapi_project/
├── app/
│   ├── __init__.py
│   ├── main.py           # FastAPI app creation and configuration
│   ├── dependencies.py   # Shared dependencies
│   ├── routers/          # Route modules
│   │   ├── __init__.py
│   │   ├── items.py
│   │   └── users.py
│   ├── models/           # Pydantic models
│   │   ├── __init__.py
│   │   ├── item.py
│   │   └── user.py
│   ├── crud/             # Database CRUD operations
│   │   ├── __init__.py
│   │   ├── item.py
│   │   └── user.py
│   ├── schemas/          # SQLAlchemy models
│   │   ├── __init__.py
│   │   ├── item.py
│   │   └── user.py
│   ├── db/               # Database configuration
│   │   ├── __init__.py
│   │   └── session.py
│   ├── core/             # Core application code
│   │   ├── __init__.py
│   │   ├── config.py     # Configuration
│   │   └── security.py   # Security utilities
│   └── api/              # API endpoints
│       ├── __init__.py
│       └── api_v1/
│           ├── __init__.py
│           ├── endpoints/
│           │   ├── items.py
│           │   └── users.py
│           └── deps.py
├── tests/                # Test files
│   ├── __init__.py
│   ├── test_items.py
│   └── test_users.py
├── alembic/              # Database migrations
├── .env                  # Environment variables
├── .gitignore
├── requirements.txt
└── README.md
"""

# main.py (structured app)
from fastapi import FastAPI
from app.core.config import settings
from app.api.api_v1.api import api_router
from app.db.session import engine
from app import schemas

# Create database tables
schemas.Base.metadata.create_all(bind=engine)

# Create FastAPI app
app = FastAPI(
    title=settings.PROJECT_NAME,
    description=settings.PROJECT_DESCRIPTION,
    version=settings.VERSION,
    openapi_url=f"{settings.API_V1_STR}/openapi.json",
)

# Include routers
app.include_router(api_router, prefix=settings.API_V1_STR)

# Environment variables with Pydantic
from pydantic import BaseSettings

class Settings(BaseSettings):
    API_V1_STR: str = "/api/v1"
    PROJECT_NAME: str = "My FastAPI Project"
    PROJECT_DESCRIPTION: str = "FastAPI project with best practices"
    VERSION: str = "0.1.0"
    DATABASE_URL: str
    SECRET_KEY: str
    
    class Config:
        env_file = ".env"

settings = Settings()

# Router modularization (in app/api/api_v1/endpoints/items.py)
from fastapi import APIRouter, Depends, HTTPException
from sqlalchemy.orm import Session
from app import crud, models, schemas
from app.api.deps import get_db

router = APIRouter()

@router.get("/", response_model=list[schemas.Item])
def read_items(
    db: Session = Depends(get_db),
    skip: int = 0,
    limit: int = 100,
):
    items = crud.item.get_multi(db, skip=skip, limit=limit)
    return items

# In app/api/api_v1/api.py
from fastapi import APIRouter
from app.api.api_v1.endpoints import items, users

api_router = APIRouter()
api_router.include_router(items.router, prefix="/items", tags=["items"])
api_router.include_router(users.router, prefix="/users", tags=["users"])
```

## Real-World Applications

Here are examples of how companies use FastAPI:

### Microservices Architecture

```python
# users_service.py
from fastapi import FastAPI, HTTPException
import httpx
from pydantic import BaseModel

app = FastAPI(title="Users Service")

# Models
class User(BaseModel):
    id: int
    name: str
    email: str

# Fake database
users_db = {
    1: User(id=1, name="Alice", email="alice@example.com"),
    2: User(id=2, name="Bob", email="bob@example.com"),
}

# Routes
@app.get("/users/{user_id}", response_model=User)
async def get_user(user_id: int):
    if user_id not in users_db:
        raise HTTPException(status_code=404, detail="User not found")
    return users_db[user_id]

# orders_service.py
from fastapi import FastAPI, HTTPException, BackgroundTasks
from pydantic import BaseModel
import httpx

app = FastAPI(title="Orders Service")

# Models
class Order(BaseModel):
    id: int
    user_id: int
    product: str
    quantity: int
    status: str = "pending"

# Fake database
orders_db = {}

# Service discovery (in a real app, use a service registry)
USERS_SERVICE_URL = "http://users-service:8000"

# Routes
@app.post("/orders/", response_model=Order)
async def create_order(order: Order, background_tasks: BackgroundTasks):
    # Validate user exists
    async with httpx.AsyncClient() as client:
        try:
            response = await client.get(f"{USERS_SERVICE_URL}/users/{order.user_id}")
            response.raise_for_status()
        except httpx.HTTPStatusError as e:
            raise HTTPException(status_code=400, detail=f"User validation failed: {str(e)}")
    
    # Store order
    orders_db[order.id] = order
    
    # Process order in background
    background_tasks.add_task(process_order, order.id)
    
    return order

async def process_order(order_id: int):
    # Simulate processing
    import time
    time.sleep(2)
    
    # Update order status
    if order_id in orders_db:
        orders_db[order_id].status = "processed"
```

### Machine Learning API

```python
from fastapi import FastAPI, File, UploadFile, HTTPException
from fastapi.responses import JSONResponse
import numpy as np
from PIL import Image
import io
import joblib
import tensorflow as tf

app = FastAPI(title="Image Classification API")

# Load pre-trained model (in a real app, use a more robust loading mechanism)
try:
    model = tf.keras.models.load_model("model.h5")
    class_names = joblib.load("class_names.joblib")
except Exception as e:
    model = None
    class_names = []
    print(f"Error loading model: {e}")

@app.post("/classify/")
async def classify_image(file: UploadFile = File(...)):
    if model is None:
        raise HTTPException(status_code=500, detail="Model not loaded")
    
    # Validate file
    if not file.content_type.startswith("image/"):
        raise HTTPException(status_code=400, detail="File must be an image")
    
    try:
        # Read and preprocess image
        contents = await file.read()
        image = Image.open(io.BytesIO(contents)).convert("RGB")
        image = image.resize((224, 224))  # Resize to model input size
        image_array = np.array(image) / 255.0  # Normalize
        image_array = np.expand_dims(image_array, axis=0)  # Add batch dimension
        
        # Make prediction
        predictions = model.predict(image_array)
        predicted_class = np.argmax(predictions[0])
        confidence = float(predictions[0][predicted_class])
        
        return {
            "class": class_names[predicted_class],
            "confidence": confidence,
            "top_predictions": [
                {"class": class_names[i], "confidence": float(predictions[0][i])}
                for i in np.argsort(predictions[0])[-3:][::-1]  # Top 3 predictions
            ]
        }
    except Exception as e:
        raise HTTPException(status_code=500, detail=f"Error processing image: {str(e)}")
```

### Real-time Dashboard API

```python
from fastapi import FastAPI, WebSocket, WebSocketDisconnect
from fastapi.responses import HTMLResponse
import json
import asyncio
import random

app = FastAPI(title="Real-time Dashboard API")

# HTML for a simple demo dashboard
html = """
<!DOCTYPE html>
<html>
    <head>
        <title>Real-time Dashboard</title>
        <style>
            #chart {
                width: 600px;
                height: 400px;
                margin: 20px auto;
                border: 1px solid #ddd;
            }
            .data-point {
                display: inline-block;
                width: 10px;
                height: 100px;
                background-color: #3498db;
                margin-right: 2px;
                vertical-align: bottom;
            }
        </style>
    </head>
    <body>
        <h1>Real-time Data Dashboard</h1>
        <div id="chart"></div>
        <script>
            const chart = document.getElementById('chart');
            const ws = new WebSocket(`ws://${window.location.host}/ws`);
            
            ws.onmessage = function(event) {
                const data = JSON.parse(event.data);
                
                // Update chart
                const dataPoint = document.createElement('div');
                dataPoint.className = 'data-point';
                dataPoint.style.height = `${data.value}px`;
                
                chart.appendChild(dataPoint);
                
                // Remove old data points if too many
                if (chart.children.length > 50) {
                    chart.removeChild(chart.children[0]);
                }
            };
        </script>
    </body>
</html>
"""

# Websocket connection manager
class ConnectionManager:
    def __init__(self):
        self.active_connections = []
    
    async def connect(self, websocket: WebSocket):
        await websocket.accept()
        self.active_connections.append(websocket)
    
    def disconnect(self, websocket: WebSocket):
        self.active_connections.remove(websocket)
    
    async def broadcast(self, data: str):
        for connection in self.active_connections:
            await connection.send_text(data)

manager = ConnectionManager()

# Serve HTML dashboard
@app.get("/", response_class=HTMLResponse)
async def get():
    return html

# WebSocket endpoint
@app.websocket("/ws")
async def websocket_endpoint(websocket: WebSocket):
    await manager.connect(websocket)
    try:
        # Keep the connection open
        while True:
            # In a real app, you'd send actual data from your system
            await asyncio.sleep(1)
    except WebSocketDisconnect:
        manager.disconnect(websocket)

# Background task to generate and broadcast data
@app.on_event("startup")
async def startup_event():
    asyncio.create_task(generate_data())

async def generate_data():
    while True:
        # Simulate data generation (in a real app, this would be actual metrics)
        data = {
            "timestamp": asyncio.get_event_loop().time(),
            "value": random.randint(20, 100),
        }
        
        # Broadcast to all connected clients
        await manager.broadcast(json.dumps(data))
        
        # Wait before next update
        await asyncio.sleep(1)
```

### API Gateway Service

```python
from fastapi import FastAPI, HTTPException, Request
from fastapi.responses import JSONResponse
import httpx
from pydantic import BaseModel, validator
import time
import jwt
from starlette.middleware.base import BaseHTTPMiddleware

app = FastAPI(title="API Gateway")

# Configuration
SECRET_KEY = "your-secret-key"  # In production, use a secure key
SERVICE_ROUTES = {
    "users": "http://users-service:8000",
    "orders": "http://orders-service:8000",
    "products": "http://products-service:8000",
}

# Authentication middleware
class AuthMiddleware(BaseHTTPMiddleware):
    async def dispatch(self, request: Request, call_next):
        # Public paths (no auth required)
        if request.url.path in ["/docs", "/redoc", "/openapi.json", "/login"]:
            return await call_next(request)
        
        # Check for token
        token = request.headers.get("Authorization")
        if not token or not token.startswith("Bearer "):
            return JSONResponse(
                status_code=401,
                content={"detail": "Missing or invalid authentication token"}
            )
        
        token = token.replace("Bearer ", "")
        
        try:
            # Verify token
            payload = jwt.decode(token, SECRET_KEY, algorithms=["HS256"])
            # Add user info to request state
            request.state.user = payload.get("sub")
        except jwt.PyJWTError:
            return JSONResponse(
                status_code=401,
                content={"detail": "Invalid authentication token"}
            )
        
        return await call_next(request)

# Add middleware
app.add_middleware(AuthMiddleware)

# Login model
class LoginData(BaseModel):
    username: str
    password: str

# Login endpoint
@app.post("/login")
async def login(data: LoginData):
    # In a real app, validate against a database
    if data.username == "admin" and data.password == "password":
        # Create JWT token
        expiration = int(time.time()) + 3600  # 1 hour
        token = jwt.encode(
            {"sub": data.username, "exp": expiration},
            SECRET_KEY,
            algorithm="HS256"
        )
        return {"access_token": token, "token_type": "bearer"}
    raise HTTPException(status_code=401, detail="Invalid username or password")

# Proxy endpoint
@app.api_route("/{service}/{path:path}", methods=["GET", "POST", "PUT", "DELETE"])
async def proxy(service: str, path: str, request: Request):
    if service not in SERVICE_ROUTES:
        raise HTTPException(status_code=404, detail=f"Service '{service}' not found")
    
    # Get target URL
    target_url = f"{SERVICE_ROUTES[service]}/{path}"
    
    # Prepare headers (exclude host)
    headers = {}
    for name, value in request.headers.items():
        if name.lower() != "host":
            headers[name] = value
    
    try:
        # Get request body if any
        body = None
        if request.method in ["POST", "PUT"]:
            body = await request.body()
        
        # Forward request to target service
        async with httpx.AsyncClient() as client:
            response = await client.request(
                method=request.method,
                url=target_url,
                headers=headers,
                params=request.query_params,
                content=body,
                timeout=30.0
            )
            
            # Return response from target service
            return JSONResponse(
                content=response.json(),
                status_code=response.status_code,
                headers=dict(response.headers)
            )
    except httpx.RequestError as e:
        raise HTTPException(status_code=503, detail=f"Service unavailable: {str(e)}")
```
