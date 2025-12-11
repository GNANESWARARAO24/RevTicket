# RevTicket - Movie Booking System
## Complete Project Documentation

---

## 📋 Table of Contents
1. [Project Overview](#project-overview)
2. [System Architecture](#system-architecture)
3. [Technology Stack](#technology-stack)
4. [Project Structure](#project-structure)
5. [Features](#features)
6. [Database Schema](#database-schema)
7. [API Endpoints](#api-endpoints)
8. [Setup & Installation](#setup--installation)
9. [Deployment](#deployment)
10. [Configuration](#configuration)
11. [Security](#security)
12. [Testing](#testing)
13. [Troubleshooting](#troubleshooting)

---

## 🎯 Project Overview

**RevTicket** is a full-stack movie ticket booking system that allows users to browse movies, book tickets, make payments, and write reviews. It includes an admin panel for managing theaters, screens, showtimes, and generating reports.

### Key Capabilities
- User authentication (JWT + OAuth2 Google)
- Movie browsing and search
- Real-time seat selection with WebSocket
- Payment integration (Razorpay)
- Review system with MongoDB
- Admin dashboard with analytics
- Email notifications
- QR code ticket generation
- Multi-language support
- Maintenance mode

---

## 🏗️ System Architecture

### Architecture Type
**Microservices-based Architecture** with:
- **Backend**: Spring Boot REST API
- **Frontend**: Angular SPA
- **Databases**: MySQL (relational) + MongoDB (reviews)
- **Containerization**: Docker & Docker Compose
- **CI/CD**: Jenkins Pipeline
- **Registry**: DockerHub

### Component Diagram
```
┌─────────────┐      ┌──────────────┐      ┌─────────────┐
│   Angular   │─────▶│  Spring Boot │─────▶│    MySQL    │
│  Frontend   │      │   Backend    │      │  (Primary)  │
│  (Port 80)  │◀─────│  (Port 8080) │      └─────────────┘
└─────────────┘      └──────────────┘              │
                            │                      │
                            │                      ▼
                            │              ┌─────────────┐
                            └─────────────▶│   MongoDB   │
                                           │  (Reviews)  │
                                           └─────────────┘
```

### Deployment Architecture
```
┌──────────────────────────────────────────────────────┐
│                    Docker Network                     │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐     │
│  │  Frontend  │  │  Backend   │  │   MySQL    │     │
│  │  :4200→80  │  │  :8081→8080│  │  :3307→3306│     │
│  └────────────┘  └────────────┘  └────────────┘     │
│                                   ┌────────────┐     │
│                                   │  MongoDB   │     │
│                                   │ :27018→27017│    │
│                                   └────────────┘     │
└──────────────────────────────────────────────────────┘
```

---

## 💻 Technology Stack

### Backend
| Technology | Version | Purpose |
|------------|---------|---------|
| Java | 17+ | Programming Language |
| Spring Boot | 3.2.0 | Application Framework |
| Spring Security | 3.2.0 | Authentication & Authorization |
| Spring Data JPA | 3.2.0 | MySQL ORM |
| Spring Data MongoDB | 3.2.0 | MongoDB Integration |
| Spring WebSocket | 3.2.0 | Real-time Communication |
| JWT (JJWT) | 0.12.3 | Token-based Auth |
| Razorpay SDK | 1.4.6 | Payment Gateway |
| Lombok | 1.18.36 | Code Generation |
| Maven | 3.x | Build Tool |

### Frontend
| Technology | Version | Purpose |
|------------|---------|---------|
| Angular | 18.0.0 | Frontend Framework |
| TypeScript | 5.4.0 | Programming Language |
| RxJS | 7.8.0 | Reactive Programming |
| STOMP.js | 7.2.1 | WebSocket Client |
| SockJS | 1.6.1 | WebSocket Fallback |
| QRCode | 1.5.4 | QR Code Generation |
| jsPDF | 3.0.4 | PDF Generation |
| html2canvas | 1.4.1 | HTML to Canvas |

### Databases
- **MySQL 8.0**: User data, bookings, movies, theaters
- **MongoDB 8.0**: Reviews and ratings

### DevOps
- **Docker**: Containerization
- **Docker Compose**: Multi-container orchestration
- **Jenkins**: CI/CD Pipeline
- **DockerHub**: Container Registry
- **Nginx**: Frontend web server

---

## 📁 Project Structure

### Root Directory
```
revTicket/
├── Backend/                    # Spring Boot application
├── Frontend/                   # Angular application
├── docker-compose.yml          # Local development
├── docker-compose.prod.yml     # Production deployment
├── docker-compose.ec2.yml      # EC2 deployment
├── Jenkinsfile                 # CI/CD pipeline
├── deploy-ec2.sh              # EC2 deployment script
├── start.sh / start.bat       # Quick start scripts
├── README.md                   # Setup guide
├── DEPLOYMENT_SUMMARY.md       # Deployment documentation
└── JENKINS_DEPLOYMENT_GUIDE.md # Jenkins setup guide
```

### Backend Structure
```
Backend/
├── src/main/java/com/revticket/
│   ├── config/                 # Configuration classes
│   │   ├── CacheConfig.java
│   │   ├── DataLoader.java
│   │   ├── SecurityConfig.java
│   │   └── WebSocketConfig.java
│   ├── controller/             # REST API endpoints
│   │   ├── AuthController.java
│   │   ├── BookingController.java
│   │   ├── MovieController.java
│   │   ├── PaymentController.java
│   │   ├── ReviewController.java
│   │   ├── TheaterController.java
│   │   └── Admin*.java (Admin controllers)
│   ├── dto/                    # Data Transfer Objects
│   ├── entity/                 # JPA Entities
│   │   ├── User.java
│   │   ├── Movie.java
│   │   ├── Theater.java
│   │   ├── Screen.java
│   │   ├── Showtime.java
│   │   ├── Booking.java
│   │   ├── Payment.java
│   │   └── MongoReview.java
│   ├── repository/             # Data access layer
│   ├── service/                # Business logic
│   ├── security/               # JWT & Security
│   ├── scheduler/              # Scheduled tasks
│   ├── exception/              # Exception handling
│   └── util/                   # Utility classes
├── src/main/resources/
│   └── application.properties  # Configuration
├── Dockerfile                  # Backend container
└── pom.xml                     # Maven dependencies
```

### Frontend Structure
```
Frontend/
├── src/app/
│   ├── auth/                   # Authentication module
│   │   ├── login/
│   │   ├── signup/
│   │   └── forgot-password/
│   ├── user/                   # User module
│   │   ├── home/
│   │   ├── movie-details/
│   │   ├── seat-selection/
│   │   ├── booking-confirmation/
│   │   ├── my-bookings/
│   │   └── profile/
│   ├── admin/                  # Admin module
│   │   ├── dashboard/
│   │   ├── movies/
│   │   ├── theaters/
│   │   ├── screens/
│   │   ├── showtimes/
│   │   ├── users/
│   │   ├── reviews/
│   │   └── reports/
│   ├── core/                   # Core services
│   │   ├── services/
│   │   ├── guards/
│   │   └── interceptors/
│   └── shared/                 # Shared components
│       ├── components/
│       ├── models/
│       └── pipes/
├── src/environments/           # Environment configs
├── Dockerfile                  # Frontend container
├── nginx.conf                  # Nginx configuration
├── package.json                # NPM dependencies
└── angular.json                # Angular configuration
```

---

## ✨ Features

### User Features
1. **Authentication**
   - Email/Password registration and login
   - Google OAuth2 login
   - JWT token-based authentication
   - Password reset via email
   - Profile management

2. **Movie Browsing**
   - Browse all movies
   - Search movies by title
   - Filter by language, genre
   - View movie details (cast, crew, trailer)
   - Read reviews and ratings

3. **Ticket Booking**
   - Select theater and showtime
   - Real-time seat selection (WebSocket)
   - Multiple seat categories (Premium, Gold, Silver)
   - Dynamic pricing
   - Seat availability updates

4. **Payment**
   - Razorpay payment integration
   - Multiple payment methods
   - Payment verification
   - Transaction history

5. **Booking Management**
   - View booking history
   - Download tickets (PDF with QR code)
   - Cancel bookings (with refund)
   - Email notifications

6. **Reviews**
   - Write movie reviews
   - Rate movies (1-5 stars)
   - Edit/delete own reviews
   - View all reviews

### Admin Features
1. **Dashboard**
   - Total revenue analytics
   - Booking statistics
   - User growth metrics
   - Revenue trends (daily/weekly/monthly)
   - Top movies by revenue

2. **Movie Management**
   - Add/Edit/Delete movies
   - Upload movie posters
   - Manage cast and crew
   - Set movie duration, language, genre

3. **Theater Management**
   - Add/Edit/Delete theaters
   - Manage theater locations
   - Set theater capacity

4. **Screen Management**
   - Add screens to theaters
   - Configure seat layouts
   - Define seat categories and pricing
   - Visual seat arrangement

5. **Showtime Management**
   - Schedule movie showtimes
   - Set show dates and times
   - Manage screen assignments

6. **User Management**
   - View all users
   - Block/Unblock users
   - View user booking history

7. **Review Moderation**
   - View all reviews
   - Delete inappropriate reviews

8. **Reports**
   - Booking reports with filters
   - Revenue reports
   - Export to CSV/PDF

9. **Settings**
   - Maintenance mode toggle
   - System-wide configurations

---

## 🗄️ Database Schema

### MySQL Tables

#### Users Table
```sql
CREATE TABLE users (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    email VARCHAR(255) UNIQUE NOT NULL,
    password VARCHAR(255),
    full_name VARCHAR(255),
    phone_number VARCHAR(20),
    role ENUM('USER', 'ADMIN'),
    is_blocked BOOLEAN DEFAULT FALSE,
    oauth_provider VARCHAR(50),
    oauth_id VARCHAR(255),
    created_at TIMESTAMP,
    updated_at TIMESTAMP
);
```

#### Movies Table
```sql
CREATE TABLE movies (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    title VARCHAR(255) NOT NULL,
    description TEXT,
    duration INT,
    release_date DATE,
    poster_url VARCHAR(500),
    trailer_url VARCHAR(500),
    language_id BIGINT,
    genre VARCHAR(100),
    cast TEXT,
    crew TEXT,
    rating DECIMAL(2,1),
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP
);
```

#### Theaters Table
```sql
CREATE TABLE theaters (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    location VARCHAR(255),
    city VARCHAR(100),
    total_screens INT,
    created_at TIMESTAMP
);
```

#### Screens Table
```sql
CREATE TABLE screens (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    screen_name VARCHAR(100),
    theater_id BIGINT,
    total_seats INT,
    seat_layout JSON,
    FOREIGN KEY (theater_id) REFERENCES theaters(id)
);
```

#### Showtimes Table
```sql
CREATE TABLE showtimes (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    movie_id BIGINT,
    screen_id BIGINT,
    show_date DATE,
    show_time TIME,
    available_seats INT,
    FOREIGN KEY (movie_id) REFERENCES movies(id),
    FOREIGN KEY (screen_id) REFERENCES screens(id)
);
```

#### Bookings Table
```sql
CREATE TABLE bookings (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    user_id BIGINT,
    showtime_id BIGINT,
    booking_date TIMESTAMP,
    total_amount DECIMAL(10,2),
    status ENUM('PENDING', 'CONFIRMED', 'CANCELLED'),
    seats JSON,
    qr_code VARCHAR(500),
    FOREIGN KEY (user_id) REFERENCES users(id),
    FOREIGN KEY (showtime_id) REFERENCES showtimes(id)
);
```

#### Payments Table
```sql
CREATE TABLE payments (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    booking_id BIGINT,
    amount DECIMAL(10,2),
    payment_method VARCHAR(50),
    razorpay_order_id VARCHAR(255),
    razorpay_payment_id VARCHAR(255),
    status ENUM('PENDING', 'SUCCESS', 'FAILED'),
    payment_date TIMESTAMP,
    FOREIGN KEY (booking_id) REFERENCES bookings(id)
);
```

### MongoDB Collections

#### Reviews Collection
```javascript
{
    _id: ObjectId,
    movieId: Long,
    userId: Long,
    userName: String,
    rating: Number (1-5),
    comment: String,
    createdAt: Date,
    updatedAt: Date
}
```

---

## 🔌 API Endpoints

### Authentication APIs
```
POST   /api/auth/signup              - User registration
POST   /api/auth/login               - User login
POST   /api/auth/google              - Google OAuth login
POST   /api/auth/forgot-password     - Request password reset
POST   /api/auth/reset-password      - Reset password
POST   /api/auth/change-password     - Change password
GET    /api/auth/verify-token        - Verify JWT token
```

### Movie APIs
```
GET    /api/movies                   - Get all movies
GET    /api/movies/{id}              - Get movie by ID
GET    /api/movies/search            - Search movies
GET    /api/movies/language/{lang}   - Filter by language
```

### Theater APIs
```
GET    /api/theaters                 - Get all theaters
GET    /api/theaters/{id}            - Get theater by ID
GET    /api/theaters/city/{city}     - Get theaters by city
```

### Showtime APIs
```
GET    /api/showtimes/movie/{id}     - Get showtimes for movie
GET    /api/showtimes/{id}           - Get showtime details
GET    /api/showtimes/{id}/seats     - Get available seats
```

### Booking APIs
```
POST   /api/bookings                 - Create booking
GET    /api/bookings/user            - Get user bookings
GET    /api/bookings/{id}            - Get booking details
PUT    /api/bookings/{id}/cancel     - Cancel booking
GET    /api/bookings/{id}/ticket     - Download ticket
```

### Payment APIs
```
POST   /api/payments/create-order    - Create Razorpay order
POST   /api/payments/verify          - Verify payment
GET    /api/payments/booking/{id}    - Get payment details
```

### Review APIs
```
POST   /api/reviews                  - Create review
GET    /api/reviews/movie/{id}       - Get movie reviews
PUT    /api/reviews/{id}             - Update review
DELETE /api/reviews/{id}             - Delete review
```

### Admin APIs
```
# Movies
POST   /api/admin/movies             - Add movie
PUT    /api/admin/movies/{id}        - Update movie
DELETE /api/admin/movies/{id}        - Delete movie

# Theaters
POST   /api/admin/theaters           - Add theater
PUT    /api/admin/theaters/{id}      - Update theater
DELETE /api/admin/theaters/{id}      - Delete theater

# Screens
POST   /api/admin/screens            - Add screen
PUT    /api/admin/screens/{id}       - Update screen
DELETE /api/admin/screens/{id}       - Delete screen

# Showtimes
POST   /api/admin/showtimes          - Add showtime
PUT    /api/admin/showtimes/{id}     - Update showtime
DELETE /api/admin/showtimes/{id}     - Delete showtime

# Users
GET    /api/admin/users              - Get all users
PUT    /api/admin/users/{id}/block   - Block/Unblock user

# Dashboard
GET    /api/admin/dashboard/stats    - Get dashboard statistics
GET    /api/admin/reports/bookings   - Get booking reports

# Settings
GET    /api/admin/settings           - Get settings
PUT    /api/admin/settings           - Update settings
```

### WebSocket Endpoints
```
CONNECT /ws                          - WebSocket connection
SUBSCRIBE /topic/seats/{showtimeId}  - Subscribe to seat updates
SEND /app/select-seat                - Select seat
SEND /app/release-seat               - Release seat
```

---

## 🚀 Setup & Installation

### Prerequisites
- Java 17+
- Node.js 18+
- Maven 3.x
- Docker & Docker Compose
- Git

### Local Development Setup

#### 1. Clone Repository
```bash
git clone <repository-url>
cd revTicket
```

#### 2. Backend Setup
```bash
cd Backend
./mvnw clean install
./mvnw spring-boot:run
```

Backend runs on: http://localhost:8080

#### 3. Frontend Setup
```bash
cd Frontend
npm install
ng serve
```

Frontend runs on: http://localhost:4200

#### 4. Database Setup (Docker)
```bash
# Start MySQL
docker run -d -p 3307:3306 \
  -e MYSQL_ROOT_PASSWORD=Admin123 \
  -e MYSQL_DATABASE=revticket_db \
  --name revticket-mysql mysql:8.0

# Start MongoDB
docker run -d -p 27018:27017 \
  --name revticket-mongodb mongo:8.0
```

### Docker Compose Setup (Recommended)

#### Quick Start
```bash
# Build and start all services
docker-compose up --build

# Or use the start script
./start.sh        # Linux/Mac
start.bat         # Windows
```

#### Services
- Frontend: http://localhost:4200
- Backend: http://localhost:8081
- MySQL: localhost:3307
- MongoDB: localhost:27018

#### Stop Services
```bash
docker-compose down
```

#### View Logs
```bash
docker-compose logs -f backend
docker-compose logs -f frontend
```

---

## 🌐 Deployment

### Docker Compose Production

#### 1. Build Images
```bash
cd Backend
docker build -t revticket-backend .

cd ../Frontend
docker build -t revticket-frontend .
```

#### 2. Deploy
```bash
docker-compose -f docker-compose.prod.yml up -d
```

### Jenkins CI/CD Pipeline

#### Pipeline Stages
1. **Checkout**: Clone from GitHub
2. **Build Backend**: Maven package
3. **Setup Buildx**: Multi-platform builder
4. **Build & Push Images**: Build for amd64/arm64
5. **Verify**: Confirm DockerHub push
6. **Deploy**: EC2 deployment instructions

#### Jenkins Setup
```bash
# Install plugins
- Docker Pipeline
- Git
- Maven Integration

# Configure credentials
- Add DockerHub credentials (ID: docker-credentials)

# Create pipeline
- New Item → Pipeline
- SCM: Git
- Script Path: Jenkinsfile
```

### AWS EC2 Deployment

#### 1. Initial Setup
```bash
# SSH to EC2
ssh -i "revticket.pem" ubuntu@<EC2-IP>

# Install Docker
sudo apt update
sudo apt install docker.io docker-compose -y
sudo usermod -aG docker ubuntu

# Clone repository
git clone <repository-url>
cd revticket
```

#### 2. Deploy
```bash
# Using deployment script
chmod +x deploy-ec2.sh
./deploy-ec2.sh

# Or manually
docker-compose -f docker-compose.ec2.yml pull
docker-compose -f docker-compose.ec2.yml up -d
```

#### 3. Access Application
- Frontend: http://<EC2-IP>:4200
- Backend: http://<EC2-IP>:8081

### Environment Variables

Create `.env` file:
```env
# Database
MYSQL_ROOT_PASSWORD=Admin123
MYSQL_DATABASE=revticket_db
MYSQL_USER=revticket
MYSQL_PASSWORD=Admin123

# MongoDB
MONGODB_URI=mongodb://mongodb:27017/revticket_reviews

# Application
SPRING_PROFILE=prod
FRONTEND_URL=http://localhost:4200

# Razorpay
RAZORPAY_KEY_ID=your_key_id
RAZORPAY_KEY_SECRET=your_key_secret

# Email
MAIL_USERNAME=your_email@gmail.com
MAIL_PASSWORD=your_app_password

# OAuth2
GOOGLE_CLIENT_ID=your_client_id
GOOGLE_CLIENT_SECRET=your_client_secret
```

---

## ⚙️ Configuration

### Backend Configuration
**File**: `Backend/src/main/resources/application.properties`

```properties
# Server
server.port=8080

# Database
spring.datasource.url=jdbc:mysql://localhost:3306/revticket_db
spring.datasource.username=root
spring.datasource.password=Admin123

# MongoDB
spring.data.mongodb.uri=mongodb://localhost:27017/revticket_reviews

# JWT
jwt.secret=YourSecretKey
jwt.expiration=86400000

# Email
spring.mail.host=smtp.gmail.com
spring.mail.port=587
spring.mail.username=your_email@gmail.com
spring.mail.password=your_app_password

# Razorpay
razorpay.key.id=your_key_id
razorpay.key.secret=your_key_secret

# OAuth2
spring.security.oauth2.client.registration.google.client-id=your_client_id
spring.security.oauth2.client.registration.google.client-secret=your_client_secret
```

### Frontend Configuration
**File**: `Frontend/src/environments/environment.ts`

```typescript
export const environment = {
  production: false,
  apiUrl: 'http://localhost:8080/api',
  wsUrl: 'http://localhost:8080/ws',
  razorpayKey: 'your_razorpay_key'
};
```

### Docker Configuration

#### Backend Dockerfile
```dockerfile
FROM openjdk:17-jdk-slim
WORKDIR /app
COPY target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

#### Frontend Dockerfile
```dockerfile
FROM node:18 AS build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=build /app/dist/rev-ticket/browser /usr/share/nginx/html
COPY nginx.conf /etc/nginx/nginx.conf
EXPOSE 80
```

---

## 🔒 Security

### Authentication
- **JWT Tokens**: Stateless authentication
- **OAuth2**: Google login integration
- **Password Hashing**: BCrypt encryption
- **Token Expiration**: 24 hours

### Authorization
- **Role-based Access**: USER, ADMIN roles
- **Route Guards**: Frontend route protection
- **API Security**: Method-level security

### Security Headers
```java
http.headers()
    .contentSecurityPolicy("default-src 'self'")
    .xssProtection()
    .frameOptions().deny();
```

### CORS Configuration
```java
@CrossOrigin(origins = "*")
// Configured in SecurityConfig
```

### Input Validation
- **Backend**: @Valid annotations
- **Frontend**: Reactive forms validation
- **SQL Injection**: JPA parameterized queries

---

## 🧪 Testing

### Backend Testing
```bash
cd Backend
./mvnw test
```

### Frontend Testing
```bash
cd Frontend
npm test
```

### Integration Testing
```bash
# Start services
docker-compose up -d

# Run tests
./mvnw verify
```

---

## 🐛 Troubleshooting

### Port Conflicts
```bash
# Check ports
lsof -ti:8080 | xargs kill -9  # Backend
lsof -ti:4200 | xargs kill -9  # Frontend
lsof -ti:3307 | xargs kill -9  # MySQL
```

### Database Connection Issues
```bash
# Check MySQL
docker-compose logs mysql
docker exec -it revticket-mysql mysql -uroot -pAdmin123

# Check MongoDB
docker-compose logs mongodb
docker exec -it revticket-mongodb mongosh
```

### Docker Issues
```bash
# Rebuild everything
docker-compose down -v
docker-compose build --no-cache
docker-compose up -d

# Clean Docker
docker system prune -a
```

### Application Logs
```bash
# Backend logs
docker-compose logs -f backend

# Frontend logs
docker-compose logs -f frontend

# All logs
docker-compose logs -f
```

### Common Errors

#### 1. JWT Token Invalid
- Check token expiration
- Verify JWT secret matches
- Clear browser localStorage

#### 2. Payment Failed
- Verify Razorpay credentials
- Check network connectivity
- Review payment logs

#### 3. WebSocket Connection Failed
- Check CORS configuration
- Verify WebSocket endpoint
- Check firewall settings

---

## 📊 Performance Optimization

### Backend
- Connection pooling (HikariCP)
- Query optimization with indexes
- Caching with Spring Cache
- Async processing for emails

### Frontend
- Lazy loading modules
- AOT compilation
- Image optimization
- Service worker caching

### Database
- Indexed columns (email, movie_id, user_id)
- Query optimization
- Connection pooling

---

## 📝 Default Credentials

### Admin Account
```
Email: admin@revticket.com
Password: Admin@123
```

### Test User
```
Email: user@revticket.com
Password: User@123
```

### Database
```
MySQL Root: Admin123
MongoDB: No authentication
```

---

## 🤝 Contributing

1. Fork the repository
2. Create feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open Pull Request

---

## 📄 License

This project is licensed under the MIT License.

---

## 📞 Support

- **Documentation**: See README.md files
- **Issues**: GitHub Issues
- **Email**: support@revticket.com

---

## 🎉 Acknowledgments

- Spring Boot Team
- Angular Team
- Razorpay
- Docker
- Jenkins

---

**Last Updated**: 2024
**Version**: 1.0.0
**Author**: RevTicket Team
