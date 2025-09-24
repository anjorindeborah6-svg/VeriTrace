

### VeriTrace - Decentralized Supply Chain Management on Clarity (Stacks)**

---

### 🚀 Overview

This Clarity smart contract enables decentralized and verifiable tracking of products across a supply chain lifecycle. It allows for:

* Role-based access for manufacturers, transporters, and retailers.
* Adding products with immutable creation metadata.
* Updating product locations with historical tracking.
* Revoking or assigning roles securely.
* Reading product data and movement history.

Ideal for building transparent, decentralized, and auditable supply chain solutions on the Stacks blockchain.

---

### 🔐 Role-Based Access

The contract uses strict role checks before allowing access to sensitive functions.

#### Roles Supported:

* **Manufacturer** – Can add products and update locations.
* **Transporter** – Can only update product location.
* **Retailer** – (Currently no write permissions, read-only role)

#### Admin Privileges:

* Only the contract owner (deploying principal) can assign or revoke roles.

---

### 🧱 Data Structures

#### 📍 `roles` map

Stores assigned roles and active status for principals.

```clojure
(define-map roles principal {
  role: (string-utf8 20),
  is-active: bool
})
```

#### 📦 `products` map

Holds the core product data including name, origin, manufacturer, etc.

```clojure
(define-map products uint {
  id: uint,
  name: (string-utf8 100),
  manufacturer: principal,
  origin: (string-utf8 50),
  timestamp: uint,
  current-location: (string-utf8 100),
  status: (string-utf8 20)
})
```

#### 🕒 `product-history` map

Tracks changes to a product’s location and status over time.

```clojure
(define-map product-history {product-id: uint, change-id: uint} {
  timestamp: uint,
  location: (string-utf8 100),
  status: (string-utf8 20)
})
```

---

### 🔧 Public Functions

| Function Name         | Description                                                                                                            |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| `assign-role`         | Assign a role (`manufacturer`, `transporter`, or `retailer`) to a principal. Only the contract owner can perform this. |
| `revoke-role`         | Remove a role from a principal. Only the contract owner can perform this.                                              |
| `add-product`         | Add a new product with name, origin, and initial location. Only `manufacturer`.                                        |
| `update-location`     | Update a product's location and track its movement history. Only `manufacturer` or `transporter`.                      |
| `get-product`         | Read a product's current state.                                                                                        |
| `get-product-history` | Read the first recorded movement in product history. *(Future improvements can expose full history.)*                  |
| `get-role`            | Get role and active status of any principal.                                                                           |

---

### 🧪 Error Codes

| Error Constant       | Code | Description                                    |
| -------------------- | ---- | ---------------------------------------------- |
| `ERR-NOT-AUTHORIZED` | 100  | Caller doesn't have permission.                |
| `ERR-INVALID-ROLE`   | 101  | Provided role string is invalid.               |
| `ERR-NOT-FOUND`      | 102  | Targeted product or role not found.            |
| `ERR-INVALID-INPUT`  | 103  | Provided data is invalid (e.g. string length). |
| `ERR-ALREADY-EXISTS` | 104  | Role or product already exists.                |

---

### 📈 Future Improvements

* Add support for Retailer updates (e.g., mark product as "sold").
* Implement pagination for product history.
* Add event emissions for tracking changes off-chain.

---

### 📜 Deployment

Deploy using the Clarity CLI or a deployment platform like [Clarinet](https://docs.stacks.co/docs/clarity/clarinet/overview/).

Ensure that:

* You initialize the `contract-owner` correctly (set to `tx-sender` at deploy).
* Only the owner can assign roles.

---

## 🧠 Five Rare & Unique Project Name Suggestions

1. **VeriTrace**
2. **Cargoflux**
3. **TransOrigin**
4. **MintSplice**
5. **ProdexChain**

Each of these names blends originality, memorability, and relevance to a traceable product system.

---

## 📦 Pull Request Description

---

### ✨ PR: Initial Smart Contract for Role-Based Product Tracking on Clarity

#### Summary:

This PR introduces a complete Clarity smart contract to manage supply chains through decentralized product tracking. It supports multiple roles and enforces strict role-based permissions for adding and updating product data.

#### Features:

* **Role Assignment**: Contract owner can assign or revoke roles (`manufacturer`, `transporter`, `retailer`).
* **Product Lifecycle**: Products can be added by manufacturers and tracked through location updates.
* **Historical Tracking**: Each location update is logged with a timestamp and status for auditability.
* **Validation**: Strict input validation ensures data integrity.
* **Error Handling**: Rich set of error codes for fail-safe execution.

#### What's Next:

* Full product history retrieval
* Role-based events for off-chain indexing
* Retailer functionality expansion

---
