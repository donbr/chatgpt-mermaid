# C4 Sample Diagrams

## Level 1: System Context Diagram

```mermaid
  graph LR
    subgraph "System"
      System[My System] --> Service1
      System --> Service2
      System --> Service3
    end
    subgraph "External"
      User((User)) --> System
      OtherService((Other Service)) --> System
    end

```


## Level 2: Container Diagram

```mermaid
  graph LR
    subgraph "User"
      User((User)) --> WebBrowser
    end
    subgraph "Web Browser"
      WebBrowser[Web Browser] --> "Web Application"("Web Application - Node.js")
    end
    subgraph "Web Application"
      "Web Application" --> "API"("API - Node.js")
    end
    subgraph "API"
      "API" --> "Database"("Database - PostgreSQL")
    end

```

## Level 3: Component Diagram

```mermaid
  graph LR
    subgraph "Web Application"
      "Web Application" --> "Authentication"("Authentication - Node.js")
      "Web Application" --> "Authorization"("Authorization - Node.js")
    end
    subgraph "API"
      "API" --> "Authentication"("Authentication - Node.js")
      "API" --> "Authorization"("Authorization - Node.js")
      "API" --> "Database"("Database - PostgreSQL")
    end

```

## Level 4: Code Diagram

```mermaid
  graph LR
    subgraph "Authentication"
      "Authentication" --> "Auth Controller"("Auth Controller - Node.js")
      "Authentication" --> "Auth Service"("Auth Service - Node.js")
      "Auth Controller" --> "Auth Repository"("Auth Repository - Node.js")
      "Auth Service" --> "Auth Repository"
    end
    subgraph "Authorization"
      "Authorization" --> "Auth Controller"("Auth Controller - Node.js")
      "Authorization" --> "Auth Service"("Auth Service - Node.js")
      "Auth Controller" --> "Auth Repository"("Auth Repository - Node.js")
      "Auth Service" --> "Auth Repository"
    end
    subgraph "API"
      "API" --> "Authentication"
      "API" --> "Authorization"
    end

```