
## 1. Product Pages Components


### ProductList Component

| Component | Purpose |
|-----------|---------|
| **ProductList** | The top-level container for displaying and managing products. Manages pagination, filtering, and coordination among child components. |
| **SearchBar** | Text input field for searching products by name, SKU, or barcode. Triggers real-time search or debounced query. |
| **FilterPanel** | Collapsible panel containing all filter options for narrowing product list. |
| ↳ CategoryFilter | Dropdown tree selector for filtering products by category hierarchy. Supports multi-select. |
| ↳ StockStatusFilter | Radio button or checkbox group for filtering by stock status (In Stock, Low Stock, Out of Stock). |
| ↳ PriceRangeFilter | Dual slider or min/max inputs for filtering products by price range. |
| **ProductTable** | Main data grid displaying product list with sortable columns. Handles row selection and bulk actions. |
| ↳ TableHeader | Sortable column headers for SKU, Name, Category, Stock, Price, Status. |
| ↳ TableRow | Individual product row displaying product information and action buttons. |
| ↳ ProductInfo | Cell displaying product name, SKU, and thumbnail image. |
| ↳ StockInfo | Cell showing current stock quantity with color-coded status indicator (green/yellow/red). |
| ↳ PriceInfo | Cell displaying unit price and cost price with optional markup indicator. |
| ↳ Actions | Action buttons container for row-level operations. |
| ↳ EditButton | Icon button that navigates to product edit form. |
| ↳ DeleteButton | Icon button that opens delete confirmation modal. |
| ↳ ViewButton | Icon button that navigates to product details page. |
| **Pagination** | Controls for navigating through pages of product data. Shows current page, total records, and page size selector. |
| **AddProductButton** | Floating action button or primary button that navigates to product creation form. |
| **DeleteConfirmationModal** | Dialog modal that appears when delete is clicked, requiring confirmation before deletion. Shows product name to prevent accidental deletion. |

---

### ProductForm Component

| Component | Purpose |
|-----------|---------|
| **ProductForm** | The top-level container for creating or editing a product. Manages form state, validation, and submission to API. |
| **FormContainer** | Wrapper component that handles form context, validation schema, and submission handling using React Hook Form. |
| **Tabs** | Tab container organizing product fields into logical sections for better usability. |
| ↳ BasicInfoTab | Tab containing fundamental product identification fields. |
| ↳ SKUInput | Text field for Stock Keeping Unit. Must be unique and follows format validation (e.g., XXX-XXX-XXX). |
| ↳ NameInput | Text field for product display name. Required field with max length validation. |
| ↳ DescriptionInput | Rich text or textarea field for product description. Supports basic formatting. |
| ↳ CategorySelect | Hierarchical dropdown for selecting product category. Supports lazy loading of subcategories. |
| ↳ UnitOfMeasureSelect | Dropdown for selecting unit (e.g., Each, Box, Kg, Meter). |
| **PricingTab** | Tab containing pricing-related fields. |
| ↳ UnitPriceInput | Currency input for selling price. Must be greater than cost price. |
| ↳ CostPriceInput | Currency input for purchase cost. Used for profit margin calculation. |
| ↳ TaxRateSelect | Dropdown for selecting applicable tax rate (e.g., 0%, 5%, 10%, 18%). |
| **InventoryTab** | Tab containing stock management fields. |
| ↳ StockQuantityInput | Numeric input for current stock quantity. Cannot be negative. |
| ↳ ReorderPointInput | Numeric input for low stock threshold. Triggers alerts when stock falls below. |
| ↳ ReorderQuantityInput | Numeric input for suggested reorder quantity. |
| ↳ LocationInput | Text field for warehouse location (e.g., Aisle-Bin-Shelf). |
| **ImagesTab** | Tab for managing product images. |
| ↳ ImageUploader | Drag-and-drop component for uploading product images. Supports multiple formats (JPEG, PNG). |
| ↳ ImageGallery | Grid display of uploaded images with preview thumbnails. |
| ↳ PrimaryImageSelector | Star icon or radio button to mark one image as the primary/thumbnail image. |
| **FormActions** | Container for action buttons at bottom of form. |
| ↳ SubmitButton | Primary button that validates and submits form. Text toggles between "Create Product" and "Update Product". |
| ↳ CancelButton | Secondary button that navigates back to product list without saving. |
| ↳ ResetButton | Text link that resets form to original values (edit mode only). |
| **FieldValidator** | Utility component that displays validation errors inline below each field. |
| **FormErrorDisplay** | Alert banner at top of form showing server-side validation errors or submission failures. |

---

### ProductDetails Component

| Component | Purpose |
|-----------|---------|
| **ProductDetails** | The top-level container for viewing product information. Fetches product data by ID and coordinates display components. |
| **Header** | Top section with navigation and primary actions. |
| ↳ BackButton | Icon button that returns to product list page. |
| ↳ ProductTitle | Heading displaying product name and SKU. |
| ↳ EditButton | Primary action button that navigates to product edit form. |
| ↳ DeleteButton | Danger button that opens delete confirmation modal. |
| **ProductImage** | Gallery component displaying product images with zoom capability. Shows primary image first with thumbnail strip. |
| **InfoCard** | Card component displaying basic product information. |
| ↳ Category | Display of product category with breadcrumb trail. |
| ↳ Description | Full product description with formatting preserved. |
| ↳ Unit of Measure | Display of selected unit of measure. |
| **StockCard** | Card component focused on inventory status. |
| ↳ CurrentStock | Large display of current stock quantity with trend indicator. |
| ↳ ReorderStatus | Badge showing if product is below reorder point. |
| ↳ AdjustStockButton | Button that opens stock adjustment modal for inventory changes. |
| **PriceCard** | Card component showing pricing information. |
| ↳ Unit Price | Display of selling price with currency formatting. |
| ↳ Cost Price | Display of cost price with profit margin calculation. |
| ↳ Tax Rate | Display of applicable tax rate. |
| **TabContainer** | Tab container for secondary product information. |
| ↳ SpecificationsTab | Tab displaying product attributes and specifications. |
| ↳ AttributesList | Table of attribute names and values (e.g., Color: Red, Size: Large). |
| ↳ AddAttributeButton | Button to add custom attributes (edit mode only). |
| ↳ StockMovementsTab | Tab showing inventory change history. |
| ↳ MovementTimeline | Chronological timeline of stock movements with icons for movement type. |
| ↳ MovementTable | Detailed table of stock movements with quantity, type, reference, date, and user. |
| ↳ PurchaseOrdersTab | Tab listing purchase orders that include this product. |
| ↳ PurchaseOrderList | Table of POs with PO number, supplier, quantity, date, and status. |
| ↳ SalesOrdersTab | Tab listing sales orders that include this product. |
| ↳ SalesOrderList | Table of sales orders with order number, customer, quantity, date, and status. |

---


## 2. Purchasing Pages Components

### PurchaseOrderForm Component

| Component | Purpose |
|-----------|---------|
| **PurchaseOrderForm** | The top-level container for creating or editing a purchase order. Manages state, validation, and submission. |
| **Header** | Top section with order identification and primary actions. |
| ↳ Title | Heading displaying "Create Purchase Order" or PO number for edit mode. |
| ↳ StatusBadge | Badge showing current PO status (Draft, Submitted, Approved, Ordered). Read-only in edit mode. |
| ↳ SaveDraftButton | Button that saves the PO without submitting for approval. Keeps status as Draft. |
| **SupplierSection** | Section for selecting and viewing supplier information. |
| ↳ SupplierSelect | Autocomplete search field for selecting supplier by name, code, or tax number. Fetches supplier details on selection. |
| ↳ SupplierInfoCard | Read-only card displaying selected supplier's contact person, phone, email, and address. |
| ↳ PaymentTermsInput | Dropdown or text field for payment terms (e.g., Net 15, Net 30, COD). Pre-populated from supplier defaults. |
| **OrderDetailsSection** | Section for order-level date and note fields. |
| ↳ OrderDatePicker | Date picker for order creation date. Defaults to current date. |
| ↳ DeliveryDatePicker | Date picker for expected delivery date. Validated to be after order date. |
| ↳ NotesInput | Textarea for internal notes about the purchase order. |
| **OrderLinesSection** | Main section for managing purchase order line items. |
| ↳ OrderLinesTable | Editable table displaying all line items with product, quantity, price, and totals. |
| ↳ ProductColumn | Cell displaying selected product with name, SKU, and unit of measure. |
| ↳ QuantityColumn | Numeric input for ordered quantity with increment/decrement buttons. |
| ↳ PriceColumn | Currency input for unit price. May show last purchase price as reference. |
| ↳ TotalColumn | Read-only cell displaying line total (quantity × unit price). |
| ↳ RemoveButton | Icon button to delete line item from order. |
| ↳ AddProductButton | Button that opens product selector modal to add new line items. |
| ↳ ProductSelectorModal | Modal dialog for searching and selecting products to add to order. Shows product list with search and filters. |
| **SummarySection** | Section displaying order financial summary. |
| ↳ Subtotal | Display of sum of all line totals before taxes and shipping. |
| ↳ TaxAmount | Display of calculated tax amount based on product tax rates. |
| ↳ ShippingCostInput | Currency input for shipping cost. Updates grand total. |
| ↳ TotalAmount | Large display of final order total (subtotal + tax + shipping). |
| **FormActions** | Container for action buttons at bottom of form. |
| ↳ CancelButton | Secondary button that navigates back without saving. |
| ↳ SaveButton | Button that saves current form state (draft mode). |
| ↳ SubmitButton | Primary button that validates and submits PO for approval. Changes status to Submitted. |

---

### SupplierList Component

| Component | Purpose |
|-----------|---------|
| **SupplierList** | The top-level container for displaying and managing suppliers. Manages view mode, filtering, and navigation. |
| **Header** | Top section with page title and primary actions. |
| ↳ Title | Heading displaying "Suppliers" with total count. |
| ↳ AddSupplierButton | Primary button that navigates to supplier creation form. |
| **SearchBar** | Text input field for searching suppliers by name, code, contact person, or tax number. Triggers debounced search. |
| **FilterBar** | Horizontal row of quick filters for narrowing supplier list. |
| ↳ ActiveFilter | Toggle or radio button to show active/inactive/all suppliers. |
| ↳ CityFilter | Dropdown for filtering suppliers by city location. |
| ↳ RatingFilter | Star rating filter for supplier performance rating (1-5 stars). |
| ↳ LeadTimeFilter | Numeric range filter for lead time in days. |
| **ViewToggle** | Button group for switching between grid and list views. |
| ↳ GridView | Icon button that switches to card-based grid layout. |
| ↳ ListView | Icon button that switches to table-based list layout. |
| **SupplierGrid** | Container displaying suppliers as cards (visible when grid view selected). |
| ↳ SupplierCard | Individual supplier card displaying summary information. |
| ↳ Name | Heading with supplier name and code. |
| ↳ Rating | Star rating display showing supplier performance. |
| ↳ ContactPerson | Text displaying primary contact name. |
| ↳ Phone | Text displaying contact phone number with click-to-call link. |
| ↳ Email | Text displaying email address with mailto link. |
| ↳ Actions | Button group for card-level operations. |
| ↳ ViewButton | Button that navigates to supplier details page. |
| ↳ EditButton | Button that navigates to supplier edit form. |
| ↳ CreatePOButton | Button that navigates to PO form with this supplier pre-selected. |
| **SupplierTable** | Table displaying suppliers in list format (visible when list view selected). |
| ↳ TableHeader | Sortable column headers for Code, Name, Contact, Phone, City, Rating, Status. |
| ↳ TableRow | Individual supplier row with all columns and action buttons. |
| **Pagination** | Controls for navigating through pages of supplier data. Shows page size selector and current range. |

---

### SupplierDetails Component

| Component | Purpose |
|-----------|---------|
| **SupplierDetails** | The top-level container for viewing detailed supplier information. Fetches supplier data by ID and coordinates display. |
| **Header** | Top section with navigation and primary actions. |
| ↳ BackButton | Icon button that returns to supplier list. |
| ↳ SupplierName | Heading displaying supplier name and code. |
| ↳ EditButton | Button that navigates to supplier edit form. |
| ↳ DeleteButton | Danger button that opens delete confirmation modal (checks for open POs first). |
| ↳ CreatePOButton | Primary button that navigates to PO form with this supplier pre-selected. |
| **InfoCards** | Grid of cards displaying supplier information. |
| ↳ ContactCard | Card displaying contact information. |
| ↳ ContactPerson | Name of primary and secondary contacts. |
| ↳ Email | Email addresses with mailto links. |
| ↳ Phone | Phone numbers with click-to-call links. |
| ↳ AddressCard | Card displaying physical and mailing addresses. |
| ↳ Street Address | Full street address with line breaks. |
| ↳ City, State, ZIP | Geographic location details. |
| ↳ Country | Country of supplier. |
| ↳ PaymentTermsCard | Card displaying payment and banking information. |
| ↳ Payment Terms | Standard payment terms (e.g., Net 30). |
| ↳ Tax Number | VAT or tax identification number. |
| ↳ Bank Account | Bank name, account number, and routing info. |
| ↳ PerformanceCard | Card displaying supplier performance metrics. |
| ↳ Rating | Star rating with average score. |
| ↳ Total Orders | Count of total purchase orders placed. |
| ↳ On-Time Delivery | Percentage of orders delivered on or before due date. |
| ↳ Average Lead Time | Average days from order to delivery. |
| **TabContainer** | Tab container for detailed supplier information. |
| ↳ PurchaseOrdersTab | Tab displaying all purchase orders for this supplier. |
| ↳ POFilter | Filter controls for PO list (status, date range). |
| ↳ POTable | Table of purchase orders with PO number, date, total, status, and actions. |
| ↳ PONumber | Link to PO details page. |
| ↳ Date | Order date and expected delivery date. |
| ↳ Status | Status badge with color coding. |
| ↳ Total | Order total amount. |
| ↳ ViewButton | Button to view PO details. |
| ↳ ProductsTab | Tab displaying products supplied by this supplier. |
| ↳ ProductList | Grid or list of products showing product name, SKU, unit price, and lead time. |
| ↳ AnalyticsTab | Tab displaying performance analytics and charts. |
| ↳ DeliveryChart | Bar or line chart showing delivery performance over time. |
| ↳ OrderVolumeChart | Chart showing order volume by month. |
| ↳ QualityRatingChart | Chart showing quality ratings from receipts. |
| **DeleteConfirmationModal** | Modal that appears when delete is clicked, requiring confirmation and showing count of open POs that would be affected. |
| **CreatePOModal** | Optional modal that allows quick creation of PO with minimal fields before navigating to full form. |