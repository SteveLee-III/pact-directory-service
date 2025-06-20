## Organization Access Request Flow

```mermaid
sequenceDiagram
    participant A as Organization A
    participant P as PACT Directory
    participant B as Organization B

    A->>P: Request access to PCF data from Org. B
    P->>B: Send email notification
    B->>P: Grant or deny access
```

## PACT IM OAuth Sequence Diagram

```mermaid
sequenceDiagram
    participant Client as Data Recipient
    participant OAuthProvider as PACT IM OAuth Provider
    participant API as Data Owner

    Client->>OAuthProvider: Exchange client_id, client_secret, and network identifier for Access Token
    OAuthProvider->>Client: Respond with Access Token

    Client->>API: Request with Access Token (e.g., GET /2/footprints?limit=10)
    API->>Client: Respond with Data
```

## Async Bidirectional Flow

```mermaid
sequenceDiagram
    participant Client as Data Recipient
    participant OAuthProvider as PACT IM OAuth Provider
    participant API as Data Owner

    Client->>OAuthProvider: Exchange client_id, client_secret, and network identifier for Access Token
    OAuthProvider->>Client: Respond with Access Token

    Client->>API: Request Create Event Action (e.g., POST /2/events)
    API->>Client: Respond with created event data

    API->>OAuthProvider: Exchange client_id, client_secret, and api url for Access Token
    OAuthProvider->>API: Respond with Access Token

    API->>Client: POST Event Action (e.g., POST /2/eventssubpath)
    API->>Client: Respond with created PCF data
```

## mTLS Flow

```mermaid
sequenceDiagram
    participant Service A as Data Recipient
    participant Service B as Data Owner

    Note over Service A,Service B: Both services have certificates signed by the same internal CA<br>and trust each other’s certificates directly.

    Service A->>Service B: Initiates mTLS connection
    Service B-->>Service A: Sends server certificate
    Service A->>Service A: Validates Service B's certificate against trusted root store

    Service A-->>Service B: Sends client certificate
    Service B->>Service B: Validates Service A's certificate against trusted root store

    Note over Service A,Service B: If both validations succeed, mTLS connection is established

    Service A->>Service B: Sends secured request
    Service B-->>Service A: Sends secured

    Note over Service A,Service B: Ongoing encrypted communication over TLS connection
```
