# airbnb-clone-project
- The Airbnb Clone Project is a comprehensive, real-world application designed to simulate the development of a robust booking platform like Airbnb.
- Airbnb is a web-based platform, where travellers can find and book short-term rentals all over the world. [Airbnb](https://www.airbnb.com/)

## Team Roles
### 1. Business analyst (BA)
- Understands customer’s business processes

- Translates customer business needs into requirements

### 2. Product owner (PO)
- Holds responsibility for a product vision and evolution

- Makes sure the final product meets customer requirements

### 3. Project manager (PM)
- Makes sure a product or its part is delivered on time and within budget

- Manages and motivates the software development team

### 4. Software architect
- Designs a high-level software architecture

- Selects appropriate tools and platforms to implement the product vision

- Sets up code quality standards and performs code reviews

### 5. Backend Developer
- Focuses on server-side logic, APIs, and integration with databases/services.
- Develops and maintains the core application logic.
- Implements security and data protection measures.
- Optimizes performance and scalability.

## ⚙️ Technology Stack
- Django: A high-level Python web framework used for building the RESTful API.
- Django REST Framework: Provides tools for creating and managing RESTful APIs.
- PostgreSQL: A powerful relational database used for data storage.
- GraphQL: Allows for flexible and efficient querying of data.
- Celery: For handling asynchronous tasks such as sending notifications or processing payments.
- Redis: Used for caching and session management.
- Docker: Containerization tool for consistent development and deployment environments.
- CI/CD Pipelines: Automated pipelines for testing and deploying code changes.

## Database Design

The database schema consists of the following entities and relationships:

### 1. **User**
- **Fields**:  
  `id` (UUID/PK), `email` (unique), `password_hash`, `name`, `role` (host/guest)  
- **Relationships**:  
  - One-to-Many with `Property` (A user can list multiple properties).  
  - One-to-Many with `Booking` (A user can make multiple bookings).  
  - One-to-Many with `Review` (A user can write multiple reviews).  

### 2. **Property**
- **Fields**:  
  `id` (UUID/PK), `title`, `description`, `price_per_night`, `location`, `host_id` (FK to User)  
- **Relationships**:  
  - Many-to-One with `User` (Each property belongs to one host).  
  - One-to-Many with `Booking` (A property can have multiple bookings).  
  - One-to-Many with `Review` (A property can receive multiple reviews).  

### 3. **Booking**
- **Fields**:  
  `id` (UUID/PK), `start_date`, `end_date`, `total_price`, `status` (confirmed/pending/cancelled), `guest_id` (FK to User), `property_id` (FK to Property)  
- **Relationships**:  
  - Many-to-One with `User` (A booking is made by one guest).  
  - Many-to-One with `Property` (A booking is for one property).  
  - One-to-One with `Payment` (Each booking has one payment).  

### 4. **Review**
- **Fields**:  
  `id` (UUID/PK), `rating` (1-5), `comment`, `created_at`, `author_id` (FK to User), `property_id` (FK to Property)  
- **Relationships**:  
  - Many-to-One with `User` (A review is written by one user).  
  - Many-to-One with `Property` (A review is about one property).  

### 5. **Payment**
- **Fields**:  
  `id` (UUID/PK), `amount`, `payment_method`, `status` (success/failed), `transaction_id`, `booking_id` (FK to Booking)  
- **Relationships**:  
  - One-to-One with `Booking` (A payment corresponds to one booking).  

---

### Schema Diagram (Conceptual)
```plaintext
User ────┐
  │      │
  ├─ Property ──── Booking ─── Payment
  │        │
  └── Review
```
## Feature Breakdown

### 1. **User Management**
- Allows users to register, log in, and manage profiles (hosts/guests).  
- Implements JWT authentication for secure access and role-based permissions (e.g., hosts can list properties).  

### 2. **Property Management**
- Enables hosts to create, update, and delete property listings with details (photos, pricing, amenities).  
- Includes search/filter functionality (e.g., by location, price range) for guests.  

### 3. **Booking System**
- Handles reservation workflows: availability checks, date selection, and payment integration.  
- Sends confirmation emails and updates booking status (confirmed/pending/cancelled).  

### 4. **Reviews & Ratings**
- Lets guests leave ratings and reviews for properties post-stay.  
- Displays aggregate scores on property listings to build trust.  

### 5. **Payment Processing**
- Integrates with Stripe/PayPal for secure transactions.  
- Calculates totals (including fees) and refunds cancellations per policy.  

### 6. **Messaging System**
- Facilitates communication between guests and hosts via real-time chat.  
- Notifies users of new messages via email or in-app alerts.  

### 7. **Admin Dashboard**
- Provides moderators with tools to manage users, properties, and resolve disputes.  
- Generates reports (e.g., revenue, popular locations) for business insights.  

---

### Upcoming Features (Optional)
- **Wishlists**: Save favorite properties for future bookings.  
- **Smart Pricing**: AI-driven price suggestions for hosts.  

## API Security

To protect sensitive data and ensure system integrity, the following security measures will be implemented:

### 1. **Authentication**
- **JWT (JSON Web Tokens)** for stateless user sessions.  
- **Password hashing** (bcrypt) to securely store credentials.  
- **Why?** Prevents unauthorized access to user accounts and personal data (e.g., payment details, bookings).

### 2. **Authorization**
- **Role-based access control (RBAC)** to restrict actions (e.g., only hosts can edit properties).  
- **Resource ownership checks** (e.g., users can only delete their own reviews).  
- **Why?** Ensures users only interact with data they’re permitted to, minimizing misuse.

### 3. **Rate Limiting**
- **Throttling** (e.g., 100 requests/minute per IP) using Django Ratelimit.  
- **Why?** Prevents brute-force attacks and API abuse (e.g., spamming bookings/reviews).

### 4. **Data Protection**
- **HTTPS/TLS** for encrypted data in transit.  
- **SQL injection prevention** via Django ORM parameterized queries.  
- **Why?** Safeguards sensitive information (e.g., credit card numbers, user emails).

### 5. **Payment Security**
- **PCI-DSS compliance** by using Stripe/PayPal (no direct card storage).  
- **Webhook validation** to verify payment events.  
- **Why?** Reduces fraud risk and ensures financial transactions are irreversible.

### 6. **CORS & CSRF Protection**
- **Strict CORS policies** to limit API access to trusted domains.  
- **CSRF tokens** for state-changing operations (e.g., form submissions).  
- **Why?** Blocks malicious cross-origin requests (e.g., phishing attacks).

### 7. **Monitoring & Logging**
- **Audit logs** for sensitive actions (e.g., login attempts, payment changes).  
- **Why?** Enables breach detection and forensic analysis.

---

### Security Tools Used
- **Django-allauth**: OAuth2 integration for social logins.  
- **django-cors-headers**: Manages CORS policies.  
- **Helmet.js** (if using Node.js middleware): Sets secure HTTP headers.


## CI/CD Pipeline

### What is CI/CD?
**Continuous Integration (CI)** and **Continuous Deployment (CD)** automate the process of testing, building, and deploying code changes. This ensures faster, more reliable software delivery.

### Why It Matters for This Project
- **Quality Control**: Automated tests catch bugs early before they reach production.  
- **Rapid Iteration**: Enables frequent, small updates instead of risky bulk releases.  
- **Consistency**: Eliminates "it works on my machine" issues via containerized environments.  

### Tools & Workflow
1. **GitHub Actions**:  
   - Runs automated tests (unit/integration) on every `git push`.  
   - Builds Docker images and deploys to staging on merge to `main`.  

2. **Docker**:  
   - Containerizes the app for identical environments (dev → prod).  
   - Simplifies scaling and dependency management.  

3. **AWS/GCP (Optional)**:  
   - Auto-deploys to cloud servers after successful CI checks.  

4. **Monitoring (Post-Deployment)**:  
   - Tools like Sentry or New Relic track errors in production.  

---

### Example Pipeline Stages
```yaml
1. Code Push → 2. Run Tests → 3. Build Docker Image → 4. Deploy to Staging → 5. Manual Approval → 6. Production Rollout
```
