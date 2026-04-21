1. Introduction

This document presents a detailed technical description of the HBnB application’s architecture and design. Its purpose is to guide the development process by clearly outlining the system structure, business logic, and interaction mechanisms.

The document covers:

High-Level Architecture (Package Diagram)
Business Logic Layer (Class Diagram)
API Interaction Flow (Sequence Diagrams)

The main objective is to achieve a system that is easy to understand, scalable, and maintainable.

2. High-Level Architecture
Package Diagram
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
Explanation

The system is designed using a three-tier architecture:

Presentation Layer
Responsible for handling user interactions (UI and API)
Sends incoming requests to the backend
Business Logic Layer
Contains the main logic of the application
Includes services, models, and a facade to coordinate operations
Persistence Layer
Handles data storage and retrieval
Uses repositories to interact with the database
Facade Pattern

The Facade acts as a central access point to the Business Logic Layer.
It simplifies communication between layers and reduces direct dependencies.

3. Business Logic Layer
Class Diagram
Explanation

The system is designed using a three-tier architecture:

Presentation Layer
Responsible for handling user interactions (UI and API)
Sends incoming requests to the backend
Business Logic Layer
Contains the main logic of the application
Includes services, models, and a facade to coordinate operations
Persistence Layer
Handles data storage and retrieval
Uses repositories to interact with the database
Facade Pattern

The Facade acts as a central access point to the Business Logic Layer.
It simplifies communication between layers and reduces direct dependencies.

3. Business Logic Layer
Class Diagram
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
Explanation
Entities
User: Represents users of the platform
Place: Represents listed properties
Review: Stores user feedback about places
Amenity: Represents available features (e.g., Wi-Fi, parking)
Design Choices
Each entity includes:
A unique identifier (UUID)
Timestamps for creation and updates
Standard CRUD operations are provided for each entity
Relationships
A User can own multiple Places
A User can create multiple Reviews
A Place can have multiple Reviews
A Place can be associated with multiple Amenities (many-to-many)
Each Review is linked to both a User and a Place
4. API Interaction Flow
4.1 User Registration
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
Explanation

All API requests follow a unified flow:

User → API → Facade → Service → Repository → Database → Response

Key Points
API Layer receives and processes incoming requests
Facade acts as an intermediary for business operations
Service Layer handles validation and core logic
Repository Layer manages data access
Database stores all persistent information
5. Conclusion

This document outlines the overall architecture and design of the HBnB system in a clear and structured way.

It helps ensure:

Proper separation between system layers
Scalability and ease of maintenance
Consistent behavior across different components

The diagrams and explanations can be used as a solid reference during development and future improvements.
