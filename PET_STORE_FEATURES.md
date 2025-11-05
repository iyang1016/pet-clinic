# 🛍️ Pet Store / E-Commerce Module

## Overview
Integrated online pet store for selling products directly to clients, with in-clinic and online purchase options.

## 🎯 Business Benefits

- **Additional Revenue Stream**: 20-30% increase in clinic revenue
- **Convenience for Clients**: One-stop shop for pet needs
- **Prescription Products**: Controlled selling of prescription food/medicine
- **Inventory Integration**: Unified stock management
- **Client Retention**: Keep clients coming back for supplies

---

## 🛒 Core Store Features

### 1. Product Catalog Management
**Priority:** 🟡 High | **Complexity:** Medium | **Time:** 3-4 days

#### Features:
- [ ] Product categories (Food, Accessories, Medicine, Grooming, Toys)
- [ ] Product variants (sizes, flavors, colors)
- [ ] Rich product descriptions
- [ ] Multiple product images
- [ ] SKU/Barcode management
- [ ] Prescription-required flag
- [ ] Featured products
- [ ] Product search & filters
- [ ] Stock availability display
- [ ] Related products
- [ ] Product reviews/ratings

#### Data Structure:
```javascript
{
  id: uuid,
  name: string,
  category: 'food' | 'medicine' | 'accessories' | 'grooming' | 'toys' | 'supplies',
  subcategory: string,
  brand: string,
  description: text,
  shortDescription: string,
  sku: string,
  barcode: string,
  images: string[],
  price: number,
  comparePrice: number, // for discounts
  cost: number, // purchase price
  weight: number,
  unit: string,
  variants: [{
    name: string, // "Size", "Flavor"
    options: string[], // ["Small", "Medium", "Large"]
    price: number,
    sku: string,
    stock: number
  }],
  stock: number,
  lowStockThreshold: number,
  requiresPrescription: boolean,
  isActive: boolean,
  tags: string[],
  petTypes: string[], // ['dog', 'cat', 'bird']
  featured: boolean,
  bestSeller: boolean
}
```

---

### 2. Shopping Cart System
**Priority:** 🟡 High | **Complexity:** Medium | **Time:** 2-3 days

#### Features:
- [ ] Add to cart (with quantity)
- [ ] Update quantities
- [ ] Remove items
- [ ] Save cart for later
- [ ] Cart persistence (local storage)
- [ ] Quick add from previous orders
- [ ] Cart summary widget
- [ ] Apply discounts/coupons
- [ ] Prescription validation
- [ ] Stock availability check
- [ ] Cart abandonment recovery

#### Cart Flow:
```
Browse Products → Add to Cart → Review Cart → Checkout → Payment → Order Confirmation
```

#### Cart Data:
```javascript
{
  id: uuid,
  clientId: uuid,
  items: [{
    productId: uuid,
    variantId: uuid,
    name: string,
    image: string,
    price: number,
    quantity: number,
    subtotal: number,
    requiresPrescription: boolean
  }],
  subtotal: number,
  discount: number,
  tax: number,
  shipping: number,
  total: number,
  couponCode: string,
  notes: text
}
```

---

### 3. Checkout Process
**Priority:** 🟡 High | **Complexity:** Medium | **Time:** 2-3 days

#### Features:
- [ ] Guest checkout option
- [ ] Customer information form
- [ ] Delivery vs Pickup selection
- [ ] Delivery address management
- [ ] Delivery scheduling
- [ ] Payment method selection
- [ ] Order summary review
- [ ] Terms acceptance
- [ ] Order confirmation email
- [ ] Invoice generation

#### Checkout Steps:
1. **Cart Review** - Final check of items
2. **Customer Info** - Contact details
3. **Delivery Method** - Pickup or delivery
4. **Payment** - Select payment method
5. **Confirmation** - Order placed

---

### 4. Order Management
**Priority:** 🟡 High | **Complexity:** Medium | **Time:** 3 days

#### Features:
- [ ] Order listing (admin & client)
- [ ] Order status tracking
- [ ] Order details view
- [ ] Order status updates
- [ ] Order cancellation
- [ ] Refunds/Returns
- [ ] Order history
- [ ] Reorder functionality
- [ ] Order notifications
- [ ] Packing slips
- [ ] Shipping labels

#### Order Status Flow:
```
Pending → Confirmed → Processing → Ready/Shipped → Delivered/Picked-up → Completed
         ↓
     Cancelled
```

#### Order Data:
```javascript
{
  id: uuid,
  orderNumber: string,
  clientId: uuid,
  items: array,
  status: 'pending' | 'confirmed' | 'processing' | 'ready' | 'shipped' | 'delivered' | 'cancelled',
  paymentStatus: 'pending' | 'paid' | 'refunded',
  paymentMethod: string,
  deliveryMethod: 'pickup' | 'delivery',
  deliveryAddress: object,
  scheduledDate: date,
  subtotal: number,
  discount: number,
  tax: number,
  shipping: number,
  total: number,
  notes: text,
  trackingNumber: string,
  createdAt: timestamp,
  updatedAt: timestamp
}
```

---

### 5. Payment Integration
**Priority:** 🟡 High | **Complexity:** High | **Time:** 3-4 days

#### Payment Methods:
- [ ] Cash on Delivery/Pickup
- [ ] Credit/Debit Card (via Stripe/PayPal)
- [ ] GCash (Philippines)
- [ ] Bank Transfer
- [ ] PayMaya
- [ ] Clinic Credit/Account
- [ ] Installment plans

#### Features:
- [ ] Secure payment processing
- [ ] Payment confirmation
- [ ] Receipt generation
- [ ] Refund processing
- [ ] Payment history
- [ ] Failed payment handling

---

### 6. Promotions & Discounts
**Priority:** 🟢 Medium | **Complexity:** Medium | **Time:** 2 days

#### Features:
- [ ] Discount codes/Coupons
- [ ] Percentage discounts
- [ ] Fixed amount discounts
- [ ] Buy X Get Y promotions
- [ ] Bundle deals
- [ ] Loyalty points
- [ ] Member pricing
- [ ] Flash sales
- [ ] Free shipping threshold
- [ ] First-time buyer discount

#### Promotion Rules:
```javascript
{
  code: string,
  type: 'percentage' | 'fixed' | 'bogo' | 'bundle',
  value: number,
  minimumPurchase: number,
  validFrom: date,
  validUntil: date,
  usageLimit: number,
  usedCount: number,
  applicableProducts: uuid[],
  applicableCategories: string[],
  customerGroups: string[]
}
```

---

### 7. Inventory Integration
**Priority:** 🟡 High | **Complexity:** High | **Time:** 3 days

#### Features:
- [ ] Real-time stock updates
- [ ] Reserved stock for orders
- [ ] Low stock notifications
- [ ] Auto-disable out of stock
- [ ] Stock synchronization
- [ ] Batch tracking
- [ ] Expiry date management
- [ ] Supplier management
- [ ] Purchase orders
- [ ] Stock adjustments

---

### 8. Client Portal Store
**Priority:** 🟢 Medium | **Complexity:** Medium | **Time:** 3 days

#### Customer Features:
- [ ] Browse products
- [ ] Search & filter
- [ ] View order history
- [ ] Track orders
- [ ] Reorder items
- [ ] Wishlist/Favorites
- [ ] Product reviews
- [ ] Prescription upload
- [ ] Auto-refill subscriptions
- [ ] Pet-specific recommendations

---

## 📱 UI Components for Store

### Product Display
```jsx
// ProductCard.jsx
<div className="card card-compact bg-base-100 shadow-xl">
  <figure>
    <img src={product.image} alt={product.name} />
    {product.bestSeller && <div className="badge badge-secondary absolute top-2 right-2">Best Seller</div>}
  </figure>
  <div className="card-body">
    <h2 className="card-title">
      {product.name}
      {product.requiresPrescription && <div className="badge badge-warning">Rx</div>}
    </h2>
    <p className="text-sm opacity-70">{product.brand}</p>
    <div className="flex items-center justify-between">
      <span className="text-2xl font-bold">₱{product.price}</span>
      {product.comparePrice && (
        <span className="text-sm line-through opacity-50">₱{product.comparePrice}</span>
      )}
    </div>
    <div className="card-actions justify-end">
      <button className="btn btn-primary btn-sm">Add to Cart</button>
    </div>
  </div>
</div>
```

### Shopping Cart
```jsx
// ShoppingCart.jsx
<div className="drawer drawer-end">
  <div className="drawer-content">
    {/* Page content */}
  </div>
  <div className="drawer-side">
    <div className="w-96 min-h-full bg-base-100">
      <div className="p-4">
        <h2 className="text-xl font-bold mb-4">Shopping Cart</h2>
        
        {/* Cart Items */}
        {cartItems.map(item => (
          <div className="flex gap-4 mb-4 p-2 border rounded">
            <img src={item.image} className="w-20 h-20 object-cover rounded" />
            <div className="flex-1">
              <h3 className="font-semibold">{item.name}</h3>
              <p className="text-sm opacity-70">{item.variant}</p>
              <div className="flex items-center gap-2 mt-2">
                <div className="join">
                  <button className="join-item btn btn-xs">-</button>
                  <input className="join-item input input-xs w-12 text-center" value={item.quantity} />
                  <button className="join-item btn btn-xs">+</button>
                </div>
                <span className="font-bold">₱{item.subtotal}</span>
              </div>
            </div>
            <button className="btn btn-ghost btn-xs">
              <X className="w-4 h-4" />
            </button>
          </div>
        ))}
        
        {/* Cart Summary */}
        <div className="divider"></div>
        <div className="space-y-2">
          <div className="flex justify-between">
            <span>Subtotal</span>
            <span>₱{cart.subtotal}</span>
          </div>
          <div className="flex justify-between">
            <span>Delivery</span>
            <span>₱{cart.shipping}</span>
          </div>
          <div className="flex justify-between font-bold text-lg">
            <span>Total</span>
            <span>₱{cart.total}</span>
          </div>
        </div>
        
        <button className="btn btn-primary w-full mt-4">Proceed to Checkout</button>
      </div>
    </div>
  </div>
</div>
```

---

## 💵 Pricing Strategy

### Product Pricing Rules
- **Markup Formula**: Cost × 1.3-2.0 (30-100% markup)
- **Member Discount**: 5-10% off regular price
- **Bundle Pricing**: 15% off when buying sets
- **Prescription Products**: Fixed pricing (regulated)
- **Promotional Pricing**: Time-limited discounts

### Delivery Fees
- **Standard Delivery**: ₱50-100 (based on distance)
- **Express Delivery**: ₱150-200
- **Free Delivery**: Orders over ₱1,500
- **In-clinic Pickup**: Free

---

## 📊 Store Analytics

### Key Metrics
- **Sales Revenue**: Daily, weekly, monthly
- **Average Order Value**: Track trends
- **Best Selling Products**: Top 10 items
- **Cart Abandonment Rate**: Recovery strategies
- **Customer Lifetime Value**: Repeat purchases
- **Product Performance**: Views vs purchases
- **Category Performance**: Which categories sell most
- **Inventory Turnover**: Stock efficiency

### Reports
- Daily sales report
- Product performance report
- Customer purchase history
- Low stock alerts
- Expired products list
- Profit margin analysis

---

## 🔄 Integration with Clinic System

### Unified Features
1. **Single Customer Database**: Same clients for clinic and store
2. **Prescription Validation**: Auto-check for Rx products
3. **Appointment Booking**: Add products during appointment
4. **Billing Integration**: Combined invoice for services + products
5. **Loyalty Program**: Points for both services and products
6. **Inventory Sharing**: Same stock for clinic use and retail

### Cross-selling Opportunities
- Suggest products after appointments
- Bundle services with products
- Prescription food with medical treatment
- Grooming products with grooming service
- Preventive care packages

---

## 📱 Mobile Considerations

### Mobile Store Features
- Mobile-optimized product browsing
- One-thumb checkout
- Barcode scanning for quick add
- Mobile payment options
- Order tracking notifications
- Click to call/chat support

---

## 🚀 Implementation Phases

### Phase 1: Basic Store (MVP+)
- Product catalog
- Simple cart
- In-clinic purchase only
- Cash payment

### Phase 2: Online Store
- Full e-commerce
- Online payments
- Delivery options
- Customer accounts

### Phase 3: Advanced Features
- Subscriptions
- Loyalty program
- Advanced promotions
- Mobile app

---

## 🎯 Success Metrics

### Store KPIs
- **Conversion Rate**: > 3%
- **Cart Abandonment**: < 70%
- **Average Order Value**: ₱500-1,000
- **Repeat Purchase Rate**: > 20%
- **Product Page Load**: < 2 seconds
- **Checkout Completion**: > 60%

---

## 🔧 Technical Requirements

### Database Tables (Additional)
```sql
-- Products table
products
  - id
  - name
  - category
  - brand
  - price
  - stock
  - images (JSON)
  - requiresPrescription

-- Orders table  
orders
  - id
  - clientId
  - orderNumber
  - status
  - total
  - deliveryMethod
  - paymentStatus

-- Order Items table
order_items
  - id
  - orderId
  - productId
  - quantity
  - price
  - subtotal

-- Cart table (optional, can use localStorage)
carts
  - id
  - clientId
  - items (JSON)
  - expiresAt
```

### API Endpoints
```
GET    /api/products
GET    /api/products/:id
GET    /api/products/search
POST   /api/cart/add
PUT    /api/cart/update
DELETE /api/cart/remove
POST   /api/checkout
GET    /api/orders
GET    /api/orders/:id
PUT    /api/orders/:id/status
```