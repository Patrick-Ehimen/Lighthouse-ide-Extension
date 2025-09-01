# Service Architecture Diagrams

## Service Layer Architecture

```mermaid
graph TB
    subgraph "Service Layer"
        subgraph "Core Services"
            AUTH[Authentication Service]
            UPLOAD[Upload Service]
            DOWNLOAD[Download Service]
            ENCRYPT[Encryption Service]
        end

        subgraph "Management Services"
            METADATA[Metadata Service]
            CONFIG[Configuration Service]
            STATE[State Management Service]
            CACHE[Cache Service]
        end

        subgraph "Integration Services"
            LIGHTHOUSE[Lighthouse SDK Service]
            KAVACH[Kavach SDK Service]
            FILESYSTEM[File System Service]
            NOTIFICATION[Notification Service]
        end
    end

    subgraph "External Dependencies"
        VSCODE_API[VSCode API]
        LIGHTHOUSE_NETWORK[Lighthouse Network]
        IPFS_NETWORK[IPFS Network]
        BLOCKCHAIN[Blockchain Networks]
    end

    %% Core Service Dependencies
    UPLOAD --> AUTH
    UPLOAD --> ENCRYPT
    UPLOAD --> LIGHTHOUSE
    UPLOAD --> METADATA
    UPLOAD --> NOTIFICATION

    DOWNLOAD --> AUTH
    DOWNLOAD --> ENCRYPT
    DOWNLOAD --> LIGHTHOUSE
    DOWNLOAD --> CACHE
    DOWNLOAD --> FILESYSTEM

    ENCRYPT --> KAVACH
    ENCRYPT --> AUTH

    AUTH --> CONFIG
    AUTH --> LIGHTHOUSE

    %% Management Service Dependencies
    METADATA --> STATE
    METADATA --> CACHE
    CONFIG --> VSCODE_API
    STATE --> VSCODE_API
    CACHE --> FILESYSTEM

    %% Integration Service Dependencies
    LIGHTHOUSE --> LIGHTHOUSE_NETWORK
    KAVACH --> BLOCKCHAIN
    NOTIFICATION --> VSCODE_API
    FILESYSTEM --> VSCODE_API

    %% External Dependencies
    LIGHTHOUSE_NETWORK --> IPFS_NETWORK

    %% Styling
    classDef coreService fill:#e3f2fd
    classDef managementService fill:#f1f8e9
    classDef integrationService fill:#fff8e1
    classDef externalService fill:#fce4ec

    class AUTH,UPLOAD,DOWNLOAD,ENCRYPT coreService
    class METADATA,CONFIG,STATE,CACHE managementService
    class LIGHTHOUSE,KAVACH,FILESYSTEM,NOTIFICATION integrationService
    class VSCODE_API,LIGHTHOUSE_NETWORK,IPFS_NETWORK,BLOCKCHAIN externalService
```

## Authentication Service Flow

```mermaid
stateDiagram-v2
    [*] --> Unauthenticated

    Unauthenticated --> ApiKeyAuth : setApiKey()
    Unauthenticated --> WalletAuth : connectWallet()

    ApiKeyAuth --> Validating : validateApiKey()
    WalletAuth --> Connecting : establishConnection()

    Validating --> Authenticated : validation success
    Validating --> Unauthenticated : validation failed

    Connecting --> SigningMessage : wallet connected
    SigningMessage --> GettingJWT : message signed
    GettingJWT --> Authenticated : JWT received
    GettingJWT --> Unauthenticated : JWT failed

    Authenticated --> RefreshingToken : token expiring
    RefreshingToken --> Authenticated : refresh success
    RefreshingToken --> Unauthenticated : refresh failed

    Authenticated --> Unauthenticated : logout()
```

## Upload Service Architecture

```mermaid
graph TB
    subgraph "Upload Service Components"
        UPLOAD_CONTROLLER[Upload Controller]
        FILE_PROCESSOR[File Processor]
        ENCRYPTION_HANDLER[Encryption Handler]
        PROGRESS_TRACKER[Progress Tracker]
        BATCH_MANAGER[Batch Manager]
    end

    subgraph "Upload Pipeline"
        VALIDATE[Validate File]
        PREPROCESS[Preprocess File]
        ENCRYPT[Encrypt (Optional)]
        CHUNK[Chunk Large Files]
        UPLOAD[Upload to IPFS]
        VERIFY[Verify Upload]
        STORE_METADATA[Store Metadata]
    end

    subgraph "External Services"
        LIGHTHOUSE_SDK[Lighthouse SDK]
        KAVACH_SDK[Kavach SDK]
        METADATA_SERVICE[Metadata Service]
        NOTIFICATION_SERVICE[Notification Service]
    end

    UPLOAD_CONTROLLER --> FILE_PROCESSOR
    UPLOAD_CONTROLLER --> ENCRYPTION_HANDLER
    UPLOAD_CONTROLLER --> PROGRESS_TRACKER
    UPLOAD_CONTROLLER --> BATCH_MANAGER

    FILE_PROCESSOR --> VALIDATE
    VALIDATE --> PREPROCESS
    PREPROCESS --> ENCRYPT
    ENCRYPT --> CHUNK
    CHUNK --> UPLOAD
    UPLOAD --> VERIFY
    VERIFY --> STORE_METADATA

    ENCRYPTION_HANDLER --> KAVACH_SDK
    FILE_PROCESSOR --> LIGHTHOUSE_SDK
    PROGRESS_TRACKER --> NOTIFICATION_SERVICE
    BATCH_MANAGER --> METADATA_SERVICE
```

## Download Service Architecture

```mermaid
graph TB
    subgraph "Download Service Components"
        DOWNLOAD_CONTROLLER[Download Controller]
        CID_RESOLVER[CID Resolver]
        DECRYPTION_HANDLER[Decryption Handler]
        STREAM_MANAGER[Stream Manager]
        CACHE_MANAGER[Cache Manager]
    end

    subgraph "Download Pipeline"
        RESOLVE_CID[Resolve CID]
        CHECK_CACHE[Check Cache]
        CHECK_ACCESS[Check Access Rights]
        FETCH_METADATA[Fetch Metadata]
        DOWNLOAD_FILE[Download File]
        DECRYPT[Decrypt (If Encrypted)]
        SAVE_LOCAL[Save to Local]
        UPDATE_CACHE[Update Cache]
    end

    subgraph "External Services"
        LIGHTHOUSE_SDK[Lighthouse SDK]
        KAVACH_SDK[Kavach SDK]
        IPFS_GATEWAY[IPFS Gateway]
        LOCAL_STORAGE[Local Storage]
    end

    DOWNLOAD_CONTROLLER --> CID_RESOLVER
    DOWNLOAD_CONTROLLER --> DECRYPTION_HANDLER
    DOWNLOAD_CONTROLLER --> STREAM_MANAGER
    DOWNLOAD_CONTROLLER --> CACHE_MANAGER

    CID_RESOLVER --> RESOLVE_CID
    RESOLVE_CID --> CHECK_CACHE
    CHECK_CACHE --> CHECK_ACCESS
    CHECK_ACCESS --> FETCH_METADATA
    FETCH_METADATA --> DOWNLOAD_FILE
    DOWNLOAD_FILE --> DECRYPT
    DECRYPT --> SAVE_LOCAL
    SAVE_LOCAL --> UPDATE_CACHE

    DECRYPTION_HANDLER --> KAVACH_SDK
    STREAM_MANAGER --> IPFS_GATEWAY
    CACHE_MANAGER --> LOCAL_STORAGE
    CID_RESOLVER --> LIGHTHOUSE_SDK
```

## State Management Architecture

```mermaid
graph TB
    subgraph "State Management Layer"
        STATE_STORE[Central State Store]
        STATE_REDUCERS[State Reducers]
        STATE_SELECTORS[State Selectors]
        STATE_MIDDLEWARE[Middleware]
    end

    subgraph "State Domains"
        AUTH_STATE[Authentication State]
        FILE_STATE[File Management State]
        UI_STATE[UI State]
        CONFIG_STATE[Configuration State]
        CACHE_STATE[Cache State]
    end

    subgraph "State Persistence"
        WORKSPACE_STORAGE[Workspace Storage]
        GLOBAL_STORAGE[Global Storage]
        MEMORY_CACHE[Memory Cache]
    end

    subgraph "State Consumers"
        UI_COMPONENTS[UI Components]
        SERVICES[Services]
        COMMANDS[Commands]
    end

    STATE_STORE --> STATE_REDUCERS
    STATE_STORE --> STATE_SELECTORS
    STATE_STORE --> STATE_MIDDLEWARE

    STATE_REDUCERS --> AUTH_STATE
    STATE_REDUCERS --> FILE_STATE
    STATE_REDUCERS --> UI_STATE
    STATE_REDUCERS --> CONFIG_STATE
    STATE_REDUCERS --> CACHE_STATE

    STATE_MIDDLEWARE --> WORKSPACE_STORAGE
    STATE_MIDDLEWARE --> GLOBAL_STORAGE
    STATE_MIDDLEWARE --> MEMORY_CACHE

    STATE_SELECTORS --> UI_COMPONENTS
    STATE_SELECTORS --> SERVICES
    STATE_SELECTORS --> COMMANDS
```
