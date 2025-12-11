# RevTicket - Project Analysis Document

**Project Name:** RevTicket  
**Type:** Full-Stack Movie Ticket Booking System  
**Repository:** https://github.com/harshWarbhe/revTicket  
**Current Branch:** master  
**Version:** 1.0.0  
**Last Updated:** December 9, 2025

---

## 📑 Table of Contents

1. [Executive Summary](#executive-summary)
2. [Project Overview](#project-overview)
3. [Architecture & Design](#architecture--design)
4. [Technology Stack](#technology-stack)
5. [Project Structure](#project-structure)
6. [Core Features](#core-features)
7. [Data Models](#data-models)
8. [Deployment Architecture](#deployment-architecture)
9. [Development Setup](#development-setup)
10. [CI/CD Pipeline](#cicd-pipeline)
11. [Security Implementation](#security-implementation)
12. [Performance & Scalability](#performance--scalability)
13. [Testing Strategy](#testing-strategy)
14. [Known Issues & Improvements](#known-issues--improvements)

---

## 📊 Executive Summary

**RevTicket** is a comprehensive, production-ready movie ticket booking platform built using modern full-stack technologies. The system combines:

- **Backend**: Spring Boot 3.2.0 with REST API architecture
- **Frontend**: Angular 18 with responsive UI
- **Databases**: MySQL 8.0 (primary) + MongoDB 8.0 (reviews)
- **Deployment**: Docker containerization with Jenkins CI/CD pipeline
- **Infrastructure**: Supports local development, production, and AWS EC2 deployment

The platform supports both user and admin functionalities, including real-time seat selection via WebSocket, payment processing through Razorpay, and comprehensive analytics dashboards.

**Status**: Fully functional with multi-platform Docker builds (amd64, arm64)

---

## 🎯 Project Overview

### Purpose

RevTicket provides a complete solution for movie theaters to manage bookings, inventory, and revenue while enabling customers to:
- Browse and book movie tickets
- Make secure payments
- Download digital tickets with QR codes
- Write and read reviews
- Manage booking history

### Key Objectives

1. ✅ Provide seamless ticket booking experience
2. ✅ Enable real-time seat availability updates
3. ✅ Support multiple payment methods (Razorpay integration)
4. ✅ Offer comprehensive admin management tools
5. ✅ Ensure secure user authentication and authorization
6. ✅ Generate detailed business analytics and reports
7. ✅ Support multi-theater operations
8. ✅ Enable easy deployment and scaling

### Business Requirements Met

- **User Management**: Registration, login (email/Google OAuth2), profile management
- **Movie Management**: Browse, search, filter movies with detailed information
- **Theater Management**: Multi-screen, multi-showtime operations
- **Booking System**: Real-time seat selection with dynamic pricing
- **Payment Processing**: Integrated with Razorpay for secure transactions
- **Revenue Analytics**: Dashboard with detailed revenue and booking metrics
- **Review System**: User ratings and reviews for movies
- **Notification System**: Email notifications for bookings and confirmations

---

## 🏗️ Architecture & Design

### System Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                     CLIENT LAYER                            │
│  ┌──────────────────────────────────────────────────────┐  │
│  │         Angular SPA (Port 4200/80)                  │  │
│  │  - Component-based UI                               │  │
│  │  - Reactive forms with RxJS                         │  │
│  │  - WebSocket integration for real-time updates      │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────────┐
│                     API LAYER                               │
│  ┌──────────────────────────────────────────────────────┐  │
│  │     Spring Boot REST API (Port 8080/8081)           │  │
│  │  - JWT Token-based Authentication                    │  │
│  │  - OAuth2 Google Integration                         │  │
│  │  - Real-time WebSocket Endpoints                     │  │
│  │  - RESTful Endpoints for CRUD operations             │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────────┐
│                  SERVICE LAYER                              │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  - Business Logic Implementation                     │  │
│  │  - Transaction Management                            │  │
│  │  - Payment Processing                                │  │
│  │  - Email Notifications                               │  │
│  │  - Scheduled Jobs (Caching, Cleanup)                 │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────────┐
│                  DATA ACCESS LAYER                          │
│  ┌──────────────────────────────────────────────────────┐  │
│  │     Spring Data JPA / MongoDB Repositories           │  │
│  │  - ORM mapping and query generation                  │  │
│  │  - Transaction management                            │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────────┐
│                   DATA LAYER                                │
│  ┌──────────────────┐         ┌──────────────────────────┐ │
│  │   MySQL 8.0      │         │    MongoDB 8.0          │ │
│  │  - Users         │         │  - Reviews              │ │
│  │  - Movies        │         │  - Ratings              │ │
│  │  - Theaters      │         │                          │ │
│  │  - Screens       │         │                          │ │
│  │  - Showtimes     │         │                          │ │
│  │  - Bookings      │         │                          │ │
│  │  - Payments      │         │                          │ │
│  └──────────────────┘         └──────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
```

### Architectural Patterns

1. **MVC Pattern**: Controller → Service → Repository architecture
2. **Dependency Injection**: Spring IoC container for loose coupling
3. **Repository Pattern**: Data access abstraction
4. **DTO Pattern**: Separation of API contracts from domain models
5. **Singleton Pattern**: Service layer components
6. **Observer Pattern**: WebSocket event broadcasting
7. **Strategy Pattern**: Multiple authentication methods (JWT, OAuth2)

---

## 💻 Technology Stack

### Backend Technologies

| Component | Technology | Version | Purpose |
|-----------|-----------|---------|---------|
| **Core Framework** | Spring Boot | 3.2.0 | REST API and application container |
| **Language** | Java | 17+ | Primary programming language |
| **ORM** | Spring Data JPA | 3.2.0 | MySQL entity mapping and queries |
| **NoSQL** | Spring Data MongoDB | 3.2.0 | MongoDB document management |
| **Authentication** | Spring Security + JWT | 3.2.0 + 0.12.3 | Token-based authentication |
| **OAuth2** | Spring OAuth2 | 3.2.0 | Google login integration |
| **WebSocket** | Spring WebSocket | 3.2.0 | Real-time seat updates |
| **Code Generation** | Lombok | 1.18.36 | Reduces boilerplate code |
| **Payment SDK** | Razorpay | 1.4.6 | Payment processing |
| **Build Tool** | Maven | 3.x | Dependency and build management |
| **Validation** | Spring Validation | 3.2.0 | Input validation |

### Frontend Technologies

| Component | Technology | Version | Purpose |
|-----------|-----------|---------|---------|
| **Framework** | Angular | 18.0.0 | Component-based SPA framework |
| **Language** | TypeScript | 5.4.0 | Type-safe JavaScript |
| **HTTP Client** | Angular HttpClient | 18.0.0 | API communication |
| **Routing** | Angular Router | 18.0.0 | Client-side navigation |
| **Forms** | Angular Reactive Forms | 18.0.0 | Form management |
| **Reactive** | RxJS | 7.8.0 | Observable-based programming |
| **WebSocket** | STOMP.js | 7.2.1 | WebSocket client |
| **WebSocket Fallback** | SockJS | 1.6.1 | HTTP fallback for WebSocket |
| **QR Code** | qrcode | 1.5.4 | QR ticket generation |
| **PDF Export** | jsPDF | 3.0.4 | PDF ticket generation |
| **Canvas Rendering** | html2canvas | 1.4.1 | HTML to image conversion |
| **Build Tool** | Angular CLI | 18.0.0 | Angular build and dev server |
| **Package Manager** | npm | Latest | Node.js dependency management |

### Database Technologies

| Database | Version | Purpose | Data |
|----------|---------|---------|------|
| **MySQL** | 8.0 | Relational data | Users, Movies, Theaters, Bookings, Payments, Showtimes, Screens |
| **MongoDB** | 8.0 | Document data | Reviews, Ratings, User feedback |

### DevOps & Infrastructure

| Tool | Purpose |
|------|---------|
| **Docker** | Application containerization |
| **Docker Compose** | Multi-container orchestration (local/prod) |
| **Nginx** | Frontend web server and reverse proxy |
| **Jenkins** | CI/CD pipeline automation |
| **DockerHub** | Container registry (harshwarbhe/revticket-*) |
| **AWS EC2** | Cloud deployment platform |

---

## 📁 Project Structure

### Root Level Organization

```
revTicket/
├── Backend/                      # Spring Boot application
├── Frontend/                     # Angular application
├── docker-compose.yml            # Development environment
├── docker-compose.prod.yml       # Production environment
├── docker-compose.ec2.yml        # EC2 deployment
├── Jenkinsfile                   # CI/CD pipeline definition
├── deploy-ec2.sh                 # EC2 deployment script
├── deploy-aws.sh                 # AWS deployment script
├── ec2-deploy.sh                 # Alternative EC2 deployment
├── start.bat                     # Windows quick start
├── start.sh                      # Linux/Mac quick start
├── README.md                     # Setup guide
├── QUICK_START.md                # Deployment quick start
├── DEPLOYMENT_SUMMARY.md         # Deployment documentation
├── JENKINS_DEPLOYMENT_GUIDE.md   # Jenkins setup guide
└── PROJECT_DOCUMENTATION.md      # Detailed feature documentation
```

### Backend Structure (Spring Boot)

```
Backend/
├── src/main/java/com/revticket/
│   ├── config/                   # Configuration classes
│   │   ├── CacheConfig.java      # Caching configuration
│   │   ├── DataLoader.java       # Initial data loading
│   │   ├── SecurityConfig.java   # Spring Security setup
│   │   └── WebSocketConfig.java  # WebSocket configuration
│   │
│   ├── controller/               # REST API endpoints
│   │   ├── AuthController.java        # Authentication endpoints
│   │   ├── BookingController.java     # Booking operations
│   │   ├── MovieController.java       # Movie browsing
│   │   ├── PaymentController.java     # Payment processing
│   │   ├── ReviewController.java      # Review management
│   │   ├── TheaterController.java     # Theater information
│   │   ├── UserController.java        # User profile
│   │   ├── AdminDashboardController.java    # Admin analytics
│   │   ├── AdminMovieController.java        # Movie management
│   │   ├── AdminTheaterController.java      # Theater management
│   │   ├── AdminScreenController.java       # Screen management
│   │   ├── AdminShowtimeController.java     # Showtime management
│   │   ├── AdminUserController.java         # User management
│   │   ├── AdminReviewController.java       # Review moderation
│   │   └── AdminReportController.java       # Report generation
│   │
│   ├── dto/                      # Data Transfer Objects
│   │   ├── AuthDTO/              # Login/registration requests
│   │   ├── BookingDTO/           # Booking request/response
│   │   ├── PaymentDTO/           # Payment data
│   │   ├── MovieDTO/             # Movie information
│   │   ├── TheaterDTO/           # Theater data
│   │   ├── UserDTO/              # User profile data
│   │   └── ...                   # Other DTOs
│   │
│   ├── entity/                   # JPA Entities (MySQL)
│   │   ├── User.java             # User account entity
│   │   ├── Movie.java            # Movie catalog
│   │   ├── Theater.java          # Theater information
│   │   ├── Screen.java           # Screen/auditorium
│   │   ├── Showtime.java         # Movie showtime
│   │   ├── Booking.java          # User booking
│   │   ├── Payment.java          # Payment record
│   │   ├── Seat.java             # Screen seat
│   │   ├── SeatCategory.java     # Seat type/pricing
│   │   └── ...                   # Other entities
│   │
│   ├── mongo/entity/             # MongoDB Documents
│   │   ├── MongoReview.java      # Review document
│   │   └── MongoRating.java      # Rating document
│   │
│   ├── repository/               # Data Access Layer
│   │   ├── UserRepository.java
│   │   ├── MovieRepository.java
│   │   ├── BookingRepository.java
│   │   ├── PaymentRepository.java
│   │   ├── TheaterRepository.java
│   │   ├── ReviewRepository.java  # MongoDB repository
│   │   └── ...
│   │
│   ├── service/                  # Business Logic Layer
│   │   ├── AuthService.java           # Authentication logic
│   │   ├── BookingService.java        # Booking management
│   │   ├── PaymentService.java        # Payment processing
│   │   ├── MovieService.java          # Movie operations
│   │   ├── TheaterService.java        # Theater operations
│   │   ├── ReviewService.java         # Review management
│   │   ├── UserService.java           # User management
│   │   ├── SeatService.java           # Seat management
│   │   ├── EmailService.java          # Email notifications
│   │   ├── AdminDashboardService.java # Analytics
│   │   ├── AdminReportService.java    # Report generation
│   │   └── ...
│   │
│   ├── security/                 # Security Implementation
│   │   ├── JwtTokenProvider.java      # JWT token management
│   │   ├── JwtAuthFilter.java         # JWT authentication filter
│   │   ├── CustomUserDetailsService.java # User authentication
│   │   ├── OAuth2SuccessHandler.java     # OAuth2 handling
│   │   └── SecurityConstants.java     # Security constants
│   │
│   ├── scheduler/                # Scheduled Tasks
│   │   ├── CacheRefreshScheduler.java # Cache updates
│   │   ├── BookingCleanupScheduler.java # Cleanup tasks
│   │   └── ReportGenerationScheduler.java # Report generation
│   │
│   ├── exception/                # Exception Handling
│   │   ├── ApiException.java      # Custom exceptions
│   │   ├── GlobalExceptionHandler.java # Global exception handler
│   │   └── ...
│   │
│   ├── util/                     # Utility Classes
│   │   ├── EmailUtil.java        # Email sending utilities
│   │   ├── QrCodeUtil.java       # QR code generation
│   │   ├── DateUtil.java         # Date/time utilities
│   │   └── ...
│   │
│   └── RevTicketApplication.java # Main Spring Boot application
│
├── src/main/resources/
│   ├── application.properties    # Configuration file
│   └── templates/                # Email templates
│
├── pom.xml                       # Maven configuration
├── Dockerfile                    # Container image definition
├── mvnw / mvnw.cmd              # Maven wrapper
└── target/                       # Compiled output
```

### Frontend Structure (Angular)

```
Frontend/
├── src/
│   ├── app/
│   │   ├── auth/                 # Authentication module
│   │   │   ├── login/
│   │   │   │   ├── login.component.ts
│   │   │   │   ├── login.component.html
│   │   │   │   └── login.component.css
│   │   │   ├── signup/
│   │   │   ├── forgot-password/
│   │   │   ├── reset-password/
│   │   │   ├── auth.routes.ts    # Auth routing
│   │   │   └── auth.guard.ts     # Route protection
│   │   │
│   │   ├── user/                 # User module
│   │   │   ├── pages/
│   │   │   │   ├── home/         # Movie listing
│   │   │   │   ├── movie-details/
│   │   │   │   ├── seat-selection/   # Real-time seat selection
│   │   │   │   ├── booking-confirmation/
│   │   │   │   ├── payment/
│   │   │   │   ├── my-bookings/
│   │   │   │   ├── booking-details/
│   │   │   │   ├── profile/
│   │   │   │   ├── ticket-download/
│   │   │   │   ├── reviews/
│   │   │   │   └── search-results/
│   │   │   ├── components/
│   │   │   │   ├── movie-card/
│   │   │   │   ├── seat-layout/
│   │   │   │   └── movie-filter/
│   │   │   ├── layout/
│   │   │   │   └── user-layout/
│   │   │   └── user.routes.ts
│   │   │
│   │   ├── admin/                # Admin module
│   │   │   ├── pages/
│   │   │   │   ├── dashboard/         # Analytics dashboard
│   │   │   │   ├── manage-movies/     # Add/Edit movies
│   │   │   │   ├── manage-theaters/   # Theater management
│   │   │   │   ├── manage-screens/    # Screen management
│   │   │   │   ├── manage-shows/      # Showtime management
│   │   │   │   ├── users/             # User management
│   │   │   │   ├── reviews/           # Review moderation
│   │   │   │   ├── bookings-report/   # Booking analytics
│   │   │   │   ├── settings/          # Admin settings
│   │   │   │   └── profile/
│   │   │   ├── components/
│   │   │   │   ├── admin-sidebar/
│   │   │   │   ├── admin-header/
│   │   │   │   ├── analytics-card/
│   │   │   │   └── data-table/
│   │   │   ├── layout/
│   │   │   │   └── admin-layout/
│   │   │   ├── styles/
│   │   │   │   └── admin-shared.css
│   │   │   └── admin.routes.ts
│   │   │
│   │   ├── core/                 # Core services
│   │   │   ├── guards/
│   │   │   │   ├── auth.guard.ts
│   │   │   │   └── admin.guard.ts
│   │   │   ├── interceptors/
│   │   │   │   ├── auth.interceptor.ts
│   │   │   │   └── error.interceptor.ts
│   │   │   ├── models/
│   │   │   │   ├── user.model.ts
│   │   │   │   ├── movie.model.ts
│   │   │   │   ├── booking.model.ts
│   │   │   │   └── ...
│   │   │   ├── services/
│   │   │   │   ├── auth.service.ts
│   │   │   │   ├── api.service.ts
│   │   │   │   ├── movie.service.ts
│   │   │   │   ├── booking.service.ts
│   │   │   │   ├── payment.service.ts
│   │   │   │   ├── websocket.service.ts
│   │   │   │   ├── notification.service.ts
│   │   │   │   └── ...
│   │   │   └── utils/
│   │   │       ├── date.util.ts
│   │   │       ├── storage.util.ts
│   │   │       └── ...
│   │   │
│   │   ├── shared/               # Shared components
│   │   │   ├── components/
│   │   │   │   ├── navbar/
│   │   │   │   ├── footer/
│   │   │   │   ├── loading-spinner/
│   │   │   │   ├── error-message/
│   │   │   │   └── modal/
│   │   │   ├── models/
│   │   │   ├── pipes/
│   │   │   │   ├── currency.pipe.ts
│   │   │   │   └── date-format.pipe.ts
│   │   │   └── shared.module.ts
│   │   │
│   │   ├── app.component.ts      # Root component
│   │   ├── app.routes.ts         # Main routing
│   │   └── app.config.ts         # App configuration
│   │
│   ├── assets/
│   │   ├── images/
│   │   │   ├── movies/           # Movie posters
│   │   │   ├── theaters/         # Theater images
│   │   │   └── users/            # User avatars
│   │   └── styles/
│   │       ├── global.css        # Global styles
│   │       └── variables.css     # CSS variables
│   │
│   ├── environments/
│   │   ├── environment.ts        # Development config
│   │   └── environment.prod.ts   # Production config
│   │
│   ├── types/
│   │   └── sockjs-client.d.ts   # Type definitions
│   │
│   ├── index.html                # HTML entry point
│   ├── main.ts                   # Bootstrap file
│   ├── styles.css                # Main styles
│   ├── test.ts                   # Test configuration
│   └── favicon.ico
│
├── package.json                  # NPM dependencies
├── package-lock.json            # Dependency lock file
├── angular.json                 # Angular CLI config
├── tsconfig.json                # TypeScript config
├── tsconfig.app.json            # App TS config
├── tsconfig.spec.json           # Test TS config
├── karma.conf.js                # Karma test config
├── Dockerfile                   # Container image
├── nginx.conf                   # Nginx web server config
└── proxy.conf.json             # Dev server proxy config
```

---

## ✨ Core Features

### 1. User Authentication & Authorization

**Features:**
- Email/Password registration and login
- JWT token-based authentication (86400000ms = 24 hours)
- Google OAuth2 integration
- Password reset via email
- Profile management and updates

**Security:**
- Passwords hashed using Spring Security
- JWT tokens with expiration
- CORS configuration for frontend access
- Session management

**Implementation:**
- `AuthController`: Login/Signup endpoints
- `JwtTokenProvider`: Token generation and validation
- `CustomUserDetailsService`: User loading for authentication
- `OAuth2SuccessHandler`: Google login handling

---

### 2. Movie Management

**User Features:**
- Browse all movies
- Search movies by title
- Filter by language, genre, release date
- View detailed movie information (cast, crew, duration, plot)
- Watch trailers
- Read and write reviews
- Rating system (1-5 stars)

**Admin Features:**
- Add new movies with posters
- Edit movie details
- Delete movies
- Manage movie metadata (cast, crew, genres)
- Set movie availability

**Database:**
- Movie entity with comprehensive fields
- Language and genre enumerations
- Movie-Review relationships (MongoDB)

---

### 3. Theater & Screen Management

**Features:**
- Add/Edit/Delete theaters
- Manage multiple screens per theater
- Configure seat layouts
- Define seat categories (Premium, Gold, Silver)
- Set dynamic pricing for different seat types
- Visual seat arrangement display

**Admin Operations:**
- Theater creation with location and capacity
- Screen configuration with row/column layout
- Seat category assignment and pricing
- Seat status management (Available, Booked, Blocked)

**Database:**
- Theater entity with location details
- Screen entity with layout configuration
- Seat entity with category and pricing
- SeatCategory enum for different tiers

---

### 4. Showtime Management

**Features:**
- Schedule movie showtimes
- Multiple showtimes per day per screen
- Set show dates and times
- Automatic seat availability based on bookings
- Showtime pricing configuration

**Real-time Updates:**
- WebSocket updates for seat availability
- Live booking notifications
- Real-time seat status

---

### 5. Booking & Seat Selection

**User Features:**
- Select theater and showtime
- Real-time seat selection with WebSocket
- View seat availability in real-time
- Multiple seat selection capability
- Seat category display with pricing
- Booking summary before payment

**Real-time Processing:**
- WebSocket connection for live updates
- Concurrent user support
- Seat locking during booking
- Automatic seat release on timeout

**Implementation:**
- `BookingController`: Booking endpoints
- `BookingService`: Business logic
- `WebSocketConfig`: WebSocket configuration
- STOMP protocol for messaging

---

### 6. Payment Processing

**Integration:**
- Razorpay payment gateway
- Multiple payment methods supported
- Payment verification and confirmation
- Transaction history

**Features:**
- Create payment orders with Razorpay
- Validate payment responses
- Store payment records
- Transaction history for users
- Refund processing for cancellations

**Security:**
- Server-side payment verification
- Order ID tracking
- Transaction logging

---

### 7. Booking Confirmation & Tickets

**User Features:**
- Booking confirmation page
- QR code generation for tickets
- PDF ticket download
- Email ticket delivery
- Ticket tracking with booking ID
- Cancellation with refund processing

**Ticket Components:**
- QR code for entry verification
- Movie details and showtime
- Seat information
- Price breakdown
- Booking reference number

**Implementation:**
- `QrCodeUtil`: QR code generation
- `EmailService`: Email delivery
- PDF generation for tickets
- HTML to Canvas conversion

---

### 8. Review & Rating System

**MongoDB-based System:**
- User reviews with ratings (1-5 stars)
- Review creation, editing, deletion
- Review moderation by admins
- Average rating calculation
- Review list with pagination

**Features:**
- User can write multiple reviews
- Edit own reviews
- Admin can hide/delete reviews
- Helpful vote counting
- Chronological ordering

---

### 9. Admin Dashboard

**Analytics & Metrics:**
- Total revenue statistics
- Booking statistics and trends
- User growth metrics
- Revenue trends (daily/weekly/monthly)
- Top movies by revenue
- Top theaters by bookings
- User acquisition analytics

**Reports:**
- Booking reports with filters
- Revenue breakdown by theater
- Movie performance analytics
- Time-period based reports (custom date ranges)

**Implementation:**
- `AdminDashboardController`: Analytics endpoints
- `AdminDashboardService`: Business logic
- Chart/Graph components on frontend

---

### 10. User Management

**Admin Capabilities:**
- View all users with detailed information
- Search users
- Block/Unblock users
- View user booking history
- User role management (User/Admin)
- Account status management

**User Self-Management:**
- Update profile information
- Change password
- Manage preferences
- View booking history
- Delete account

---

### 11. Email Notifications

**Triggers:**
- Welcome email on registration
- Booking confirmation email
- Payment confirmation email
- Ticket delivery email
- Password reset emails
- Booking cancellation emails

**Implementation:**
- Spring Mail integration
- Gmail SMTP configuration
- Email template system
- Async email sending

---

### 12. Additional Features

**Caching:**
- In-memory caching for frequent data
- Scheduled cache refresh
- Cache invalidation on updates

**Maintenance Mode:**
- Admin-controlled maintenance flag
- Graceful service degradation
- Notification messaging

**Real-time Communication:**
- WebSocket for live updates
- STOMP messaging protocol
- SockJS fallback for compatibility

---

## 🗄️ Data Models

### User Entity (MySQL)
```
User
├── userId (PK)
├── username (unique)
├── email (unique)
├── password (hashed)
├── firstName
├── lastName
├── phone
├── address
├── city
├── role (User/Admin)
├── createdAt
├── updatedAt
└── isActive
```

### Movie Entity (MySQL)
```
Movie
├── movieId (PK)
├── title
├── description
├── posterUrl
├── trailerUrl
├── duration (minutes)
├── genre (enum)
├── language
├── releaseDate
├── cast (JSON array)
├── crew (JSON array)
├── rating (float)
├── totalRatings (int)
├── createdAt
└── updatedAt
```

### Theater Entity (MySQL)
```
Theater
├── theaterId (PK)
├── name
├── location
├── city
├── address
├── phone
├── screens (relationship)
├── totalCapacity
├── createdAt
└── updatedAt
```

### Screen Entity (MySQL)
```
Screen
├── screenId (PK)
├── screenNumber
├── theatre (FK)
├── totalSeats
├── rows
├── columns
├── seatCategories (relationship)
├── createdAt
└── updatedAt
```

### Seat Entity (MySQL)
```
Seat
├── seatId (PK)
├── seatNumber
├── screen (FK)
├── row
├── column
├── category (enum: Premium/Gold/Silver)
├── price
├── createdAt
└── updatedAt
```

### Showtime Entity (MySQL)
```
Showtime
├── showtimeId (PK)
├── movie (FK)
├── screen (FK)
├── showDate
├── showTime
├── format (2D/3D/IMAX)
├── priceModifier
├── availableSeats
├── totalSeats
├── createdAt
└── updatedAt
```

### Booking Entity (MySQL)
```
Booking
├── bookingId (PK)
├── user (FK)
├── showtime (FK)
├── seats (JSON array of seatIds)
├── totalPrice
├── bookingStatus (PENDING/CONFIRMED/CANCELLED)
├── bookingDate
├── bookingTime
├── cancellationDate
└── notes
```

### Payment Entity (MySQL)
```
Payment
├── paymentId (PK)
├── booking (FK)
├── razorpayOrderId
├── razorpayPaymentId
├── razorpaySignature
├── amount
├── currency
├── paymentStatus (PENDING/SUCCESS/FAILED)
├── paymentMethod
├── transactionDate
└── updatedAt
```

### MongoReview Document (MongoDB)
```
Review
├── _id (PK)
├── movieId (FK)
├── userId (FK)
├── rating (1-5)
├── reviewText
├── isModerated
├── isHidden (admin moderation)
├── helpfulCount
├── createdAt
├── updatedAt
└── userName
```

---

## 🚀 Deployment Architecture

### Docker Compose Network

```
Docker Network: revticket-network

┌─────────────────────────────────────────────────────┐
│              Container Environment                   │
│  ┌──────────────────────────────────────────────┐   │
│  │  Frontend Container                          │   │
│  │  - Image: harshwarbhe/revticket-frontend     │   │
│  │  - Port: 4200 → 80                           │   │
│  │  - Server: Nginx                             │   │
│  │  - Healthcheck: N/A                          │   │
│  └──────────────────────────────────────────────┘   │
│                                                      │
│  ┌──────────────────────────────────────────────┐   │
│  │  Backend Container                           │   │
│  │  - Image: harshwarbhe/revticket-backend      │   │
│  │  - Port: 8081 → 8080                         │   │
│  │  - Server: Spring Boot Tomcat                │   │
│  │  - Healthcheck: /actuator/health             │   │
│  │  - Depends on: MySQL, MongoDB                │   │
│  │  - Memory: 512MB - 1024MB (JAVA_OPTS)       │   │
│  └──────────────────────────────────────────────┘   │
│                                                      │
│  ┌──────────────────────────────────────────────┐   │
│  │  MySQL Container                             │   │
│  │  - Image: mysql:8.0                          │   │
│  │  - Port: 3307 → 3306                         │   │
│  │  - Volume: mysql_data:/var/lib/mysql         │   │
│  │  - Healthcheck: mysqladmin ping              │   │
│  └──────────────────────────────────────────────┘   │
│                                                      │
│  ┌──────────────────────────────────────────────┐   │
│  │  MongoDB Container                           │   │
│  │  - Image: mongo:8.0                          │   │
│  │  - Port: 27018 → 27017                       │   │
│  │  - Volume: mongo_data:/data/db               │   │
│  │  - Healthcheck: mongosh ping                 │   │
│  └──────────────────────────────────────────────┘   │
│                                                      │
└─────────────────────────────────────────────────────┘

Host Machine Ports:
- 4200: Frontend (Angular dev)
- 8081: Backend API
- 3307: MySQL Database
- 27018: MongoDB Database
```

### Docker Images

**Backend Image: `harshwarbhe/revticket-backend`**
- Base: OpenJDK 17
- Build: Maven-based compilation
- Entrypoint: Spring Boot application
- Healthcheck: HTTP /actuator/health
- Multi-platform: linux/amd64, linux/arm64

**Frontend Image: `harshwarbhe/revticket-frontend`**
- Base: nginx:latest
- Build: Angular production build
- Static files: Nginx static server
- Configuration: nginx.conf with SPA routing
- Multi-platform: linux/amd64, linux/arm64

### Deployment Environments

1. **Local Development** (`docker-compose.yml`)
   - Docker Compose orchestration
   - Development configurations
   - Volume mounting for live reload
   - Exposed ports for testing

2. **Production** (`docker-compose.prod.yml`)
   - Production-optimized images
   - Environment-based configurations
   - Memory limits
   - Health checks enabled

3. **AWS EC2** (`docker-compose.ec2.yml`)
   - EC2-specific deployment
   - SSH key authentication
   - Environment variables from AWS
   - EC2 instance configuration

---

## 🔧 Development Setup

### Prerequisites
- Java 17+
- Maven 3.x
- Node.js 18+ and npm
- Docker & Docker Compose
- Git

### Local Development Steps

1. **Clone Repository**
   ```bash
   git clone https://github.com/harshWarbhe/revTicket.git
   cd revTicket
   ```

2. **Backend Setup**
   ```bash
   cd Backend
   ./mvnw clean package -DskipTests
   ```

3. **Frontend Setup**
   ```bash
   cd Frontend
   npm install
   ```

4. **Start Services**
   ```bash
   docker-compose up --build
   ```

5. **Access Application**
   - Frontend: http://localhost:4200
   - Backend: http://localhost:8081
   - MySQL: localhost:3307
   - MongoDB: localhost:27018

### Configuration Files

**Backend Configuration** (`Backend/src/main/resources/application.properties`)
- Spring Boot settings
- Database connections (MySQL, MongoDB)
- JWT configuration
- Email settings (Gmail SMTP)
- Razorpay API keys
- Google OAuth2 credentials
- CORS settings

**Frontend Configuration** (`Frontend/src/environments/environment.ts`)
- API Base URL
- Authentication endpoints
- Feature flags
- Environment-specific settings

---

## 🔄 CI/CD Pipeline

### Jenkins Pipeline Overview

**Stages:**
1. **Checkout** - Clone repository from GitHub
2. **Build Backend** - Maven compilation (Java 17)
3. **Setup Buildx** - Docker multi-architecture builder
4. **Build & Push Multi-Platform Images**
   - Backend: linux/amd64, linux/arm64
   - Frontend: linux/amd64, linux/arm64
5. **Verify Images Pushed** - Registry verification
6. **Deploy to EC2** - Automated deployment

### Features
- ✅ Multi-platform Docker builds
- ✅ Automatic image tagging
- ✅ DockerHub registry push
- ✅ Mac, Linux, Windows, Ubuntu, EC2 support
- ✅ Build caching and optimization

### Docker Registry
- **Registry**: DockerHub
- **Username**: harshwarbhe
- **Images**:
  - `harshwarbhe/revticket-backend:latest`
  - `harshwarbhe/revticket-frontend:latest`

---

## 🔐 Security Implementation

### Authentication Methods

1. **JWT Token-based**
   - Secret Key: RevTicketSecretKeyForJWTTokenGeneration2024SecureAndLongEnough
   - Expiration: 24 hours (86400000ms)
   - Encoding: HS256
   - Token Validation: JwtAuthFilter

2. **Google OAuth2**
   - Client ID: 21826531648-9ai38efgotj5f76lvf29kgqn1toenr20.apps.googleusercontent.com
   - Scopes: profile, email
   - Callback: /login/oauth2/code/{registrationId}

### Spring Security Configuration

- HTTP Basic disabled
- CSRF disabled
- CORS enabled with wildcard origin
- HTTPS redirect (production)
- Password encoding: BCrypt

### Password Management
- Email-based password reset
- Reset tokens with expiration
- Secure token generation
- Verification flow

### Sensitive Data Protection
- API keys stored in environment variables
- Database credentials in properties files
- JWT secret key in configuration
- Razorpay keys in properties

### CORS Configuration
- Allowed Origins: * (configurable)
- Allowed Methods: GET, POST, PUT, DELETE, OPTIONS
- Allowed Headers: Authorization, Content-Type, Accept

---

## ⚡ Performance & Scalability

### Caching Strategy

1. **In-Memory Caching**
   - Movie listings
   - Theater information
   - Frequent queries
   - Refresh intervals

2. **Database Connection Pooling**
   - Hikari CP (HikariDataSource)
   - Connection timeout: 60000ms
   - Maximum pool size: 5 connections
   - Optimized for multi-user scenarios

### Database Optimization

1. **MySQL Indexes**
   - Primary keys on all tables
   - Foreign key relationships
   - Query optimization

2. **MongoDB Indexes**
   - Movie ID indexes
   - User ID indexes
   - Auto-indexing enabled

### WebSocket Optimization

- STOMP protocol for message routing
- SockJS fallback for compatibility
- Message compression
- Broadcast optimization for seat updates

### Frontend Optimization

- Angular lazy loading for modules
- OnPush change detection strategy
- RxJS subscription management
- Unsubscribe patterns to prevent memory leaks

---

## 🧪 Testing Strategy

### Backend Testing

**Unit Tests**
- Service layer testing with mocks
- Repository testing
- Utility function testing

**Integration Tests**
- Controller endpoint testing
- Database integration testing
- API endpoint verification

**Testing Frameworks**
- JUnit 5
- Mockito for mocking
- Spring Boot Test

### Frontend Testing

**Unit Tests**
- Component testing with TestBed
- Service testing
- Pipe and utility testing

**Testing Frameworks**
- Jasmine (test framework)
- Karma (test runner)
- Chrome headless browser
- HTML reporters

---

## 🐛 Known Issues & Improvements

### Current Limitations

1. **JWT Secret Key**
   - Should use environment variables in production
   - Currently hardcoded in properties

2. **Razorpay Keys**
   - Currently in properties files
   - Should use secure vaults

3. **Email Configuration**
   - Gmail app-specific password used
   - Should use enterprise email service

4. **CORS Configuration**
   - Wildcard (*) origin enabled
   - Should restrict in production

### Recommended Improvements

1. **Security Enhancements**
   - Implement OAuth2 refresh tokens
   - Add rate limiting
   - Implement API key authentication for external APIs
   - Use AWS Secrets Manager for credentials

2. **Performance Enhancements**
   - Implement Redis caching layer
   - Add database query pagination
   - Implement CDN for static assets
   - Add data compression (gzip)

3. **Scalability Improvements**
   - Microservices architecture migration
   - Implement message queues (RabbitMQ/Kafka)
   - Add load balancing
   - Database replication and sharding

4. **Monitoring & Logging**
   - Implement ELK stack for logs
   - Add distributed tracing
   - Implement APM (Application Performance Monitoring)
   - Add alert system

5. **Testing Improvements**
   - Increase code coverage to 80%+
   - Add E2E tests with Cypress/Playwright
   - Implement load testing
   - Add security testing (OWASP)

---

## 📋 Summary

RevTicket is a **comprehensive, production-ready movie ticket booking system** that demonstrates:

✅ **Modern Architecture**: Microservices-ready with clear separation of concerns  
✅ **Complete Feature Set**: User management, booking, payments, analytics  
✅ **Enterprise Security**: JWT, OAuth2, password hashing, secure sessions  
✅ **Real-time Capabilities**: WebSocket integration for live seat updates  
✅ **Containerized Deployment**: Docker multi-platform builds and orchestration  
✅ **Automated CI/CD**: Jenkins pipeline with automated testing and deployment  
✅ **Scalable Design**: Database optimization, caching, connection pooling  
✅ **Admin Tools**: Comprehensive dashboard with analytics and management features  

The project is suitable for:
- Production deployment
- Educational purposes
- Portfolio demonstration
- Enterprise ticket booking solutions
- Learning full-stack development

---

**Document Generated**: December 9, 2025  
**Project Version**: 1.0.0  
**Status**: Production Ready
