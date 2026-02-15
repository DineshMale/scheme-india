# Design Document
## Scheme India – AI-Powered Government Scheme Eligibility & Recommendation Platform

## 1. Overview

This document outlines the system design and architecture for Scheme India, an AI-powered platform that helps Indian citizens discover government schemes they are eligible for. The platform uses machine learning to analyze user profiles and recommend relevant schemes with high accuracy.

### Design Goals
- Scalable architecture supporting 100,000+ users
- Fast ML predictions (<2 seconds)
- Secure authentication and data protection
- Modular and maintainable codebase
- Mobile-responsive user interface

## 2. High-Level Architecture

### System Architecture Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                        Client Layer                          │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │   Web App    │  │  Admin Panel │  │ Mobile (Future)│     │
│  │  (React.js)  │  │  (React.js)  │  │               │      │
│  └──────────────┘  └──────────────┘  └──────────────┘      │
└─────────────────────────────────────────────────────────────┘
                            │
                    HTTPS / REST API
                            │
┌─────────────────────────────────────────────────────────────┐
│                      API Gateway Layer                       │
│                   (Load Balancer / Nginx)                    │
└─────────────────────────────────────────────────────────────┘
                            │
┌─────────────────────────────────────────────────────────────┐
│                     Application Layer                        │
│  ┌──────────────────────────────────────────────────────┐   │
│  │         Backend API Server (Node.js/Express)         │   │
│  │  ┌────────────┐  ┌────────────┐  ┌────────────┐    │   │
│  │  │   Auth     │  │   User     │  │   Scheme   │    │   │
│  │  │  Service   │  │  Service   │  │  Service   │    │   │
│  │  └────────────┘  └────────────┘  └────────────┘    │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
                            │
                ┌───────────┴───────────┐
                │                       │
┌───────────────▼──────────┐  ┌────────▼──────────────────────┐
│    ML Engine Layer       │  │     Database Layer            │
│  ┌────────────────────┐  │  │  ┌──────────────────────┐    │
│  │  Python ML Service │  │  │  │  MongoDB/PostgreSQL  │    │
│  │  ┌──────────────┐  │  │  │  │  ┌────────────────┐  │    │
│  │  │ Prediction   │  │  │  │  │  │ Users          │  │    │
│  │  │ Engine       │  │  │  │  │  │ Schemes        │  │    │
│  │  └──────────────┘  │  │  │  │  │ Bookmarks      │  │    │
│  │  ┌──────────────┐  │  │  │  │  │ Admin          │  │    │
│  │  │ Model        │  │  │  │  │  └────────────────┘  │    │
│  │  │ Training     │  │  │  │  └──────────────────────┘    │
│  │  └──────────────┘  │  │  │                               │
│  └────────────────────┘  │  └───────────────────────────────┘
└──────────────────────────┘
```

### Architecture Layers

1. **Client Layer**: User-facing interfaces (Web, Admin, Mobile)
2. **API Gateway**: Load balancing, rate limiting, SSL termination
3. **Application Layer**: Business logic and API endpoints
4. **ML Engine Layer**: Machine learning prediction service
5. **Database Layer**: Data persistence and retrieval


## 3. Detailed Component Design

### 3.1 Client Layer Components

#### 3.1.1 Web Application (React.js)

**Components:**
- Authentication Module (Login, Register, Password Reset)
- User Dashboard (Recommendations, Profile, Bookmarks)
- Scheme Browser (Search, Filter, Details)
- Profile Management (Form, Validation)
- Bookmark Manager

**State Management:**
- Redux/Context API for global state
- User authentication state
- Scheme data cache
- UI state (loading, errors)

**Routing:**
```
/                    → Landing Page
/login               → Login Page
/register            → Registration Page
/dashboard           → User Dashboard
/profile             → Profile Management
/schemes             → Browse All Schemes
/schemes/:id         → Scheme Details
/bookmarks           → Saved Schemes
```

#### 3.1.2 Admin Panel (React.js)

**Components:**
- Admin Authentication
- Scheme Management (CRUD operations)
- Eligibility Criteria Editor
- Bulk Upload Interface
- Analytics Dashboard
- User Management

**Admin Routing:**
```
/admin/login         → Admin Login
/admin/dashboard     → Admin Dashboard
/admin/schemes       → Manage Schemes
/admin/schemes/new   → Add New Scheme
/admin/schemes/:id   → Edit Scheme
/admin/analytics     → Usage Analytics
```

### 3.2 Application Layer Components

#### 3.2.1 Authentication Service

**Responsibilities:**
- User registration and login
- JWT token generation and validation
- Password hashing (bcrypt)
- Session management
- Role-based access control

**Key Functions:**
```javascript
registerUser(email, password, phone)
loginUser(email, password)
verifyToken(token)
resetPassword(email)
refreshToken(token)
```

#### 3.2.2 User Service

**Responsibilities:**
- User profile CRUD operations
- Profile validation
- User data retrieval
- Account deletion

**Key Functions:**
```javascript
createProfile(userId, profileData)
updateProfile(userId, profileData)
getProfile(userId)
deleteAccount(userId)
validateProfileData(data)
```

#### 3.2.3 Scheme Service

**Responsibilities:**
- Scheme CRUD operations
- Scheme search and filtering
- Bookmark management
- Scheme data validation

**Key Functions:**
```javascript
getAllSchemes(filters)
getSchemeById(schemeId)
searchSchemes(query)
addScheme(schemeData)
updateScheme(schemeId, data)
deleteScheme(schemeId)
bookmarkScheme(userId, schemeId)
getBookmarks(userId)
```

#### 3.2.4 Recommendation Service

**Responsibilities:**
- Interface with ML engine
- Process user profile for prediction
- Rank and filter recommendations
- Cache prediction results

**Key Functions:**
```javascript
getRecommendations(userId)
predictEligibility(profileData, schemes)
rankSchemes(predictions)
cacheResults(userId, recommendations)
```

#### 3.2.5 Admin Service

**Responsibilities:**
- Admin authentication
- Bulk scheme upload
- Analytics data aggregation
- System monitoring

**Key Functions:**
```javascript
bulkUploadSchemes(csvData)
getAnalytics(dateRange)
getUserStatistics()
getPopularSchemes()
exportReport(type)
```

### 3.3 ML Engine Layer

#### 3.3.1 Prediction Engine (Python)

**Architecture:**
```python
class SchemePredictor:
    def __init__(self):
        self.model = load_model()
        self.preprocessor = DataPreprocessor()
    
    def predict_eligibility(self, user_profile, schemes):
        # Preprocess user data
        # Generate predictions for each scheme
        # Return probability scores
        pass
    
    def rank_schemes(self, predictions):
        # Sort by probability score
        # Apply business rules
        # Return ranked list
        pass
```

**Components:**
- Model Loader: Loads trained ML model
- Data Preprocessor: Transforms user input
- Feature Engineer: Creates ML features
- Predictor: Generates eligibility scores
- Ranker: Sorts schemes by relevance

**ML Pipeline:**
```
User Profile → Feature Engineering → Model Prediction → Ranking → Results
```

**Features Used:**
- Age (numerical)
- Gender (categorical)
- Income (numerical, binned)
- State (categorical, one-hot encoded)
- Location Type (binary: rural/urban)
- Occupation (categorical)
- Education Level (ordinal)
- Caste Category (categorical)
- Disability Status (binary)

**Model Type:**
- Random Forest Classifier OR
- Gradient Boosting Classifier OR
- Logistic Regression (baseline)

**Model Output:**
- Eligibility probability (0-1)
- Confidence score
- Feature importance

#### 3.3.2 Model Training Pipeline

**Training Process:**
```python
1. Data Collection
   - Gather historical scheme data
   - Collect eligibility criteria
   - Create training dataset

2. Data Preprocessing
   - Handle missing values
   - Encode categorical variables
   - Normalize numerical features
   - Split train/test sets

3. Model Training
   - Train multiple models
   - Hyperparameter tuning
   - Cross-validation
   - Select best model

4. Model Evaluation
   - Accuracy, Precision, Recall
   - F1 Score
   - Confusion Matrix
   - Feature importance analysis

5. Model Deployment
   - Serialize model (Joblib)
   - Version control
   - Deploy to production
```

**Retraining Strategy:**
- Periodic retraining (monthly/quarterly)
- Triggered by new scheme additions
- Performance monitoring
- A/B testing for model updates


## 4. Database Design

### 4.1 Database Schema (MongoDB)

#### 4.1.1 Users Collection

```javascript
{
  _id: ObjectId,
  email: String (unique, required),
  phone: String (unique),
  password: String (hashed, required),
  role: String (enum: ['user', 'admin'], default: 'user'),
  profile: {
    age: Number,
    gender: String (enum: ['Male', 'Female', 'Other']),
    annualIncome: Number,
    state: String,
    locationType: String (enum: ['Rural', 'Urban']),
    occupation: String,
    educationLevel: String,
    casteCategory: String (enum: ['General', 'OBC', 'SC', 'ST']),
    hasDisability: Boolean,
    disabilityType: String (optional)
  },
  bookmarks: [ObjectId] (references Schemes),
  createdAt: Date,
  updatedAt: Date,
  lastLogin: Date
}
```

**Indexes:**
- email (unique)
- phone (unique)
- role

#### 4.1.2 Schemes Collection

```javascript
{
  _id: ObjectId,
  name: String (required),
  description: String (required),
  category: String (enum: ['Education', 'Health', 'Agriculture', 'Employment', 'Housing', 'Social Welfare', 'Other']),
  schemeType: String (enum: ['Central', 'State']),
  state: String (null for Central schemes),
  benefits: [String],
  eligibilityCriteria: {
    minAge: Number,
    maxAge: Number,
    gender: [String],
    maxIncome: Number,
    states: [String],
    locationType: [String],
    occupations: [String],
    educationLevels: [String],
    casteCategories: [String],
    disabilityRequired: Boolean
  },
  requiredDocuments: [String],
  applicationLink: String (URL),
  officialWebsite: String (URL),
  contactInfo: {
    phone: String,
    email: String,
    address: String
  },
  isActive: Boolean (default: true),
  createdBy: ObjectId (references Users),
  createdAt: Date,
  updatedAt: Date
}
```

**Indexes:**
- name (text index for search)
- category
- schemeType
- state
- isActive

#### 4.1.3 Predictions Collection (Cache)

```javascript
{
  _id: ObjectId,
  userId: ObjectId (references Users, required),
  predictions: [
    {
      schemeId: ObjectId (references Schemes),
      eligibilityScore: Number (0-1),
      confidence: Number,
      rank: Number
    }
  ],
  profileSnapshot: Object (user profile at prediction time),
  createdAt: Date,
  expiresAt: Date (TTL index)
}
```

**Indexes:**
- userId
- createdAt (TTL index for auto-deletion)

#### 4.1.4 Analytics Collection

```javascript
{
  _id: ObjectId,
  eventType: String (enum: ['login', 'search', 'view_scheme', 'bookmark', 'prediction']),
  userId: ObjectId (references Users),
  schemeId: ObjectId (references Schemes, optional),
  metadata: Object,
  timestamp: Date
}
```

**Indexes:**
- eventType
- userId
- timestamp

### 4.2 Database Schema (PostgreSQL Alternative)

#### Users Table
```sql
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  email VARCHAR(255) UNIQUE NOT NULL,
  phone VARCHAR(20) UNIQUE,
  password VARCHAR(255) NOT NULL,
  role VARCHAR(20) DEFAULT 'user',
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  last_login TIMESTAMP
);
```

#### User_Profiles Table
```sql
CREATE TABLE user_profiles (
  id SERIAL PRIMARY KEY,
  user_id INTEGER REFERENCES users(id) ON DELETE CASCADE,
  age INTEGER,
  gender VARCHAR(20),
  annual_income DECIMAL(12, 2),
  state VARCHAR(100),
  location_type VARCHAR(20),
  occupation VARCHAR(100),
  education_level VARCHAR(100),
  caste_category VARCHAR(20),
  has_disability BOOLEAN DEFAULT FALSE,
  disability_type VARCHAR(100),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

#### Schemes Table
```sql
CREATE TABLE schemes (
  id SERIAL PRIMARY KEY,
  name VARCHAR(255) NOT NULL,
  description TEXT NOT NULL,
  category VARCHAR(100),
  scheme_type VARCHAR(20),
  state VARCHAR(100),
  benefits TEXT[],
  required_documents TEXT[],
  application_link VARCHAR(500),
  official_website VARCHAR(500),
  is_active BOOLEAN DEFAULT TRUE,
  created_by INTEGER REFERENCES users(id),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

#### Eligibility_Criteria Table
```sql
CREATE TABLE eligibility_criteria (
  id SERIAL PRIMARY KEY,
  scheme_id INTEGER REFERENCES schemes(id) ON DELETE CASCADE,
  min_age INTEGER,
  max_age INTEGER,
  gender VARCHAR(50)[],
  max_income DECIMAL(12, 2),
  states VARCHAR(100)[],
  location_type VARCHAR(20)[],
  occupations VARCHAR(100)[],
  education_levels VARCHAR(100)[],
  caste_categories VARCHAR(20)[],
  disability_required BOOLEAN DEFAULT FALSE
);
```

#### Bookmarks Table
```sql
CREATE TABLE bookmarks (
  id SERIAL PRIMARY KEY,
  user_id INTEGER REFERENCES users(id) ON DELETE CASCADE,
  scheme_id INTEGER REFERENCES schemes(id) ON DELETE CASCADE,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  UNIQUE(user_id, scheme_id)
);
```

### 4.3 Data Relationships

```
Users (1) ──── (1) User_Profiles
Users (1) ──── (N) Bookmarks
Schemes (1) ──── (N) Bookmarks
Schemes (1) ──── (1) Eligibility_Criteria
Users (1) ──── (N) Predictions
Schemes (1) ──── (N) Predictions
```



## 5. API Design

### 5.1 RESTful API Endpoints

#### 5.1.1 Authentication APIs

**POST /api/auth/register**
```json
Request:
{
  "email": "user@example.com",
  "phone": "9876543210",
  "password": "SecurePass123"
}

Response (201):
{
  "success": true,
  "message": "User registered successfully",
  "userId": "64a1b2c3d4e5f6g7h8i9j0k1"
}
```

**POST /api/auth/login**
```json
Request:
{
  "email": "user@example.com",
  "password": "SecurePass123"
}

Response (200):
{
  "success": true,
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": "64a1b2c3d4e5f6g7h8i9j0k1",
    "email": "user@example.com",
    "role": "user"
  }
}
```

**POST /api/auth/logout**
```json
Headers: Authorization: Bearer <token>

Response (200):
{
  "success": true,
  "message": "Logged out successfully"
}
```

**POST /api/auth/reset-password**
```json
Request:
{
  "email": "user@example.com"
}

Response (200):
{
  "success": true,
  "message": "Password reset link sent to email"
}
```

#### 5.1.2 User Profile APIs

**GET /api/users/profile**
```json
Headers: Authorization: Bearer <token>

Response (200):
{
  "success": true,
  "profile": {
    "age": 28,
    "gender": "Male",
    "annualIncome": 350000,
    "state": "Karnataka",
    "locationType": "Urban",
    "occupation": "Software Engineer",
    "educationLevel": "Graduate",
    "casteCategory": "General",
    "hasDisability": false
  }
}
```

**PUT /api/users/profile**
```json
Headers: Authorization: Bearer <token>

Request:
{
  "age": 28,
  "gender": "Male",
  "annualIncome": 350000,
  "state": "Karnataka",
  "locationType": "Urban",
  "occupation": "Software Engineer",
  "educationLevel": "Graduate",
  "casteCategory": "General",
  "hasDisability": false
}

Response (200):
{
  "success": true,
  "message": "Profile updated successfully"
}
```

**DELETE /api/users/account**
```json
Headers: Authorization: Bearer <token>

Response (200):
{
  "success": true,
  "message": "Account deleted successfully"
}
```

#### 5.1.3 Scheme APIs

**GET /api/schemes**
```json
Query Parameters:
- page: number (default: 1)
- limit: number (default: 20)
- category: string (optional)
- state: string (optional)
- schemeType: string (optional)
- search: string (optional)

Response (200):
{
  "success": true,
  "schemes": [
    {
      "id": "64a1b2c3d4e5f6g7h8i9j0k1",
      "name": "Pradhan Mantri Awas Yojana",
      "description": "Housing scheme for economically weaker sections",
      "category": "Housing",
      "schemeType": "Central",
      "benefits": ["Financial assistance for house construction"],
      "applicationLink": "https://pmaymis.gov.in/"
    }
  ],
  "pagination": {
    "currentPage": 1,
    "totalPages": 10,
    "totalSchemes": 200
  }
}
```

**GET /api/schemes/:id**
```json
Response (200):
{
  "success": true,
  "scheme": {
    "id": "64a1b2c3d4e5f6g7h8i9j0k1",
    "name": "Pradhan Mantri Awas Yojana",
    "description": "Housing scheme for economically weaker sections",
    "category": "Housing",
    "schemeType": "Central",
    "benefits": ["Financial assistance for house construction"],
    "eligibilityCriteria": {
      "minAge": 18,
      "maxAge": 70,
      "maxIncome": 600000,
      "locationType": ["Rural", "Urban"]
    },
    "requiredDocuments": ["Aadhaar Card", "Income Certificate", "Bank Account"],
    "applicationLink": "https://pmaymis.gov.in/",
    "officialWebsite": "https://pmaymis.gov.in/"
  }
}
```

**GET /api/schemes/search**
```json
Query Parameters:
- q: string (search query)

Response (200):
{
  "success": true,
  "results": [
    {
      "id": "64a1b2c3d4e5f6g7h8i9j0k1",
      "name": "Pradhan Mantri Awas Yojana",
      "description": "Housing scheme...",
      "category": "Housing"
    }
  ]
}
```

#### 5.1.4 Recommendation APIs

**GET /api/recommendations**
```json
Headers: Authorization: Bearer <token>

Response (200):
{
  "success": true,
  "recommendations": [
    {
      "scheme": {
        "id": "64a1b2c3d4e5f6g7h8i9j0k1",
        "name": "Pradhan Mantri Awas Yojana",
        "description": "Housing scheme...",
        "category": "Housing"
      },
      "eligibilityScore": 0.92,
      "confidence": 0.88,
      "rank": 1,
      "matchedCriteria": ["Age", "Income", "Location"]
    }
  ],
  "totalRecommendations": 15
}
```

**POST /api/recommendations/predict**
```json
Headers: Authorization: Bearer <token>

Request:
{
  "profileData": {
    "age": 28,
    "gender": "Male",
    "annualIncome": 350000,
    "state": "Karnataka",
    "locationType": "Urban",
    "occupation": "Software Engineer",
    "educationLevel": "Graduate",
    "casteCategory": "General",
    "hasDisability": false
  }
}

Response (200):
{
  "success": true,
  "predictions": [...]
}
```

#### 5.1.5 Bookmark APIs

**POST /api/bookmarks**
```json
Headers: Authorization: Bearer <token>

Request:
{
  "schemeId": "64a1b2c3d4e5f6g7h8i9j0k1"
}

Response (201):
{
  "success": true,
  "message": "Scheme bookmarked successfully"
}
```

**GET /api/bookmarks**
```json
Headers: Authorization: Bearer <token>

Response (200):
{
  "success": true,
  "bookmarks": [
    {
      "id": "64a1b2c3d4e5f6g7h8i9j0k1",
      "name": "Pradhan Mantri Awas Yojana",
      "category": "Housing",
      "bookmarkedAt": "2024-01-15T10:30:00Z"
    }
  ]
}
```

**DELETE /api/bookmarks/:schemeId**
```json
Headers: Authorization: Bearer <token>

Response (200):
{
  "success": true,
  "message": "Bookmark removed successfully"
}
```

#### 5.1.6 Admin APIs

**POST /api/admin/schemes**
```json
Headers: Authorization: Bearer <admin-token>

Request:
{
  "name": "New Scheme",
  "description": "Scheme description",
  "category": "Education",
  "schemeType": "Central",
  "benefits": ["Benefit 1", "Benefit 2"],
  "eligibilityCriteria": {...},
  "requiredDocuments": ["Doc 1", "Doc 2"],
  "applicationLink": "https://example.com"
}

Response (201):
{
  "success": true,
  "message": "Scheme added successfully",
  "schemeId": "64a1b2c3d4e5f6g7h8i9j0k1"
}
```

**PUT /api/admin/schemes/:id**
```json
Headers: Authorization: Bearer <admin-token>

Request:
{
  "name": "Updated Scheme Name",
  "description": "Updated description"
}

Response (200):
{
  "success": true,
  "message": "Scheme updated successfully"
}
```

**DELETE /api/admin/schemes/:id**
```json
Headers: Authorization: Bearer <admin-token>

Response (200):
{
  "success": true,
  "message": "Scheme deleted successfully"
}
```

**POST /api/admin/schemes/bulk-upload**
```json
Headers: Authorization: Bearer <admin-token>
Content-Type: multipart/form-data

Request:
{
  "file": <CSV/JSON file>
}

Response (200):
{
  "success": true,
  "message": "Bulk upload completed",
  "imported": 150,
  "failed": 5,
  "errors": [...]
}
```

**GET /api/admin/analytics**
```json
Headers: Authorization: Bearer <admin-token>

Query Parameters:
- startDate: date
- endDate: date

Response (200):
{
  "success": true,
  "analytics": {
    "totalUsers": 10000,
    "activeUsers": 3500,
    "totalSchemes": 500,
    "popularSchemes": [...],
    "searchTrends": [...],
    "userGrowth": [...]
  }
}
```

### 5.2 ML Service API

**POST /ml/predict**
```json
Request:
{
  "userProfile": {
    "age": 28,
    "gender": "Male",
    "annualIncome": 350000,
    "state": "Karnataka",
    "locationType": "Urban",
    "occupation": "Software Engineer",
    "educationLevel": "Graduate",
    "casteCategory": "General",
    "hasDisability": false
  },
  "schemes": [
    {
      "id": "scheme1",
      "eligibilityCriteria": {...}
    }
  ]
}

Response (200):
{
  "predictions": [
    {
      "schemeId": "scheme1",
      "eligibilityScore": 0.92,
      "confidence": 0.88,
      "features": {...}
    }
  ],
  "processingTime": 1.2
}
```

### 5.3 API Error Responses

**400 Bad Request**
```json
{
  "success": false,
  "error": "Validation error",
  "details": {
    "field": "email",
    "message": "Invalid email format"
  }
}
```

**401 Unauthorized**
```json
{
  "success": false,
  "error": "Authentication required",
  "message": "Please login to access this resource"
}
```

**403 Forbidden**
```json
{
  "success": false,
  "error": "Access denied",
  "message": "You don't have permission to access this resource"
}
```

**404 Not Found**
```json
{
  "success": false,
  "error": "Resource not found",
  "message": "The requested scheme does not exist"
}
```

**500 Internal Server Error**
```json
{
  "success": false,
  "error": "Internal server error",
  "message": "An unexpected error occurred"
}
```


## 6. Security Architecture

### 6.1 Authentication & Authorization

#### JWT-Based Authentication Flow

```
1. User Login
   ↓
2. Server validates credentials
   ↓
3. Server generates JWT token
   - Header: Algorithm & Token Type
   - Payload: User ID, Role, Expiry
   - Signature: Secret Key
   ↓
4. Client stores token (localStorage/sessionStorage)
   ↓
5. Client includes token in Authorization header
   ↓
6. Server validates token on each request
   ↓
7. Server grants/denies access
```

**JWT Token Structure:**
```javascript
{
  "header": {
    "alg": "HS256",
    "typ": "JWT"
  },
  "payload": {
    "userId": "64a1b2c3d4e5f6g7h8i9j0k1",
    "email": "user@example.com",
    "role": "user",
    "iat": 1640000000,
    "exp": 1640086400
  },
  "signature": "..."
}
```

**Token Expiry:**
- Access Token: 24 hours
- Refresh Token: 7 days (future enhancement)

#### Role-Based Access Control (RBAC)

**Roles:**
- User: Access to user features
- Admin: Full access to admin panel

**Permissions Matrix:**
```
Feature                  | User | Admin
-------------------------|------|-------
View Schemes             |  ✓   |   ✓
Get Recommendations      |  ✓   |   ✓
Bookmark Schemes         |  ✓   |   ✓
Add/Edit/Delete Schemes  |  ✗   |   ✓
Bulk Upload              |  ✗   |   ✓
View Analytics           |  ✗   |   ✓
Manage Users             |  ✗   |   ✓
```

### 6.2 Data Security

#### Password Security
- Hashing Algorithm: bcrypt with salt rounds = 10
- Password Requirements:
  - Minimum 8 characters
  - At least 1 uppercase letter
  - At least 1 lowercase letter
  - At least 1 number
  - At least 1 special character

#### Data Encryption
- Data in Transit: TLS 1.3 (HTTPS)
- Data at Rest: Database-level encryption
- Sensitive Fields: Additional encryption for PII

#### Input Validation & Sanitization
```javascript
// Example validation middleware
const validateProfile = (req, res, next) => {
  const schema = Joi.object({
    age: Joi.number().min(0).max(120).required(),
    gender: Joi.string().valid('Male', 'Female', 'Other').required(),
    annualIncome: Joi.number().min(0).required(),
    state: Joi.string().required(),
    // ... other validations
  });
  
  const { error } = schema.validate(req.body);
  if (error) {
    return res.status(400).json({ error: error.details[0].message });
  }
  next();
};
```

#### SQL Injection Prevention
- Use parameterized queries
- ORM/ODM (Mongoose/Sequelize)
- Input validation

#### XSS Prevention
- Sanitize user input
- Content Security Policy headers
- Escape output in templates

### 6.3 API Security

#### Rate Limiting
```javascript
// Express rate limiter
const rateLimit = require('express-rate-limit');

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // limit each IP to 100 requests per windowMs
  message: 'Too many requests, please try again later'
});

app.use('/api/', limiter);
```

#### CORS Configuration
```javascript
const cors = require('cors');

app.use(cors({
  origin: ['https://schemeindia.com'],
  methods: ['GET', 'POST', 'PUT', 'DELETE'],
  allowedHeaders: ['Content-Type', 'Authorization'],
  credentials: true
}));
```

#### Security Headers
```javascript
const helmet = require('helmet');

app.use(helmet({
  contentSecurityPolicy: {
    directives: {
      defaultSrc: ["'self'"],
      styleSrc: ["'self'", "'unsafe-inline'"],
      scriptSrc: ["'self'"],
      imgSrc: ["'self'", "data:", "https:"],
    },
  },
  hsts: {
    maxAge: 31536000,
    includeSubDomains: true,
    preload: true
  }
}));
```

### 6.4 Audit Logging

**Log Events:**
- User login/logout
- Profile updates
- Scheme access
- Admin actions
- Failed authentication attempts
- API errors

**Log Structure:**
```javascript
{
  timestamp: "2024-01-15T10:30:00Z",
  level: "info",
  event: "user_login",
  userId: "64a1b2c3d4e5f6g7h8i9j0k1",
  ip: "192.168.1.1",
  userAgent: "Mozilla/5.0...",
  metadata: {...}
}
```

### 6.5 Data Privacy Compliance

**GDPR/Data Protection Principles:**
- User consent for data collection
- Right to access personal data
- Right to delete account and data
- Data minimization
- Purpose limitation
- Transparent privacy policy

**Privacy Features:**
- Account deletion functionality
- Data export capability
- Privacy policy acceptance
- Cookie consent management


## 7. Data Flow Diagrams

### 7.1 User Registration Flow

```
User → Frontend → API Gateway → Backend
                                    ↓
                              Validate Input
                                    ↓
                              Hash Password
                                    ↓
                              Save to Database
                                    ↓
                              Generate JWT Token
                                    ↓
Frontend ← API Gateway ← Backend (Token + User Data)
    ↓
Store Token
    ↓
Redirect to Dashboard
```

### 7.2 Scheme Recommendation Flow

```
User Profile Input
    ↓
Frontend (React)
    ↓
API Request (POST /api/recommendations)
    ↓
Backend API Server
    ↓
┌─────────────────────────────────┐
│ 1. Fetch User Profile           │
│ 2. Fetch All Active Schemes     │
└─────────────────────────────────┘
    ↓
ML Service API (POST /ml/predict)
    ↓
┌─────────────────────────────────┐
│ ML Prediction Engine            │
│ 1. Preprocess User Data         │
│ 2. Feature Engineering          │
│ 3. Model Prediction             │
│ 4. Generate Scores              │
└─────────────────────────────────┘
    ↓
Prediction Results (Scores)
    ↓
Backend API Server
    ↓
┌─────────────────────────────────┐
│ 1. Rank Schemes by Score        │
│ 2. Filter Top N Schemes         │
│ 3. Enrich with Scheme Details   │
│ 4. Cache Results                │
└─────────────────────────────────┘
    ↓
API Response (Ranked Schemes)
    ↓
Frontend (React)
    ↓
Display Recommendations to User
```

### 7.3 Admin Scheme Management Flow

```
Admin Login
    ↓
Admin Panel (React)
    ↓
Add/Edit Scheme Form
    ↓
API Request (POST/PUT /api/admin/schemes)
    ↓
Backend API Server
    ↓
┌─────────────────────────────────┐
│ 1. Verify Admin Token           │
│ 2. Validate Scheme Data         │
│ 3. Check Eligibility Rules      │
└─────────────────────────────────┘
    ↓
Save to Database
    ↓
┌─────────────────────────────────┐
│ 1. Update Scheme Collection     │
│ 2. Invalidate Prediction Cache  │
│ 3. Log Admin Action             │
└─────────────────────────────────┘
    ↓
API Response (Success)
    ↓
Admin Panel (React)
    ↓
Display Success Message
    ↓
Refresh Scheme List
```

### 7.4 Bookmark Flow

```
User Views Scheme
    ↓
Click Bookmark Button
    ↓
Frontend (React)
    ↓
API Request (POST /api/bookmarks)
    ↓
Backend API Server
    ↓
┌─────────────────────────────────┐
│ 1. Verify User Token            │
│ 2. Check if Already Bookmarked  │
│ 3. Validate Scheme Exists       │
└─────────────────────────────────┘
    ↓
Update Database
    ↓
┌─────────────────────────────────┐
│ Add Bookmark Record             │
│ (userId + schemeId)             │
└─────────────────────────────────┘
    ↓
API Response (Success)
    ↓
Frontend (React)
    ↓
Update UI (Show Bookmarked State)
```

## 8. Deployment Architecture

### 8.1 Cloud Infrastructure (AWS Example)

```
                    ┌─────────────────────┐
                    │   Route 53 (DNS)    │
                    └──────────┬──────────┘
                               │
                    ┌──────────▼──────────┐
                    │  CloudFront (CDN)   │
                    │  SSL Certificate    │
                    └──────────┬──────────┘
                               │
                    ┌──────────▼──────────┐
                    │  Application Load   │
                    │     Balancer        │
                    └──────────┬──────────┘
                               │
            ┌──────────────────┼──────────────────┐
            │                  │                  │
    ┌───────▼────────┐ ┌──────▼───────┐ ┌───────▼────────┐
    │   EC2 Instance │ │ EC2 Instance │ │  EC2 Instance  │
    │   (Backend)    │ │  (Backend)   │ │   (Backend)    │
    │   Auto Scaling │ │              │ │                │
    └───────┬────────┘ └──────┬───────┘ └───────┬────────┘
            │                  │                  │
            └──────────────────┼──────────────────┘
                               │
            ┌──────────────────┼──────────────────┐
            │                  │                  │
    ┌───────▼────────┐ ┌──────▼───────┐ ┌───────▼────────┐
    │   RDS/MongoDB  │ │  EC2 (ML)    │ │   S3 Bucket    │
    │   (Database)   │ │  Service     │ │  (Static Files)│
    │   Multi-AZ     │ │              │ │                │
    └────────────────┘ └──────────────┘ └────────────────┘
```

### 8.2 Deployment Components

#### Frontend Deployment
- **Hosting**: AWS S3 + CloudFront OR Vercel/Netlify
- **Build**: React production build
- **CDN**: CloudFront for global distribution
- **SSL**: AWS Certificate Manager

#### Backend Deployment
- **Compute**: AWS EC2 (t3.medium) OR AWS ECS/Fargate
- **Auto Scaling**: Based on CPU/Memory usage
- **Load Balancer**: Application Load Balancer
- **Container**: Docker containers

#### ML Service Deployment
- **Compute**: AWS EC2 (c5.xlarge) OR AWS SageMaker
- **Model Storage**: S3 bucket
- **API**: Flask/FastAPI service
- **Scaling**: Horizontal scaling based on load

#### Database Deployment
- **MongoDB**: MongoDB Atlas (Managed) OR EC2 self-hosted
- **PostgreSQL**: AWS RDS (Multi-AZ)
- **Backup**: Automated daily backups
- **Replication**: Read replicas for scaling

#### Caching Layer
- **Redis/ElastiCache**: For prediction caching
- **TTL**: 24 hours for predictions

### 8.3 CI/CD Pipeline

```
Developer Push Code
    ↓
GitHub Repository
    ↓
GitHub Actions / Jenkins
    ↓
┌─────────────────────────────────┐
│ 1. Run Tests                    │
│ 2. Code Quality Checks          │
│ 3. Security Scanning            │
└─────────────────────────────────┘
    ↓
Build Docker Images
    ↓
Push to Container Registry (ECR)
    ↓
Deploy to Staging Environment
    ↓
Run Integration Tests
    ↓
Manual Approval (Production)
    ↓
Deploy to Production
    ↓
Health Checks & Monitoring
```

### 8.4 Monitoring & Logging

**Monitoring Tools:**
- AWS CloudWatch for infrastructure metrics
- Application Performance Monitoring (APM): New Relic/Datadog
- Uptime monitoring: Pingdom/UptimeRobot

**Metrics to Monitor:**
- API response times
- ML prediction latency
- Database query performance
- Error rates
- CPU/Memory usage
- Request throughput

**Logging:**
- Centralized logging: AWS CloudWatch Logs OR ELK Stack
- Log levels: ERROR, WARN, INFO, DEBUG
- Log retention: 30 days

**Alerting:**
- High error rates
- Slow API responses
- Database connection issues
- High CPU/Memory usage
- ML service failures

### 8.5 Backup & Disaster Recovery

**Backup Strategy:**
- Database: Automated daily backups with 7-day retention
- ML Models: Versioned in S3 with lifecycle policies
- Configuration: Version controlled in Git

**Disaster Recovery:**
- RTO (Recovery Time Objective): 4 hours
- RPO (Recovery Point Objective): 24 hours
- Multi-region deployment (future)
- Database replication across availability zones

### 8.6 Scalability Strategy

**Horizontal Scaling:**
- Backend API: Auto-scaling group (2-10 instances)
- ML Service: Auto-scaling based on queue depth
- Database: Read replicas for read-heavy operations

**Vertical Scaling:**
- Upgrade instance types during peak usage
- Database instance scaling

**Caching Strategy:**
- Redis for prediction results (24-hour TTL)
- CDN for static assets
- API response caching for public endpoints

**Load Balancing:**
- Round-robin distribution
- Health checks every 30 seconds
- Automatic removal of unhealthy instances


## 9. Performance Optimization

### 9.1 Frontend Optimization

**Code Splitting:**
```javascript
// Lazy loading routes
const Dashboard = lazy(() => import('./pages/Dashboard'));
const SchemeDetails = lazy(() => import('./pages/SchemeDetails'));

<Suspense fallback={<Loading />}>
  <Routes>
    <Route path="/dashboard" element={<Dashboard />} />
    <Route path="/schemes/:id" element={<SchemeDetails />} />
  </Routes>
</Suspense>
```

**Optimization Techniques:**
- Code splitting by route
- Image lazy loading
- Minification and compression (Gzip/Brotli)
- Tree shaking to remove unused code
- Service workers for offline capability
- Debouncing search inputs
- Virtual scrolling for long lists

**Caching Strategy:**
- Browser caching for static assets
- Service worker caching
- LocalStorage for user preferences

### 9.2 Backend Optimization

**Database Optimization:**
- Indexing on frequently queried fields
- Query optimization and explain plans
- Connection pooling
- Pagination for large result sets
- Aggregation pipelines for complex queries

**API Optimization:**
- Response compression (Gzip)
- Pagination for list endpoints
- Field selection (return only required fields)
- Batch operations for bulk requests
- Async processing for heavy operations

**Caching Strategy:**
```javascript
// Redis caching example
const getCachedRecommendations = async (userId) => {
  const cacheKey = `recommendations:${userId}`;
  
  // Check cache first
  const cached = await redis.get(cacheKey);
  if (cached) {
    return JSON.parse(cached);
  }
  
  // If not cached, fetch from ML service
  const recommendations = await mlService.predict(userId);
  
  // Cache for 24 hours
  await redis.setex(cacheKey, 86400, JSON.stringify(recommendations));
  
  return recommendations;
};
```

### 9.3 ML Service Optimization

**Model Optimization:**
- Model quantization for faster inference
- Feature caching for repeated predictions
- Batch prediction for multiple users
- Model serving with TensorFlow Serving or ONNX Runtime

**Prediction Caching:**
- Cache predictions for 24 hours
- Invalidate cache on profile update
- Pre-compute predictions for common profiles

**Async Processing:**
```python
# Async prediction using Celery
@celery.task
def predict_eligibility_async(user_id):
    user_profile = get_user_profile(user_id)
    schemes = get_all_schemes()
    predictions = model.predict(user_profile, schemes)
    cache_predictions(user_id, predictions)
    return predictions
```

### 9.4 Database Query Optimization

**MongoDB Indexes:**
```javascript
// Create indexes for performance
db.users.createIndex({ email: 1 }, { unique: true });
db.schemes.createIndex({ name: "text", description: "text" });
db.schemes.createIndex({ category: 1, state: 1 });
db.schemes.createIndex({ isActive: 1 });
db.bookmarks.createIndex({ userId: 1, schemeId: 1 }, { unique: true });
```

**Query Optimization:**
```javascript
// Bad: Fetching all fields
const schemes = await Scheme.find({ isActive: true });

// Good: Select only required fields
const schemes = await Scheme.find({ isActive: true })
  .select('name description category benefits')
  .limit(20);
```

**Aggregation Pipeline:**
```javascript
// Efficient aggregation for analytics
db.analytics.aggregate([
  { $match: { eventType: "view_scheme", timestamp: { $gte: startDate } } },
  { $group: { _id: "$schemeId", count: { $sum: 1 } } },
  { $sort: { count: -1 } },
  { $limit: 10 }
]);
```

## 10. Error Handling & Resilience

### 10.1 Error Handling Strategy

**Frontend Error Handling:**
```javascript
// Global error boundary
class ErrorBoundary extends React.Component {
  componentDidCatch(error, errorInfo) {
    logErrorToService(error, errorInfo);
    this.setState({ hasError: true });
  }
  
  render() {
    if (this.state.hasError) {
      return <ErrorPage />;
    }
    return this.props.children;
  }
}

// API error handling
const fetchRecommendations = async () => {
  try {
    const response = await api.get('/recommendations');
    return response.data;
  } catch (error) {
    if (error.response?.status === 401) {
      // Redirect to login
      redirectToLogin();
    } else if (error.response?.status === 500) {
      // Show error message
      showErrorNotification('Server error. Please try again.');
    } else {
      // Generic error
      showErrorNotification('Something went wrong.');
    }
    throw error;
  }
};
```

**Backend Error Handling:**
```javascript
// Global error handler middleware
app.use((err, req, res, next) => {
  // Log error
  logger.error({
    message: err.message,
    stack: err.stack,
    url: req.url,
    method: req.method,
    userId: req.user?.id
  });
  
  // Send appropriate response
  if (err.name === 'ValidationError') {
    return res.status(400).json({
      success: false,
      error: 'Validation error',
      details: err.details
    });
  }
  
  if (err.name === 'UnauthorizedError') {
    return res.status(401).json({
      success: false,
      error: 'Authentication required'
    });
  }
  
  // Generic error
  res.status(500).json({
    success: false,
    error: 'Internal server error'
  });
});
```

### 10.2 Resilience Patterns

**Circuit Breaker:**
```javascript
// Circuit breaker for ML service
const circuitBreaker = new CircuitBreaker(mlService.predict, {
  timeout: 5000,
  errorThresholdPercentage: 50,
  resetTimeout: 30000
});

circuitBreaker.fallback(() => {
  // Return cached predictions or default response
  return getCachedPredictions();
});
```

**Retry Logic:**
```javascript
// Retry failed requests
const retryRequest = async (fn, maxRetries = 3) => {
  for (let i = 0; i < maxRetries; i++) {
    try {
      return await fn();
    } catch (error) {
      if (i === maxRetries - 1) throw error;
      await sleep(1000 * Math.pow(2, i)); // Exponential backoff
    }
  }
};
```

**Graceful Degradation:**
```javascript
// Fallback when ML service is unavailable
const getRecommendations = async (userId) => {
  try {
    return await mlService.predict(userId);
  } catch (error) {
    logger.warn('ML service unavailable, using rule-based fallback');
    return ruleBasedRecommendations(userId);
  }
};
```

## 11. Testing Strategy

### 11.1 Frontend Testing

**Unit Testing (Jest + React Testing Library):**
```javascript
// Component test
test('renders scheme card with correct data', () => {
  const scheme = {
    name: 'Test Scheme',
    description: 'Test description',
    category: 'Education'
  };
  
  render(<SchemeCard scheme={scheme} />);
  
  expect(screen.getByText('Test Scheme')).toBeInTheDocument();
  expect(screen.getByText('Test description')).toBeInTheDocument();
});
```

**Integration Testing:**
- Test API integration
- Test user flows (login, profile creation, recommendations)
- Test state management

**E2E Testing (Cypress/Playwright):**
```javascript
// E2E test
describe('User Registration Flow', () => {
  it('should register a new user successfully', () => {
    cy.visit('/register');
    cy.get('[name="email"]').type('test@example.com');
    cy.get('[name="password"]').type('SecurePass123');
    cy.get('[type="submit"]').click();
    cy.url().should('include', '/dashboard');
  });
});
```

### 11.2 Backend Testing

**Unit Testing (Jest/Mocha):**
```javascript
// Service test
describe('SchemeService', () => {
  test('should return schemes by category', async () => {
    const schemes = await schemeService.getSchemesByCategory('Education');
    expect(schemes).toHaveLength(5);
    expect(schemes[0].category).toBe('Education');
  });
});
```

**Integration Testing:**
```javascript
// API endpoint test
describe('POST /api/auth/login', () => {
  test('should return token on valid credentials', async () => {
    const response = await request(app)
      .post('/api/auth/login')
      .send({ email: 'test@example.com', password: 'password123' });
    
    expect(response.status).toBe(200);
    expect(response.body).toHaveProperty('token');
  });
});
```

**Database Testing:**
- Use test database
- Seed test data
- Clean up after tests

### 11.3 ML Model Testing

**Model Evaluation:**
```python
# Model testing
def test_model_accuracy():
    X_test, y_test = load_test_data()
    predictions = model.predict(X_test)
    accuracy = accuracy_score(y_test, predictions)
    assert accuracy > 0.80, f"Model accuracy {accuracy} below threshold"

def test_prediction_time():
    sample_profile = create_sample_profile()
    start_time = time.time()
    prediction = model.predict(sample_profile)
    end_time = time.time()
    assert (end_time - start_time) < 2.0, "Prediction too slow"
```

**Data Validation Testing:**
- Test feature engineering
- Test data preprocessing
- Test edge cases

### 11.4 Performance Testing

**Load Testing (Artillery/JMeter):**
```yaml
# Artillery config
config:
  target: 'https://api.schemeindia.com'
  phases:
    - duration: 60
      arrivalRate: 10
      name: "Warm up"
    - duration: 120
      arrivalRate: 50
      name: "Sustained load"

scenarios:
  - name: "Get recommendations"
    flow:
      - post:
          url: "/api/auth/login"
          json:
            email: "test@example.com"
            password: "password123"
          capture:
            - json: "$.token"
              as: "token"
      - get:
          url: "/api/recommendations"
          headers:
            Authorization: "Bearer {{ token }}"
```

**Stress Testing:**
- Test system under extreme load
- Identify breaking points
- Test auto-scaling behavior

## 12. Future Enhancements

### Phase 2 Features

**Multilingual Support:**
- i18n implementation (react-i18next)
- Support for Hindi, Telugu, Tamil, Bengali, Marathi
- RTL support for Urdu
- Language detection and switching

**Voice-Based Input:**
- Speech-to-text integration
- Voice commands for navigation
- Accessibility for visually impaired users

**DigiLocker Integration:**
- Document verification
- Auto-fill from DigiLocker
- Secure document storage

**Real-Time Updates:**
- WebSocket for live notifications
- Real-time scheme updates
- Push notifications

**Mobile Application:**
- React Native app
- Offline capability
- Push notifications
- Biometric authentication

**AI Chatbot:**
- NLP-based query handling
- Scheme recommendations via chat
- Multi-language support
- Integration with WhatsApp/Telegram

**Advanced Analytics:**
- User behavior tracking
- Scheme effectiveness metrics
- Predictive analytics for scheme popularity
- Admin dashboard with visualizations

### Technical Improvements

**Microservices Architecture:**
- Split monolith into microservices
- Service mesh (Istio)
- Event-driven architecture
- Message queue (RabbitMQ/Kafka)

**Advanced ML:**
- Deep learning models
- Personalized ranking algorithms
- Collaborative filtering
- Continuous learning from user feedback

**Enhanced Security:**
- Two-factor authentication
- Biometric authentication
- Advanced fraud detection
- Security audits and penetration testing

## 13. Conclusion

This design document provides a comprehensive blueprint for building Scheme India, an AI-powered government scheme recommendation platform. The architecture is designed to be scalable, secure, and maintainable, with clear separation of concerns and modular components.

Key design principles:
- Scalability through horizontal scaling and caching
- Security through JWT authentication, encryption, and input validation
- Performance through optimization at all layers
- Reliability through error handling and resilience patterns
- Maintainability through clean architecture and comprehensive testing

The platform will help millions of Indian citizens discover government schemes they are eligible for, bridging the gap between government initiatives and beneficiaries.
