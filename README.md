# Airbnb Clone Project

The backend for the Airbnb Clone project is designed to provide a robust and scalable foundation for managing user interactions, property listings, bookings, and payments. This backend will support various functionalities required to mimic the core features of Airbnb, ensuring a smooth experience for users and hosts.

## Project Goals

1. **User Management**: Implement a secure system for user registration, authentication, and profile management.  
2. **Property Management**: Develop features for property listing creation, updates, and retrieval.  
3. **Booking System**: Create a booking mechanism for users to reserve properties and manage booking details.  
4. **Payment Processing**: Integrate a payment system to handle transactions and record payment details.
5. **Review System**: Allow users to leave reviews and ratings for properties.
6. **Data Optimization**: Ensure efficient data retrieval and storage through database optimizations.  

## Features Overview

### 1. API Documentation

- **OpenAPI Standard**: The backend APIs are documented using the OpenAPI standard to ensure clarity and ease of integration.
- **Django REST Framework**: Provides a comprehensive RESTful API for handling CRUD operations on user and property data.
- **GraphQL**: Offers a flexible and efficient query mechanism for interacting with the backend.  

### 2. User Authentication

- **Endpoints**: /users/, /users/{user_id}/
- **Features**: Register new users, authenticate, and manage user profiles.

### 3. Property Management  

- **Endpoints**: /properties/, /properties/{property_id}/
- **Features**: Create, update, retrieve, and delete property listings.

### 4. Booking System

- **Endpoints**: /bookings/, /bookings/{booking_id}/  
- **Features**: Make, update, and manage bookings, including check-in and check-out details.

### 5. Payment Processing

- **Endpoints**: /payments/
- **Features**: Handle payment transactions related to bookings.  

### 6. Review System

- **Endpoints**: /reviews/, /reviews/{review_id}/
- **Features**: Post and manage reviews for properties.

### 7. Database Optimizations

- **Indexing**: Implement indexes for fast retrieval of frequently accessed data.
- **Caching**: Use caching strategies to reduce database load and improve performance.  

## Technology Stack

- **Django**:  
Serves as the main backend framework, handling the server-side logic, models, views, authentication, and routing to power the core features like user management, property listings, bookings, and payments.

- **Django REST Framework (DRF)**:  
Extends Django’s capabilities by providing powerful tools to easily build, document, and manage RESTful APIs, making data exchange between the frontend and backend seamless.

- **PostgreSQL**:  
Acts as the primary database to securely store and manage structured data for users, properties, bookings, payments, and reviews with high reliability and scalability.

- **GraphQL**:  
Provides a more efficient and flexible way to fetch or manipulate specific data by allowing clients to request exactly what they need from the backend, reducing over-fetching or under-fetching of data.

- **Celery**:  
Manages asynchronous tasks like sending confirmation emails, processing background booking updates, or handling payment workflows without blocking the main application processes.

- **Redis**:  
Used for caching frequently accessed data (like user sessions or popular listings) to improve response times and reduce database load, and also supports Celery as a message broker.

- **Docker**:  
Containerizes the application and its dependencies, ensuring a consistent environment across development, testing, and production, making deployments more reliable and scalable.

- **CI/CD Pipelines**:  
Automates the testing, building, and deployment process, helping the team deliver updates faster, catch bugs earlier, and maintain high code quality with every change.

## Team Roles

- **Backend Developer**:  
Responsible for implementing the core backend logic, API endpoints, and integrations needed to power user management, property listings, bookings, payments, and reviews. They also design the app’s architecture, ensure the scalability of the backend, and implement essential business logic following best practices.

- **Database Administrator**:  
Manages the PostgreSQL database setup, optimization, and security to ensure efficient and reliable data storage for users, properties, bookings, payments, and reviews. Responsibilities include performance tuning, backups, access control, indexing, and disaster recovery planning to keep the system stable and data safe.

- **DevOps Engineer**:
Oversees the deployment, scaling, and monitoring of backend services using Docker and CI/CD pipelines. They unify development and operations by automating code releases, ensuring quick iterations without compromising the stability and reliability of the system.

- **QA Engineer**:  
Evaluates backend functionality, API performance, security, and data integrity through thorough testing processes. They create test plans, protocols, and reports, ensuring that the core features like user management, property management, bookings, payments, and reviews meet quality standards and user expectations.

## Database Design

### Users Table

| Field | Type | Description |
|-----|----|-----------|
| id | UUID/INT | Primary Key |
| email | VARCHAR | Unique email address |
| firstname | VARCHAR | First name of the user |
| lastname | VARCHAR | Last name of the user |
| password | VARCHAR | Hashed password |
| street | VARCHAR | Street address |
| city | VARCHAR | City address |
| state | VARCHAR | State address |
| postal_code | VARCHAR | Postal code |
| country | VARCHAR | Country |
| phone | VARCHAR | Phone number |
| profile_image | VARCHAR | Path or URL to profile image |

### PaymentMethods Table

| Field | Type | Description |
|-----|----|-----------|
| id | UUID/INT | Primary Key |
| user_id | UUID/INT | Foreign Key referencing Users(id) |
| type_of_card | VARCHAR | Type of credit card |
| card_token | VARCHAR | Tokenized card representation |
| name_on_card | VARCHAR | Name on the card |
| expiration_date | DATE | Card expiration date |

### Properties Table

| Field | Type | Description |
|-----|----|-----------|
| id | UUID/INT | Primary Key |
| name | VARCHAR | Property name |
| street | VARCHAR | Street address |
| city | VARCHAR | City address |
| state | VARCHAR | State address |
| postal_code | VARCHAR | Postal code |
| country | VARCHAR | Country |
| price | DECIMAL | Price per night |
| description | TEXT | Property description |
| discount | DECIMAL | Discount percentage |
| latitude | DECIMAL | Latitude for geolocation |
| longitude | DECIMAL | Longitude for geolocation |
| property_type | VARCHAR | Type of property (e.g., apartment, house) |
| bedrooms | INT | Number of bedrooms |
| bathrooms | INT | Number of bathrooms |
| max_guests | INT | Maximum number of guests |
| total_rating | DECIMAL | Average rating of the property |
| owner_id | UUID/INT | Foreign Key referencing Users(id) |

### Photos Table

| Field | Type | Description |
|-----|----|-----------|
| id | UUID/INT | Primary Key |
| property_id | UUID/INT | Foreign Key referencing Properties(id) |
|url | VARCHAR | URL of the photo |

### amenities Table

| Field | Type | Description |
|-----|----|-----------|
| id | UUID/INT | Primary Key |
| name | VARCHAR | Amenity name (e.g., Pool, Gym) |
| icon | VARCHAR | Icon name or URL |

### property_amenities Table

| Field | Type | Description |
|-----|----|-----------|
| property_id | UUID/INT | Foreign Key referencing Properties(id) |
| amenity_id | UUID/INT | Foreign Key referencing Amenities(id) |

### payments Table

| Field | Type | Description |
|-----|----|-----------|
| id | UUID/INT | Primary Key |
| booking_id | UUID/INT | Foreign Key referencing Bookings(id) |
| type | VARCHAR | Type of payment (e.g., credit card, PayPal) |
| amount | DECIMAL | Amount charged |
| status | VARCHAR | Payment status (e.g., pending, completed) |
| payment_method_id | UUID/INT | Foreign Key referencing PaymentMethods(id) |
| transaction_id | UUID/INT | Unique transaction identifier |

### Bookings Table

| Field | Type | Description |
|-----|----|-----------|
| id | UUID/INT | Primary Key |
| user_id | UUID/INT | Foreign Key referencing Users(id) |
| property_id | UUID/INT | Foreign Key referencing Properties(id) |
| date_in | DATE | Check-in date |
| date_out | DATE | Check-out date |
| confirmed | BOOLEAN | Booking confirmation status |
| total_price | DECIMAL | Total price for the booking |
| payment_id | UUID/INT | Foreign Key referencing Payments(id) |

### Reviews Table

| Field | Type | Description |
|-----|----|-----------|
| id | UUID/INT | Primary Key |
| user_id | UUID/INT | Foreign Key referencing Users(id) |
| property_id | UUID/INT | Foreign Key referencing Properties(id) |
| comment | TEXT | Review comment |
| rating | INT | Rating score (e.g., 1 to 5) |

### Relationships

- **Users** can have multiple **PaymentMethods**, **Bookings**, and **Reviews**.
- **Properties** can have multiple **Photos**, **Bookings**, and **Reviews**.
- **amentities** relate to **Properties** via the `property_amenities` join table.

### Design Considerations

- **Indexing**: Fields like `user_id`, `property_id`, and date fields will be indexed for performance.
- **Caching**: caching frequent queries to enhance response times.
- **Availability Logic**: Using the dynamic query logic to determine property availability instead of a static field.
- **Geospatial Queries**: using a geospatial database extension (like PostGIS in PostgreSQL) to handle latitude and longitude efficiently.
- **Security**: Implementing strong hashing for passwords, tokenization for payment methods, and secure API endpoints to protect sensitive data.
- **Data Validation**: Using Django's built-in validators and serializers to ensure data integrity and security.
- **Transaction Management**: Using Django's transaction management to ensure data consistency during complex operations like bookings and payments.

## Feature Breakdown

### User Management

This feature allows users to create new accounts, update personal information, and manage their payment methods securely. Users can also delete their accounts, which facilitates flexible and secure management of user data and preferences.

### Property Management

Property management enables hosts to list new properties, update their information, and manage their visibility through filtering searches. Hosts can add or remove amenities and photos, providing a comprehensive toolset for maintaining attractive and current property listings.

### Booking System

The booking system allows users to easily create new bookings, confirm them, update booking details, or delete bookings. This feature streamlines the reservation process, ensuring users can efficiently manage their stay plans.

### Payment Processing

This feature integrates a secure payment system to handle transactions related to bookings. It streamlines payment operations, ensuring that financial exchanges are processed accurately and securely.

### Review System

Users can leave reviews and delete them as needed, enabling feedback on properties and fostering community trust. This feature helps in maintaining transparency and encouraging better services from hosts.

## API Security

- **JWT Tokens and Cookies**: Used for robust authentication and authorization to manage user sessions securely, ensuring that only verified users have access to the application features.

- **Password Hashing with Bcrypt**: Ensures user passwords are hashed with salt to protect credentials against brute-force attacks, maintaining password security across the platform.

- **Bcrypt for Payment Information**: Encrypts sensitive payment card details to protect user financial data from unauthorized access and potential fraud.

- **Django Permissions and Authentication**: Utilizes Django's built-in permissions and authentication mechanisms to enforce access control, ensuring that users can only access resources they are authorized to view or modify.

- **Input Validation and Sanitization**: All user inputs are validated and sanitized to prevent SQL injection, cross-site scripting (XSS), and other common web vulnerabilities.

- **Secure Cookies and Tokens**: Safeguard API communication by caching JWT tokens and cookies, securing data exchanges and preventing interception during transmission.

- **Rate Limiting**: Implemented to control request frequency, prevent misuse of API resources, and protect against denial-of-service (DoS) attacks, ensuring fair usage across users.

- **HTTPS**: All API interactions occur over HTTPS to encrypt data transmission and secure communication channels.

- **CORS Configuration**: Properly configured Cross-Origin Resource Sharing (CORS) ensures prevention of unauthorized external access to the API.

- **Logging and Monitoring**: Regularly logs API activities, enabling prompt detection and response to suspicious activities and maintaining application integrity.

## CI/CD Pipeline

Continuous Integration (CI) and Continuous Deployment (CD) pipelines are automated processes that streamline the software development lifecycle. CI ensures that code changes are consistently integrated, tested, and validated, making it easier to detect and fix issues early in the development process. CD automates the deployment of applications, ensuring updates are released smoothly and reliably.

### Importance for the Project

- **Consistency**: CI/CD pipelines provide a reliable and consistent approach to building, testing, and deploying code, minimizing risks associated with manual processes.
- **Efficiency**: Automation reduces the time and effort required to release changes, enabling faster delivery of new features and bug fixes.
- **Quality Assurance**: Automated testing during CI ensures that new code integrations do not break existing functionality, maintaining the project's integrity.
- **Confidence in Deployment**: CD facilitates stable and predictable deployments, reducing downtime and errors during updates.

### Suggested tools for CI/CD

- **GitHub Actions**: A versatile tool for setting up CI/CD workflows directly within GitHub repositories, providing integration with a variety of services and tasks.
- **Docker**: Used for containerizing applications to ensure consistent environments across development, testing, and production stages.
- **Jenkins**: A widely-used automation server for building CI/CD pipelines that supports numerous integrations with tools and frameworks.
- **CircleCI**: A cloud-based CI/CD platform that specializes in fast and scalable pipelines, supporting a range of programming languages and tools.
- **Travis CI**: A cloud-based CI service that integrates seamlessly with GitHub, providing automated testing and deployment capabilities.
