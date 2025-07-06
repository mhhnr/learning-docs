# SQLAlchemy for Complete Beginners

## What is SQLAlchemy?

SQLAlchemy is a Python library that helps developers work with databases. It's what we call an "Object-Relational Mapper" (ORM), which means it translates between Python code and database languages like SQL. With SQLAlchemy, you can interact with your database using familiar Python code instead of writing complex SQL queries.

Think of SQLAlchemy as a translator between two different worlds:
- The world of Python (with objects, classes, and methods)
- The world of databases (with tables, rows, and SQL)

## Why Use SQLAlchemy?

Before we dive into the code, let's understand why so many developers use SQLAlchemy:

1. **Write Python, not SQL**: You can interact with your database using Python syntax that's familiar and easier to read.

2. **Database Independence**: Your code works with different types of databases (SQLite, PostgreSQL, MySQL, etc.) without changes.

3. **Security**: SQLAlchemy helps prevent SQL injection attacks by properly handling parameter values.

4. **Object Mapping**: You can work with your data as Python objects, which is more natural in Python applications.

5. **Integration**: It works well with web frameworks like FastAPI, Flask, and Django.

## Basic Concepts

### 1. Engine

The Engine is your connection to the database. It manages how SQLAlchemy talks to your database.

```python
# Creating an engine
from sqlalchemy import create_engine

# Connect to SQLite database (a simple file-based database)
engine = create_engine('sqlite:///mydatabase.db')

# Connect to PostgreSQL
# engine = create_engine('postgresql://username:password@localhost:5432/mydatabase')

# Connect to MySQL
# engine = create_engine('mysql://username:password@localhost/mydatabase')
```

### 2. Declarative Base

The declarative base is a factory function that returns a base class. Your database models (tables) will inherit from this base class.

```python
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy import Column, Integer, String

# Create a base class for our class definitions
Base = declarative_base()

# Define a model (table)
class User(Base):
    __tablename__ = 'users'  # This will be the name of the table in the database
    
    id = Column(Integer, primary_key=True)  # Primary key column
    name = Column(String)  # String column
    age = Column(Integer)  # Integer column
    
    def __repr__(self):
        # This helps with debugging by showing a readable representation
        return f"<User(name='{self.name}', age={self.age})>"
```

### 3. Session

The Session is how you interact with the database. It's like a workspace for your database operations.

```python
from sqlalchemy.orm import sessionmaker

# Create a session factory
Session = sessionmaker(bind=engine)

# Create a session
session = Session()
```

## Creating Tables

Once you've defined your models, you need to create the actual tables in the database:

```python
# Create all tables defined in your models
Base.metadata.create_all(engine)
```

## Basic Database Operations (CRUD)

CRUD stands for Create, Read, Update, and Delete - the four basic operations you'll perform with databases.

### Create (Insert Records)

```python
# Create a new user
new_user = User(name='John', age=30)

# Add to the session
session.add(new_user)

# Add multiple users at once
session.add_all([
    User(name='Alice', age=25),
    User(name='Bob', age=35),
    User(name='Charlie', age=40)
])

# Commit the changes to the database
session.commit()
```

### Read (Query Records)

```python
# Get all users
all_users = session.query(User).all()
for user in all_users:
    print(user)

# Get a specific user by ID
user = session.query(User).get(1)  # Get user with ID 1
print(user)

# Filter users
young_users = session.query(User).filter(User.age < 30).all()
for user in young_users:
    print(user)

# Complex filters
users = session.query(User).filter(
    (User.age > 25) & (User.name.like('%A%'))
).all()
```

### Update Records

```python
# Find the user to update
user_to_update = session.query(User).filter(User.name == 'John').first()

# Update the user
if user_to_update:
    user_to_update.age = 31
    session.commit()
    print("User updated")
```

### Delete Records

```python
# Find the user to delete
user_to_delete = session.query(User).filter(User.name == 'Charlie').first()

# Delete the user
if user_to_delete:
    session.delete(user_to_delete)
    session.commit()
    print("User deleted")
```

## Relationships Between Tables

In most applications, you'll have multiple tables that relate to each other. SQLAlchemy makes it easy to define and work with these relationships.

### One-to-Many Relationship

```python
from sqlalchemy import ForeignKey
from sqlalchemy.orm import relationship

class User(Base):
    __tablename__ = 'users'
    
    id = Column(Integer, primary_key=True)
    name = Column(String)
    
    # Define the relationship to Post
    posts = relationship("Post", back_populates="author")
    
    def __repr__(self):
        return f"<User(name='{self.name}')>"

class Post(Base):
    __tablename__ = 'posts'
    
    id = Column(Integer, primary_key=True)
    title = Column(String)
    content = Column(String)
    user_id = Column(Integer, ForeignKey('users.id'))  # Foreign key
    
    # Define the relationship to User
    author = relationship("User", back_populates="posts")
    
    def __repr__(self):
        return f"<Post(title='{self.title}')>"
```

Using relationships:

```python
# Create a user with posts
user = User(name='John')
user.posts = [
    Post(title='First Post', content='Content for first post'),
    Post(title='Second Post', content='Content for second post')
]

# Add and commit
session.add(user)
session.commit()

# Query user and access posts
user = session.query(User).filter_by(name='John').first()
for post in user.posts:
    print(f"{user.name} wrote: {post.title}")

# Query post and access author
post = session.query(Post).first()
print(f"This post was written by {post.author.name}")
```

### Many-to-Many Relationship

Many-to-many relationships require an association table:

```python
# Association table
student_course = Table('student_course', Base.metadata,
    Column('student_id', Integer, ForeignKey('students.id')),
    Column('course_id', Integer, ForeignKey('courses.id'))
)

class Student(Base):
    __tablename__ = 'students'
    
    id = Column(Integer, primary_key=True)
    name = Column(String)
    
    # Define the relationship to Course
    courses = relationship("Course", secondary=student_course, back_populates="students")
    
    def __repr__(self):
        return f"<Student(name='{self.name}')>"

class Course(Base):
    __tablename__ = 'courses'
    
    id = Column(Integer, primary_key=True)
    name = Column(String)
    
    # Define the relationship to Student
    students = relationship("Student", secondary=student_course, back_populates="courses")
    
    def __repr__(self):
        return f"<Course(name='{self.name}')>"
```

Using many-to-many relationships:

```python
# Create students and courses
student1 = Student(name='Alice')
student2 = Student(name='Bob')
course1 = Course(name='Math')
course2 = Course(name='Science')

# Establish relationships
student1.courses = [course1, course2]
student2.courses = [course1]

# Add and commit
session.add_all([student1, student2, course1, course2])
session.commit()

# Query and use the relationships
student = session.query(Student).filter_by(name='Alice').first()
for course in student.courses:
    print(f"{student.name} is enrolled in {course.name}")

course = session.query(Course).filter_by(name='Math').first()
for student in course.students:
    print(f"{student.name} is taking {course.name}")
```

## Advanced Querying

SQLAlchemy provides powerful querying capabilities:

### Ordering Results

```python
# Order users by age descending
users = session.query(User).order_by(User.age.desc()).all()

# Order by multiple columns
users = session.query(User).order_by(User.name, User.age.desc()).all()
```

### Limiting Results

```python
# Get only the first 5 users
users = session.query(User).limit(5).all()

# Skip the first 5 users and get the next 5
users = session.query(User).offset(5).limit(5).all()
```

### Joining Tables

```python
# Join query
results = session.query(User, Post).join(Post).all()
for user, post in results:
    print(f"{user.name} wrote: {post.title}")

# Filtering with joins
results = session.query(User).join(User.posts).filter(Post.title.like('%First%')).all()
```

### Aggregation and Grouping

```python
from sqlalchemy import func

# Count all users
user_count = session.query(func.count(User.id)).scalar()
print(f"Total users: {user_count}")

# Group by and count
age_groups = session.query(User.age, func.count(User.id)).group_by(User.age).all()
for age, count in age_groups:
    print(f"Age {age}: {count} users")
```

## Transactions

Transactions ensure that multiple operations either all succeed or all fail together:

```python
# Start a transaction
try:
    # Multiple operations
    user1 = User(name='User1', age=25)
    user2 = User(name='User2', age=30)
    
    session.add(user1)
    session.add(user2)
    
    # If everything is OK, commit
    session.commit()
    print("Transaction committed")
except:
    # If there's an error, rollback
    session.rollback()
    print("Transaction rolled back")
finally:
    # Always close the session
    session.close()
```

## Complete Example

Let's put everything together into a simple but complete example:

```python
from sqlalchemy import create_engine, Column, Integer, String, ForeignKey
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker, relationship

# Create engine and base
engine = create_engine('sqlite:///example.db', echo=True)  # echo=True shows SQL
Base = declarative_base()

# Define models
class User(Base):
    __tablename__ = 'users'
    
    id = Column(Integer, primary_key=True)
    name = Column(String)
    age = Column(Integer)
    
    # Relationship to Address
    addresses = relationship("Address", back_populates="user", cascade="all, delete-orphan")
    
    def __repr__(self):
        return f"<User(name='{self.name}', age={self.age})>"

class Address(Base):
    __tablename__ = 'addresses'
    
    id = Column(Integer, primary_key=True)
    email = Column(String, nullable=False)
    user_id = Column(Integer, ForeignKey('users.id'))
    
    # Relationship to User
    user = relationship("User", back_populates="addresses")
    
    def __repr__(self):
        return f"<Address(email='{self.email}')>"

# Create tables
Base.metadata.create_all(engine)

# Create session
Session = sessionmaker(bind=engine)
session = Session()

# Create data
def create_sample_data():
    # Check if data already exists
    if session.query(User).count() == 0:
        # Create users with addresses
        john = User(name='John', age=30)
        john.addresses = [
            Address(email='john@example.com'),
            Address(email='john@work.com')
        ]
        
        alice = User(name='Alice', age=25)
        alice.addresses = [
            Address(email='alice@example.com')
        ]
        
        # Add to session and commit
        session.add_all([john, alice])
        session.commit()
        print("Sample data created")
    else:
        print("Data already exists")

# Query and display data
def display_data():
    # Get all users and their addresses
    users = session.query(User).all()
    
    for user in users:
        print(f"\nUser: {user.name}, Age: {user.age}")
        print("Addresses:")
        for address in user.addresses:
            print(f"  - {address.email}")

# Run functions
create_sample_data()
display_data()

# Close session
session.close()
```

## Integration with FastAPI

Here's how SQLAlchemy is typically used with FastAPI:

```python
from fastapi import FastAPI, Depends, HTTPException
from sqlalchemy import create_engine, Column, Integer, String
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker, Session
from pydantic import BaseModel
from typing import List, Optional

# Database setup
SQLALCHEMY_DATABASE_URL = "sqlite:///./fastapi_test.db"
engine = create_engine(SQLALCHEMY_DATABASE_URL)
SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)
Base = declarative_base()

# SQLAlchemy model
class UserDB(Base):
    __tablename__ = "users"
    
    id = Column(Integer, primary_key=True, index=True)
    name = Column(String)
    age = Column(Integer)

# Create tables
Base.metadata.create_all(bind=engine)

# Pydantic models for request/response
class UserBase(BaseModel):
    name: str
    age: int

class UserCreate(UserBase):
    pass

class User(UserBase):
    id: int
    
    class Config:
        orm_mode = True

# Dependency to get the database session
def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()

# FastAPI app
app = FastAPI()

# API endpoints
@app.post("/users/", response_model=User)
def create_user(user: UserCreate, db: Session = Depends(get_db)):
    db_user = UserDB(**user.dict())
    db.add(db_user)
    db.commit()
    db.refresh(db_user)
    return db_user

@app.get("/users/", response_model=List[User])
def read_users(skip: int = 0, limit: int = 100, db: Session = Depends(get_db)):
    users = db.query(UserDB).offset(skip).limit(limit).all()
    return users

@app.get("/users/{user_id}", response_model=User)
def read_user(user_id: int, db: Session = Depends(get_db)):
    db_user = db.query(UserDB).filter(UserDB.id == user_id).first()
    if db_user is None:
        raise HTTPException(status_code=404, detail="User not found")
    return db_user

@app.put("/users/{user_id}", response_model=User)
def update_user(user_id: int, user: UserCreate, db: Session = Depends(get_db)):
    db_user = db.query(UserDB).filter(UserDB.id == user_id).first()
    if db_user is None:
        raise HTTPException(status_code=404, detail="User not found")
    
    # Update user attributes
    for key, value in user.dict().items():
        setattr(db_user, key, value)
    
    db.commit()
    db.refresh(db_user)
    return db_user

@app.delete("/users/{user_id}", response_model=User)
def delete_user(user_id: int, db: Session = Depends(get_db)):
    db_user = db.query(UserDB).filter(UserDB.id == user_id).first()
    if db_user is None:
        raise HTTPException(status_code=404, detail="User not found")
    
    db.delete(db_user)
    db.commit()
    return db_user
```

## Common SQLAlchemy Data Types

SQLAlchemy supports many data types:

```python
from sqlalchemy import (
    Boolean, Column, Integer, Float, String, Text,
    Date, DateTime, Time, ForeignKey, Enum
)
import enum
from datetime import datetime, date, time

# Example enum
class UserType(enum.Enum):
    regular = "regular"
    admin = "admin"
    guest = "guest"

class AdvancedUser(Base):
    __tablename__ = 'advanced_users'
    
    id = Column(Integer, primary_key=True)
    
    # String types
    username = Column(String(50), unique=True)  # Limited length string
    bio = Column(Text)  # Unlimited length text
    
    # Numeric types
    age = Column(Integer)
    height = Column(Float)
    
    # Boolean
    is_active = Column(Boolean, default=True)
    
    # Date and time
    birth_date = Column(Date)
    created_at = Column(DateTime, default=datetime.utcnow)
    login_time = Column(Time)
    
    # Enum
    user_type = Column(Enum(UserType), default=UserType.regular)
```

## Best Practices

1. **Always close your sessions**: Use `session.close()` or context managers to ensure sessions are closed.

2. **Use transactions**: Wrap related operations in try-except blocks with commit and rollback.

3. **Define relationships clearly**: Make sure to define `back_populates` or `backref` for bidirectional relationships.

4. **Use migrations**: For production applications, use Alembic to manage database schema changes.

5. **Keep session scope short**: Create sessions when needed and close them as soon as possible.

6. **Use ORM selectively**: Sometimes raw SQL or SQLAlchemy Core is more appropriate for complex queries.

7. **Index important columns**: Add indexes to columns used frequently in filtering or joins.

8. **Set cascade behavior**: Determine how changes should propagate through relationships.

## Conclusion

SQLAlchemy is a powerful tool that makes working with databases in Python much easier. By learning these basics, you're now equipped to:

- Create database models as Python classes
- Perform CRUD operations (Create, Read, Update, Delete)
- Define and use relationships between tables
- Write complex queries using Python code

###################


# SQLAlchemy for Complete Beginners

## What is SQLAlchemy?

SQLAlchemy is a library that facilitates the communication between Python programs and databases. Most of the times, this library is used as an Object Relational Mapper (ORM) tool that translates Python classes to tables on relational databases and automatically converts function calls to SQL statements.

Think of SQLAlchemy as a translator between two different worlds:
- The world of Python (with objects, classes, and methods)
- The world of databases (with tables, rows, and SQL commands)

Without SQLAlchemy, you would need to write raw SQL code like this:
```sql
SELECT * FROM users WHERE age > 21;
```

With SQLAlchemy, you can write Python code instead:
```python
session.query(User).filter(User.age > 21).all()
```

## Why Use SQLAlchemy?

SQLAlchemy is the Python SQL toolkit and Object Relational Mapper that gives application developers the full power and flexibility of SQL. It provides several important benefits:

1. **Write Python, not SQL**: You can work with databases using Python syntax that's familiar and easier to read.

2. **Database Independence**: Your code works with different types of databases (SQLite, PostgreSQL, MySQL, etc.) without changes.

3. **Security**: SQLAlchemy helps prevent SQL injection attacks by properly handling parameter values.

4. **Object Mapping**: You can work with your data as Python objects, which is more natural in Python applications.

5. **Integration**: It works well with web frameworks like FastAPI, Flask, and Django.

## How Does SQLAlchemy Work?

SQLAlchemy has two main components:

1. **SQLAlchemy Core**: A SQL toolkit that provides a way to interact with databases using SQL expressions in Python.

2. **SQLAlchemy ORM**: An Object-Relational Mapper that lets you work with database tables as Python classes and rows as objects.

SQLAlchemy Core focuses on SQL interaction, while SQLAlchemy ORM maps Python objects to databases. Most beginners start with the ORM because it's more intuitive if you're used to Python objects.

## Getting Started with SQLAlchemy

Let's walk through the basic steps of using SQLAlchemy:

### 1. Installation

First, you need to install SQLAlchemy:

```python
pip install sqlalchemy
```

You may also need to install a database driver. For example, for PostgreSQL:

```python
pip install psycopg2-binary
```

### 2. Creating a Connection

The first step is to create an "engine" that connects to your database:

```python
from sqlalchemy import create_engine

# Connect to SQLite (a simple file-based database)
engine = create_engine('sqlite:///mydata.db')

# Or connect to PostgreSQL
# engine = create_engine('postgresql://username:password@localhost:5432/mydatabase')
```

### 3. Defining Models

Models are Python classes that represent database tables:

```python
from sqlalchemy import Column, Integer, String, create_engine
from sqlalchemy.ext.declarative import declarative_base

# Create a base class for our models
Base = declarative_base()

# Define a User model
class User(Base):
    __tablename__ = 'users'  # Table name in the database
    
    id = Column(Integer, primary_key=True)
    name = Column(String)
    age = Column(Integer)
    
    def __repr__(self):
        return f"<User(name='{self.name}', age={self.age})>"
```

### 4. Creating Tables

After defining your models, you need to create the corresponding tables in the database:

```python
# Create tables
Base.metadata.create_all(engine)
```

### 5. Working with Sessions

In SQLAlchemy, you use sessions to interact with the database:

```python
from sqlalchemy.orm import sessionmaker

# Create a session factory
Session = sessionmaker(bind=engine)

# Create a session
session = Session()
```

## Basic Database Operations (CRUD)

Now let's learn how to perform the four basic database operations: Create, Read, Update, and Delete.

### Create (Adding Records)

```python
# Create a new user
new_user = User(name='John', age=30)

# Add to session
session.add(new_user)

# Commit changes to database
session.commit()
```

### Read (Querying Records)

```python
# Get all users
all_users = session.query(User).all()
for user in all_users:
    print(user.name, user.age)

# Get users with filters
young_users = session.query(User).filter(User.age < 25).all()
```

### Update (Modifying Records)

```python
# Find a user
user = session.query(User).filter_by(name='John').first()

# Update the user
if user:
    user.age = 31
    session.commit()
```

### Delete (Removing Records)

```python
# Find a user
user = session.query(User).filter_by(name='John').first()

# Delete the user
if user:
    session.delete(user)
    session.commit()
```

## Relationships Between Tables

Most applications have multiple tables that relate to each other. For example, a user might have many posts.

### One-to-Many Relationship:

```python
from sqlalchemy import Column, Integer, String, ForeignKey
from sqlalchemy.orm import relationship

class User(Base):
    __tablename__ = 'users'
    
    id = Column(Integer, primary_key=True)
    name = Column(String)
    
    # Define relationship to Post
    posts = relationship("Post", back_populates="author")

class Post(Base):
    __tablename__ = 'posts'
    
    id = Column(Integer, primary_key=True)
    title = Column(String)
    content = Column(String)
    user_id = Column(Integer, ForeignKey('users.id'))
    
    # Define relationship to User
    author = relationship("User", back_populates="posts")
```

## Integration with FastAPI

SQLAlchemy is a powerful Object-Relational Mapping (ORM) library for Python that bridges the gap between database systems and Python code. Instead of writing raw SQL queries, SQLAlchemy allows you to interact with your database using familiar Python objects and methods.

Here's how you would typically use SQLAlchemy with FastAPI:

```python
from fastapi import FastAPI, Depends
from sqlalchemy.orm import Session

# Set up database connection and models...

# Dependency to get database session
def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()

app = FastAPI()

@app.get("/users/")
def read_users(db: Session = Depends(get_db)):
    users = db.query(User).all()
    return users
```

## Common Pitfalls and Best Practices

1. **Always close your sessions**: Use `try/finally` or context managers to ensure sessions are closed.

2. **Use transactions**: Wrap related operations in try-except blocks with commit and rollback.

3. **Don't keep sessions open too long**: Create sessions when needed and close them as soon as possible.

4. **Be careful with relationships**: Make sure to define both sides of relationships correctly.

5. **Use migrations for schema changes**: Tools like Alembic help manage database schema changes safely.

## Real-world Use Cases

Companies use SQLAlchemy for various purposes:

1. **Web Applications**: To store and retrieve data for web apps built with frameworks like FastAPI, Flask, or Django.

2. **Data Analysis**: To pull data from databases for analysis and reporting.

3. **API Services**: To build data-driven APIs that access and manipulate database information.

4. **E-commerce**: To manage products, orders, customers, and transactions.

