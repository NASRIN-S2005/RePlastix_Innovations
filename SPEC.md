# RePlastix Dashboard - Specification

## Project Overview

- **Project Name**: RePlastix Dashboard
- **Type**: Single Page Application (SPA) - Plastic Waste Inventory Management
- **Core Functionality**: Simulated inventory system with order creation, replenishment requests, and approval workflows
- **Target Users**: Warehouse managers, inventory staff

## Tech Stack

- React (Functional Components)
- TypeScript
- Tailwind CSS
- Vite
- Zustand (state management)

---

## UI/UX Specification

### Color Palette

| Role | Color | Hex |
|------|-------|-----|
| Primary | Deep Teal | `#0D9488` |
| Primary Dark | Dark Teal | `#0F766E` |
| Primary Light | Light Teal | `#5EEAD4` |
| Secondary | Slate | `#475569` |
| Accent | Amber | `#F59E0B` |
| Success | Emerald | `#10B981` |
| Warning | Orange | `#F97316` |
| Error | Red | `#EF4444` |
| Background | Off-White | `#F8FAFC` |
| Card BG | White | `#FFFFFF` |
| Text Primary | Slate 900 | `#0F172A` |
| Text Secondary | Slate 500 | `#64748B` |
| Border | Slate 200 | `#E2E8F0` |

### Typography

- **Font Family**: "Inter", system-ui, sans-serif
- **Headings**:
  - H1: 28px, font-weight 700
  - H2: 22px, font-weight 600
  - H3: 18px, font-weight 600
- **Body**: 14px, font-weight 400
- **Small**: 12px, font-weight 400

### Layout & Spacing

- **Max Width**: 1280px, centered
- **Sidebar Width**: 240px (fixed)
- **Main Content**: Fluid, padding 24px
- **Card Padding**: 20px
- **Grid Gap**: 16px
- **Border Radius**: 8px (cards), 6px (buttons), 4px (inputs)

### Responsive Breakpoints

- Mobile: < 768px (sidebar collapses to hamburger)
- Tablet: 768px - 1024px
- Desktop: > 1024px

### Components

#### Sidebar Navigation
- Logo at top
- Nav items: Dashboard, Products, Orders, Replenishments
- Active state: teal background with white text
- Hover state: light teal background

#### Cards
- White background
- Subtle shadow (0 1px 3px rgba(0,0,0,0.1))
- Border radius 8px

#### Buttons
- Primary: Teal background, white text
- Secondary: White background, slate border
- Danger: Red background, white text
- Disabled: Gray background, reduced opacity

#### Badges/Pills
- Small rounded pills
- Padding 4px 12px
- Font size 12px
- Colors based on status

#### Tables
- Striped rows (alternating white/slate-50)
- Header: Slate-100 background
- Hover: Slate-50

#### Forms
- Input fields with border
- Focus: Teal ring
- Labels above inputs

#### Toasts/Popups
- Fixed position bottom-right
- Slide in animation
- Auto-dismiss after 3s

---

## Functionality Specification

### Data Structures

#### Product
```typescript
interface Product {
  id: string;
  name: string;
  category: string;
  stock: number;
  threshold: number;
  unit: string;
}
```

#### Order
```typescript
interface Order {
  id: string;
  productId: string;
  productName: string;
  quantity: number;
  status: 'Completed' | 'Low Stock';
  createdAt: string;
}
```

#### ReplenishmentRequest
```typescript
interface ReplenishmentRequest {
  id: string;
  productId: string;
  productName: string;
  status: 'Pending' | 'Approved' | 'Rejected';
  requestedAt: string;
  quantityNeeded: number;
}
```

### Features

#### 1. Dashboard Page
- Stats cards showing:
  - Total Products (count)
  - Low Stock Items (count with warning)
  - Total Orders (count)
  - Pending Requests (count)
- Quick links to other sections

#### 2. Product Inventory Page
- Table displaying all products
- Columns: Name, Category, Stock, Threshold, Status
- Low stock items highlighted with red badge
- Search/filter by name (optional)

#### 3. Create Order Form
- Dropdown to select product
- Number input for quantity
- On submit:
  - Validate quantity > 0
  - Check if quantity <= stock
  - If insufficient: show error toast
  - If sufficient: create order, reduce stock, show success toast

#### 4. Orders List
- Table displaying all orders
- Columns: Product, Quantity, Status, Date
- Status badge (green for Completed, red for Low Stock)
- Read-only display

#### 5. Replenishment Requests
- Auto-created when stock < threshold
- Table showing all requests
- Columns: Product, Quantity Needed, Status, Actions
- Approve/Reject buttons for Pending requests

#### 6. Approval Simulation
- Approve button:
  - Add stock equal to threshold * 2
  - Update status to Approved
  - Show success toast
- Reject button:
  - Update status to Rejected
  - Show info toast

#### 7. Email Notification Simulation
- Toast notification: "Email sent to Warehouse Manager"
- Triggered when replenishment auto-created

### Initial Dummy Data

8 products:
1. PET Bottles - 150 units, threshold 50
2. HDPE Containers - 30 units, threshold 40 (LOW STOCK)
3. PVC Pipes - 200 units, threshold 30
4. LDPE Film - 45 units, threshold 50 (LOW STOCK)
5. PP Caps - 300 units, threshold 100
6. PS Containers - 25 units, threshold 30 (LOW STOCK)
7. ABS Pellets - 80 units, threshold 25
8. PE Bags - 500 units, threshold 150

---

## Acceptance Criteria

1. App loads without errors
2. Dashboard shows correct stats based on data
3. Products page displays all items with correct low stock highlighting
4. Creating order with insufficient stock shows error
5. Creating valid order reduces stock and adds to orders list
6. Low stock products trigger auto replenishment request
7. Replenishment email toast appears
8. Approve increases stock, updates status
9. Reject updates status to Rejected
10. All pages navigate correctly via sidebar
11. Responsive design works on mobile
12. Toast notifications appear and auto-dismiss