```mermaid
classDiagram
    class Product {
        +UUID id
        +String sku
        +String name
        +String description
        +BigDecimal unitPrice
        +Integer currentStock
        +Integer reorderLevel
        +Integer reorderQuantity
        +String unitOfMeasure
        +String imageUrl
        +Boolean active
        +LocalDateTime createdAt
        +LocalDateTime updatedAt
        +create()
        +update()
        +isLowStock()
        +adjustStock(quantity)
    }
    
    class Category {
        +UUID id
        +String name
        +String description
        +UUID parentId
        +Integer sortOrder
        +Boolean active
        +addSubCategory()
        +getFullPath()
    }
    
    class StockMovement {
        +UUID id
        +UUID productId
        +Integer quantity
        +MovementType type
        +String referenceType
        +UUID referenceId
        +Integer previousStock
        +Integer newStock
        +String notes
        +LocalDateTime createdAt
        +UUID createdBy
        +calculateStockChange()
    }
    
    class MovementType {
        <<enumeration>>
        INBOUND
        OUTBOUND
        ADJUSTMENT
        RETURN
        DAMAGED
    }
    
    Product "1" -- "1" Category : belongs to
    Product "1" -- "*" StockMovement : has
```