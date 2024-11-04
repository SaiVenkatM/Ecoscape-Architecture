``` mermaid
    sequenceDiagram
    participant Frontend
    participant Bottle Server
    participant TileComputer
    participant GCS
    participant GEE
    participant Cloud Tasks

    Frontend->>Bottle Server: POST /compute_tile
    Note over Bottle Server: Validate API Key
    Bottle Server->>TileComputer: compute_tile()
    
    rect rgb(2, 20, 200)
        Note over TileComputer: Clean temp directory
        TileComputer->>TileComputer: Initialize TileComputeEngine
        TileComputer->>TileComputer: Compute Tile
    end

    alt do_connectivity = true
        TileComputer->>Frontend: Callback with results
    else do_connectivity = false
        TileComputer->>Cloud Tasks: Create connectivity computation task
        Cloud Tasks-->>Frontend: Task created response
    end
```