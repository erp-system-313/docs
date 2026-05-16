## 1. SalesOrderForm

| Component | Purpose |
|-----------|---------|
| **SalesOrderForm** | The top-level container for creating/editing a sales order. Manages overall state, validation, and coordination among child components. |
| **OrderHeader** | Groups order-level fields: customer lookup, order dates, and order status. |
| ↳ CustomerLookup | Auto‑complete or search field to select an existing customer. May also allow quick creation of a new customer. |
| ↳ DatePickers | Fields for order date and required delivery date, often using calendar widgets with validation (e.g., required date not before order date). |
| ↳ OrderStatus | Badge or dropdown showing current status (draft, submitted, approved). In edit mode it may be read‑only. |
| **LineItemsGrid** | Editable table for adding/removing order lines. Handles product selection, quantities, pricing, and real‑time calculation of line totals. |
| ↳ ProductAutocomplete | Search field to find products by code or name. Fetches product details (price, description) and populates the row. |
| ↳ QuantitySpinner | Numeric input with increment/decrement buttons for quantity. Triggers recalculation of line total. |
| ↳ PriceEditor | Field for unit price, often with overrides allowed (if permissions permit). May show discount field. |
| ↳ Add/RemoveRow | Buttons to insert a new blank line or delete the current line. |
| **TotalsPanel** | Displays order summary calculations, updated dynamically as line items change. |
| ↳ SubtotalDisplay | Sum of line net amounts before taxes and discounts. |
| ↳ TaxSummary | Breakdown of taxes applied (e.g., per tax rate). |
| ↳ GrandTotal | Final total after all additions/subtractions. |
| **FooterActions** | Action buttons that submit the order or move it to the next stage. |
| ↳ SaveDraftButton | Saves the order without triggering approval workflows; remains editable. |
| ↳ SubmitForApproval | Changes status to “submitted” and may initiate an approval process. |
| ↳ CreateInvoiceButton | (Optional) Creates an invoice directly from the sales order if the order is ready. |
| **AdditionalDetailsTabs** | Tab container for extra order information that doesn’t fit in the main view. |
| ↳ ShippingTab | Fields for shipping address, carrier, tracking number. |
| ↳ PaymentTermsTab | Dropdown for payment terms (e.g., Net 30) and possibly credit card details. |
| ↳ NotesTab | Rich text or plain text field for internal notes or customer instructions. |
| ↳ AttachmentsTab | List of uploaded files (e.g., signed contracts, specs) with upload capability. |

---

## 2. CustomersList

| Component | Purpose |
|-----------|---------|
| **CustomersList** | Main container for listing and managing customer records. Provides search, filter, and bulk actions. |
| **GlobalSearch** | A single text input that searches across multiple customer fields (name, ID, phone, email) and updates the grid instantly. |
| **AdvancedFiltersPanel** | Collapsible panel with structured filters for refined search. |
| ↳ StatusDropdown | Filter by active/inactive or other custom statuses. |
| ↳ CreditRangeFilter | Slider or min/max inputs to filter customers by credit limit. |
| ↳ CreationDateRange | Date picker range to find customers added within a period. |
| **CustomerDataGrid** | The main table displaying customer records with interactive features. |
| ↳ ConfigurableColumns | Allows users to show/hide or reorder columns (e.g., ID, name, phone, credit limit, last order). |
| ↳ PaginationControls | Buttons and page size selector to navigate through large datasets. |
| ↳ RowActionMenu | Icons or a dropdown per row for quick actions. |
| ↳ ViewDetails | Opens the customer details page (or modal) for the selected customer. |
| ↳ EditInline | Enables inline editing of fields (e.g., status) without leaving the list. |
| ↳ DeleteAction | Deletes the customer after confirmation (may be restricted by permissions). |
| **ActionToolbar** | Top bar with primary actions that affect the whole list or multiple selected rows. |
| ↳ AddCustomerButton | Navigates to a blank customer creation form. |
| ↳ BulkOperationsMenu | Dropdown with actions like export selected, delete selected, change status in bulk. |

---

## 3. CustomerDetails

| Component | Purpose |
|-----------|---------|
| **CustomerDetails** | Central hub for viewing and editing all information related to a single customer. |
| **CustomerHeader** | Top section showing customer name, status badge, and key contact details (e.g., primary phone/email). |
| **DetailTabs** | Tab container that organizes customer data into logical sections. |
| ↳ GeneralInfo | Editable form with fields like addresses, tax ID, payment terms, credit limit. |
| ↳ EditableFields | Inputs, selects, and checkboxes for all general information; can be switched to edit mode. |
| ↳ SaveCancelButtons | Appear in edit mode to confirm or discard changes. |
| ↳ ContactsList | Grid of contact persons (name, role, email, phone). |
| ↳ ContactsDataGrid | Table of contacts with actions to edit, delete, or add a new contact. |
| ↳ AddContactButton | Opens a modal or inline form to create a new contact. |
| ↳ SalesHistoryGrid | Displays recent sales orders, invoices, and quotes linked to this customer. |
| ↳ LinkedSalesOrders | Clickable rows that navigate to the respective sales order form. |
| ↳ LinkedInvoices | Clickable rows that navigate to invoice details. |
| ↳ ActivityTimeline | Chronological list of interactions (emails, calls, notes) with timestamps and user info. |
| ↳ TimelineItems | Each item shows an icon, description, and date; may support adding new notes. |
| ↳ AttachmentList | List of documents uploaded for this customer. |
| ↳ FileUpload | Drag‑and‑drop or button to upload new files (e.g., contracts, credit checks). |

---

## 4. InvoiceList

| Component | Purpose |
|-----------|---------|
| **InvoiceList** | Main view for managing sales invoices: search, filter, and perform actions. |
| **QuickFilters** | Predefined filter chips or dropdowns for common criteria. |
| ↳ StatusChips | Buttons to show invoices by status (paid, overdue, draft). |
| ↳ DateRangePicker | Quick selection for invoice date range (e.g., last 7 days, this month). |
| ↳ CustomerGroupFilter | Dropdown to filter by customer segment or individual customer. |
| **InvoiceDataGrid** | Table showing key invoice data with row‑specific actions. |
| ↳ Columns | Standard columns: invoice number, date, due date, customer, amount, balance due, status. |
| ↳ RowActionButtons | Icons for common operations per invoice. |
| ↳ ViewPDF | Opens a PDF preview or downloads the invoice PDF. |
| ↳ Edit | Navigates to the invoice form for editing (only if draft or not yet posted). |
| ↳ CreateCreditNote | Opens a credit note creation form pre‑populated with invoice details. |
| ↳ RecordPaymentModal | A modal to enter payment amount, date, and reference without leaving the list. |
| **BulkActionBar** | Appears when one or more invoices are selected; contains mass operations. |
| ↳ SendEmailBatch | Sends invoices as PDF attachments to selected customers. |
| ↳ ExportToCSV | Exports selected invoices to a CSV file. |
| ↳ MarkAsPaid | Bulk updates status to paid (useful for offline payments). |

---

## 5. InvoiceForm

| Component | Purpose |
|-----------|---------|
| **InvoiceForm** | Form for creating/editing an invoice, often derived from a sales order. |
| **InvoiceHeader** | Contains top‑level invoice metadata. |
| ↳ NumberField | Invoice number (auto‑generated or user‑editable based on settings). |
| ↳ DatePickers | Invoice date and due date. |
| ↳ CustomerLookup | Search/select customer; may be locked if created from an order. |
| ↳ OrderReference | Reference to the source sales order or quote (display‑only or selectable). |
| **LineItemsGrid** | Editable table for invoice lines. |
| ↳ ProductAutocomplete | Select product; populates description, unit price, tax code. |
| ↳ GLAccountPicker | Accounting‑specific: choose general ledger account for revenue (or other account types). |
| ↳ TaxCodeSelector | Dropdown to select applicable tax rate for the line. |
| ↳ Add/RemoveRow | Buttons to add/delete lines. |
| **TotalsAndTaxBreakdown** | Displays subtotal, taxes per rate, and grand total. |
| ↳ Subtotal | Sum of line net amounts. |
| ↳ TaxLinesList | List of tax categories with amounts. |
| ↳ GrandTotal | Final total after taxes. |
| **FooterActions** | Buttons for finalising the invoice. |
| ↳ SaveButton | Saves as draft. |
| ↳ Submit/Post | Posts the invoice (makes it non‑editable and creates accounting entries). |
| ↳ Print | Triggers browser print of a PDF‑formatted invoice. |
| ↳ SendByEmail | Opens a modal to send the invoice PDF to the customer. |
| ↳ RecordPaymentButton | Opens a modal to record a payment against this invoice. |
| **PDFPreviewPanel** | Right‑side or embedded panel that shows a real‑time PDF preview of the invoice. |
| ↳ EmbeddedPDFViewer | Renders a PDF based on current data; updates as user changes the invoice. |

---

## 6. JournalEntries

| Component | Purpose |
|-----------|---------|
| **JournalEntries** | Manages the double‑entry accounting records. It has two main sub‑components: a list view and a form view. |
| **JournalEntryList** | Displays a list of existing journal entries. |
| ↳ Search & FilterBar | Fields to filter by date range, journal type, description, or status (draft/posted). |
| ↳ JournalEntryDataGrid | Table showing entry number, date, description, total debit, total credit, status. |
| ↳ RowActions | Icons to view/edit (if draft), reverse (if posted), or duplicate an entry. |
| **JournalEntryForm** | Form for creating or editing a journal entry. |
| ↳ EntryHeader | Top section with general entry information. |
| ↳ DatePicker | Accounting date of the entry. |
| ↳ DescriptionField | Brief narrative for the entire entry. |
| ↳ ReferenceField | Optional reference number (e.g., invoice number). |
| ↳ JournalTypeDropdown | Selector for journal type (sales, purchase, adjustment, etc.). |
| ↳ EntryLinesGrid | Editable table where each line is a debit or credit to a GL account. |
| ↳ AccountAutocomplete | Search for GL account by code or name; shows account type. |
| ↳ DebitInput | Numeric field for debit amount. |
| ↳ CreditInput | Numeric field for credit amount. |
| ↳ LineDescription | Optional description for the individual line. |
| ↳ AddLineButton | Adds a new blank row. |
| ↳ DebitCreditTotals | Displays total debits and total credits side by side. |
| ↳ ImbalanceWarning | Shows a warning message if total debits ≠ total credits; disables posting. |
| ↳ EntryActions | Footer buttons. |
| ↳ SaveDraft | Saves entry without affecting general ledger balances. |
| ↳ PostButton | Posts the entry (makes it permanent, updates ledger balances, and locks editing). |
| ↳ ReverseButton | Creates a reversing entry for a posted journal (if allowed). |

---
