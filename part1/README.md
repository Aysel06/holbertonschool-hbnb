# HBnB Technical Documentation

## 1. Introduction

This document provides a detailed technical overview of the HBnB application architecture and design. It explains the system structure, business logic, and interaction flows.

The document includes:
- High-Level Architecture (Package Diagram)
- Business Logic Layer (Class Diagram)
- API Interaction Flow (Sequence Diagrams)

The goal is to ensure the system is clear, scalable, and maintainable.

---

## 2. High-Level Architecture

### Package Diagram

```mermaid
flowchart TD

subgraph Presentation_Layer
    UI[User Interface]
    API[API Controllers]
end

subgraph Business_Logic_Layer
    Facade[Facade]
    Services[Services]
    
    subgraph Models
        User
        Place
        Review
        Amenity
    end
end

subgraph Persistence_Layer
    Repositories[Repositories]
    Database[(Database)]
end

UI --> Facade
API --> Facade

Facade --> Services
Services --> User
Services --> Place
Services --> Review
Services --> Amenity

Services --> Repositories
Repositories --> Database
```

### Explanation

The system follows a three-layer architecture:

- **Presentation Layer**
  - Handles user interaction (UI and API)
  - Sends requests to the backend

- **Business Logic Layer**
  - Contains core application logic
  - Includes services, models, and a facade

- **Persistence Layer**
  - Manages data storage
  - Uses repositories to communicate with the database

### Facade Pattern

The Facade acts as a single entry point to the Business Logic Layer and simplifies communication between components.

---

## 3. Business Logic Layer

### Class Diagram

```mermaid
classDiagram

class User {
    +UUID id
    +string first_name
    +string last_name
    +string email
    +string password
    +datetime created_at
    +datetime updated_at
    +create()
    +update()
    +delete()
}

class Place {
    +UUID id
    +string title
    +string description
    +float price
    +datetime created_at
    +datetime updated_at
    +create()
    +update()
    +delete()
}

class Review {
    +UUID id
    +string text
    +int rating
    +datetime created_at
    +datetime updated_at
    +create()
    +update()
    +delete()
}

class Amenity {
    +UUID id
    +string name
    +datetime created_at
    +datetime updated_at
    +create()
    +update()
    +delete()
}

User "1" --> "0..*" Place : owns
User "1" --> "0..*" Review : writes
Place "1" --> "0..*" Review : has
Place "0..*" --> "0..*" Amenity : includes
Review --> User : author
Review --> Place : place
```

### Explanation

#### Entities

- **User**: Application users  
- **Place**: Property listings  
- **Review**: Feedback on places  
- **Amenity**: Features like Wi-Fi or parking  

#### Design

- Each entity has a UUID and timestamps  
- CRUD operations are defined  

#### Relationships

- A User can own multiple Places  
- A User can write multiple Reviews  
- A Place can have multiple Reviews  
- A Place can have multiple Amenities  
- A Review belongs to a User and a Place  

---

## 4. API Interaction Flow

### 4.1 User Registration

```mermaid
sequenceDiagram
participant User
participant API
participant Facade
participant Service
participant Repository
participant Database

User->>API: POST /users
API->>Facade: create_user(data)
Facade->>Service: validate_user(data)
Service->>Repository: save_user(user)
Repository->>Database: INSERT user
Database-->>Repository: success
Repository-->>Service: user saved
Service-->>Facade: return user
Facade-->>API: response
API-->>User: 201 Created
```

---

### 4.2 Place Creation

```mermaid
sequenceDiagram
participant User
participant API
participant Facade
participant Service
participant Repository
participant Database

User->>API: POST /places
API->>Facade: create_place(data)
Facade->>Service: validate_place(data)
Service->>Repository: save_place(place)
Repository->>Database: INSERT place
Database-->>Repository: success
Repository-->>Service: place saved
Service-->>Facade: return place
Facade-->>API: response
API-->>User: 201 Created
```

---

### 4.3 Review Submission

```mermaid
sequenceDiagram
participant User
participant API
participant Facade
participant Service
participant Repository
participant Database

User->>API: POST /reviews
API->>Facade: create_review(data)
Facade->>Service: validate_review(data)
Service->>Repository: save_review(review)
Repository->>Database: INSERT review
Database-->>Repository: success
Repository-->>Service: review saved
Service-->>Facade: return review
Facade-->>API: response
API-->>User: 201 Created
```

---

### 4.4 Fetch Places

```mermaid
sequenceDiagram
participant User
participant API
participant Facade
participant Service
participant Repository
participant Database

User->>API: GET /places
API->>Facade: get_places(filters)
Facade->>Service: handle_filters(filters)
Service->>Repository: retrieve_places(filters)
Repository->>Database: SELECT * FROM places
Database-->>Repository: places data
Repository-->>Service: results
Service-->>Facade: processed data
Facade-->>API: response
API-->>User: 200 OK
```

---

## 5. Conclusion

This document describes the architecture and design of the HBnB system.

It ensures:
- Clear separation of concerns  
- Scalability  
- Maintainability  

It can be used as a reference for development and future improvements.
