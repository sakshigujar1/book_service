# Book Management Service

## 📚 Table of Contents
- [Overview](#overview)
- [Features](#features)
- [Folder Structure](#folder-structure)
- [REST API Endpoints](#rest-api-endpoints)
- [Data Model](#data-model)
- [Module Architecture Diagram](#module-architecture-diagram)
- [Component Diagram](#component-diagram)
- [Sequence Diagram](#sequence-diagram)
- [Run Locally](#run-locally)

## Overview

- The Book Management Service is a microservice responsible for handling all book-related operations in the Library Management System. It supports adding, updating, deleting, and retrieving book records. The service is independently deployable and integrated via Eureka Discovery and Spring Cloud Gateway.

---

## Features
- Add new books to the library
- Update book information (title, author, genre, etc.)
- Delete books from the system
- Retrieve books by ID or search by title
- Manage available copies for borrowing
---
## Folder Structure
<pre>
src/
└── main/
    ├── java/
    │   └── com.library.member/
    │       ├── controller/       # REST controllers
    │       ├── dto/              # Data Transfer Objects
    │       ├── entity/           # JPA Entities
    │       ├── repository/       # Spring Data Repositories
    │       └── service/          # Business logic layer
    └── resources/
        └── application.properties  # App configuration
</pre>
---
##  REST API Endpoints

| Method | Endpoint           | Description                |
|--------|--------------------|----------------------------|
| POST   | `/api/books`       | Add a new book             |
| PUT    | `/api/books/{id}`  | Update existing book       |
| DELETE | `/api/books/{id}`  | Delete book by ID          |
| GET    | `/api/books`       | List or search books       |
| GET    | `/api/books/{id}`  | Get book details by ID     |

Swagger Url : http://localhost:8081/swagger-ui/index.html#/

---
## Data Model
 `Book` Entity

| Field            | Type     | Description                    |
|------------------|----------|--------------------------------|
| bookId           | BIGINT   | Primary key, auto-generated    |
| title            | VARCHAR  | Book title                     |
| author           | VARCHAR  | Author name                    |
| genre            | VARCHAR  | Book genre/category            |
| isbn             | VARCHAR  | Unique book identifier         |
| yearPublished    | INTEGER  | Year of publication            |
| availableCopies  | INTEGER  | Number of copies available     |

---

## Module Architecture Diagram
```mermaid
graph TD
    BookController["BookController"]:::controller --> BookService["BookService"]:::service
    BookService --> BookRepository["BookRepository"]:::repository
    BookRepository --> Database["Database"]:::database

    classDef controller fill:#f9f,color:#000,stroke:#333,stroke-width:2px;
    classDef service fill:#ccf,color:#000,stroke:#333,stroke-width:2px;
    classDef repository fill:#cfc,color:#000,stroke:#333,stroke-width:2px;
    classDef database fill:#fcf,color:#000,stroke:#333,stroke-width:2px;

    class BookController controller;
    class BookService service;
    class BookRepository repository;
    class Database database;
```
## Component diagram

```mermaid
flowchart LR
 
  subgraph React_Frontend [React Frontend]
    direction TB
    A[Book UI Components]
    B[Book API Client]
  end
 
  subgraph Backend [Spring Boot / ASP.NET Core Backend]
    direction TB
    C[BookController]
    D[BookService]
    E[BookRepository]
  end
 
  subgraph Database [Relational Database]
    direction TB
    F[(Books Table)]
  end
 
  G[Book DTO]
  H[Book Entity]
 
  %% Connections
  B -->|HTTP/REST| C
  C -->|Calls| D
  D -->|Calls| E
  E -->|ORM / JDBC| F
 
  C ---|uses| G
  E ---|maps to| H
 
  %% Styling
  classDef ui fill:#dae8fc,stroke:#6c8ebf,color:#1a237e
  classDef backend fill:#d5e8d4,stroke:#82b366,color:#1b4332
  classDef database fill:#f3e5f5,stroke:#ab47bc,color:#4a148c
  classDef model fill:#fff2cc,stroke:#d6b656,color:#7f4f24
 
  class A,B ui
  class C,D,E backend
  class F database
  class G,H model
```
## Sequence Diagram
```mermaid

sequenceDiagram
  actor UI as React Frontend
  participant C as BookController
  participant S as BookService
  participant R as BookRepository
  participant DB as Database
  UI->>C: HTTP POST /api/books (BookDto)
  C->>S: registerMember(bookDto)
  S->>R: save(bookEntity)
  R->>DB: INSERT INTO Books
  R-->>S: Return Saved Book
  S-->>C: Return BookDto
  C-->>UI: 201 Created (BookDto)
  %% Styling (as comments for Mermaid, no visual impact)
  %% UI:       #dae8fc (soft blue)
  %% Controller/Service/Repo: #d5e8d4 (light green)
  %% DB:       #f3e5f5 (lavender)
```
---
##  Run Locally
 
```bash
# Clone this repo
 
# Navigate to the folder
cd member-service
 
# Build and run
mvn clean install
mvn spring-boot:run
