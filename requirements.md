# Requirements Document
## Scheme India – AI-Powered Government Scheme Eligibility & Recommendation Platform

## Project Overview

Scheme India is a machine learning-powered web platform designed to help Indian citizens easily discover government schemes they are eligible for. The platform addresses the challenge of fragmented government scheme information by providing an intelligent recommendation system that predicts scheme eligibility based on user demographic and socio-economic data.

### Problem Statement
- Government schemes are fragmented across multiple portals
- Eligibility criteria are complex and difficult to interpret
- Citizens lack a centralized, personalized recommendation system
- Many eligible beneficiaries miss opportunities due to lack of awareness

### Solution
An AI-driven web platform that collects user information, processes it through a machine learning model, and recommends eligible government schemes ranked by relevance with complete details and application links.

## Functional Requirements

### FR1: User Authentication & Authorization
**Priority: High**

- FR1.1: Users shall be able to register with email, phone number, and password
- FR1.2: Users shall be able to login using registered credentials
- FR1.3: System shall implement JWT-based authentication
- FR1.4: Users shall be able to reset forgotten passwords
- FR1.5: System shall maintain secure session management
- FR1.6: Admin users shall have separate login with elevated privileges

### FR2: User Profile Management
**Priority: High**

- FR2.1: Users shall input the following personal details:
  - Age
  - Gender
  - Annual income
  - State of residence
  - Rural/Urban location
  - Occupation
  - Education level
  - Caste category (General/OBC/SC/ST)
  - Disability status (if applicable)
- FR2.2: Users shall be able to update their profile information
- FR2.3: System shall validate all input data for correctness
- FR2.4: Users shall be able to view their saved profile

### FR3: AI-Based Scheme Recommendation Engine
**Priority: High**

- FR3.1: System shall process user input through ML model
- FR3.2: ML model shall predict eligibility for each scheme
- FR3.3: System shall generate probability scores for each scheme
- FR3.4: System shall rank schemes by relevance and eligibility score
- FR3.5: Prediction response time shall be under 2 seconds
- FR3.6: System shall display top recommended schemes on user dashboard

### FR4: Scheme Information Display
**Priority: High**

- FR4.1: System shall display for each scheme:
  - Scheme name and description
  - Benefits provided
  - Detailed eligibility criteria
  - Required documents
  - Official application link
  - Scheme category (Central/State)
- FR4.2: Users shall be able to view full scheme details
- FR4.3: System shall provide clear eligibility match indicators

### FR5: Personalized User Dashboard
**Priority: High**

- FR5.1: Users shall have a personalized dashboard showing:
  - Recommended schemes
  - Saved/bookmarked schemes
  - Profile completion status
  - Recent activity
- FR5.2: Dashboard shall be accessible immediately after login
- FR5.3: Dashboard shall update dynamically based on profile changes

### FR6: Scheme Search & Filter
**Priority: Medium**

- FR6.1: Users shall be able to search schemes by name or keyword
- FR6.2: Users shall be able to filter schemes by:
  - Category (Education, Health, Agriculture, etc.)
  - State
  - Central vs State schemes
  - Benefit type
- FR6.3: Search results shall be displayed in real-time
- FR6.4: System shall support autocomplete for search queries

### FR7: Bookmark & Save Schemes
**Priority: Medium**

- FR7.1: Users shall be able to bookmark schemes
- FR7.2: Users shall be able to view all saved schemes
- FR7.3: Users shall be able to remove bookmarks
- FR7.4: Bookmarks shall persist across sessions

### FR8: Admin Panel - Scheme Management
**Priority: High**

- FR8.1: Admin shall be able to login to admin panel
- FR8.2: Admin shall be able to add new schemes with:
  - Scheme details
  - Eligibility criteria
  - Required documents
  - Application links
- FR8.3: Admin shall be able to update existing schemes
- FR8.4: Admin shall be able to delete schemes
- FR8.5: Admin shall be able to upload scheme datasets in bulk (CSV/JSON)

### FR9: Admin Panel - Eligibility Criteria Management
**Priority: High**

- FR9.1: Admin shall be able to define eligibility rules for each scheme
- FR9.2: Admin shall be able to update eligibility criteria
- FR9.3: System shall validate eligibility rule syntax
- FR9.4: Changes shall reflect immediately in recommendation engine

### FR10: Admin Panel - Analytics & Monitoring
**Priority: Medium**

- FR10.1: Admin shall view usage statistics:
  - Total users
  - Active users
  - Popular schemes
  - Search trends
- FR10.2: Admin shall view system performance metrics
- FR10.3: Admin shall export analytics reports

## Non-Functional Requirements

### NFR1: Performance
- NFR1.1: ML prediction response time shall be under 2 seconds
- NFR1.2: Page load time shall be under 3 seconds
- NFR1.3: System shall support concurrent users (minimum 1000)
- NFR1.4: Database queries shall execute within 500ms
- NFR1.5: API response time shall be under 1 second

### NFR2: Scalability
- NFR2.1: Architecture shall support horizontal scaling
- NFR2.2: Database shall handle growth of scheme data (10,000+ schemes)
- NFR2.3: System shall handle increasing user base (100,000+ users)
- NFR2.4: ML model shall be retrained periodically without downtime

### NFR3: Security
- NFR3.1: All passwords shall be hashed using bcrypt or similar
- NFR3.2: JWT tokens shall expire after defined period
- NFR3.3: All API endpoints shall be protected with authentication
- NFR3.4: Sensitive user data shall be encrypted at rest
- NFR3.5: HTTPS shall be enforced for all communications
- NFR3.6: System shall implement rate limiting to prevent abuse
- NFR3.7: Input validation shall prevent SQL injection and XSS attacks
- NFR3.8: Admin panel shall have role-based access control

### NFR4: Data Privacy
- NFR4.1: System shall comply with data protection regulations
- NFR4.2: User data shall not be shared with third parties
- NFR4.3: Users shall be able to delete their account and data
- NFR4.4: System shall maintain audit logs for data access

### NFR5: Usability
- NFR5.1: UI shall be intuitive and user-friendly
- NFR5.2: Platform shall be mobile responsive
- NFR5.3: Forms shall provide clear validation messages
- NFR5.4: System shall provide helpful error messages
- NFR5.5: Navigation shall be consistent across pages

### NFR6: Reliability
- NFR6.1: System uptime shall be 99.5% or higher
- NFR6.2: System shall implement error handling and logging
- NFR6.3: Database shall have automated backups
- NFR6.4: System shall gracefully handle ML model failures

### NFR7: Maintainability
- NFR7.1: Code shall follow industry best practices
- NFR7.2: System shall have comprehensive documentation
- NFR7.3: Components shall be modular and loosely coupled
- NFR7.4: ML model shall be versioned and reproducible

### NFR8: Compatibility
- NFR8.1: Platform shall support modern browsers (Chrome, Firefox, Safari, Edge)
- NFR8.2: Platform shall be responsive on mobile devices (iOS, Android)
- NFR8.3: APIs shall follow RESTful standards

## User Stories

### End User Stories

**US1: User Registration**
- As a citizen, I want to register on the platform so that I can discover schemes I'm eligible for.
- Acceptance Criteria:
  - User can register with email and password
  - System validates email format
  - User receives confirmation message

**US2: Profile Creation**
- As a registered user, I want to enter my personal details so that the system can recommend relevant schemes.
- Acceptance Criteria:
  - User can input all required demographic information
  - System validates input data
  - Profile is saved successfully

**US3: View Recommendations**
- As a user, I want to see schemes I'm eligible for so that I can apply for benefits.
- Acceptance Criteria:
  - System displays recommended schemes within 2 seconds
  - Schemes are ranked by relevance
  - Each scheme shows key information

**US4: View Scheme Details**
- As a user, I want to view complete details of a scheme so that I can understand benefits and requirements.
- Acceptance Criteria:
  - User can click on scheme to view full details
  - All scheme information is displayed clearly
  - Application link is provided

**US5: Bookmark Schemes**
- As a user, I want to save schemes for later so that I can review them again.
- Acceptance Criteria:
  - User can bookmark schemes
  - Bookmarked schemes appear in saved section
  - User can remove bookmarks

**US6: Search Schemes**
- As a user, I want to search for specific schemes so that I can find information quickly.
- Acceptance Criteria:
  - Search returns relevant results
  - Results appear in real-time
  - User can filter results

### Admin User Stories

**US7: Add New Scheme**
- As an admin, I want to add new schemes so that users can discover them.
- Acceptance Criteria:
  - Admin can input all scheme details
  - System validates data
  - Scheme appears in database immediately

**US8: Update Scheme Information**
- As an admin, I want to update scheme details so that information stays current.
- Acceptance Criteria:
  - Admin can edit existing schemes
  - Changes are saved successfully
  - Updated information reflects in user recommendations

**US9: Manage Eligibility Criteria**
- As an admin, I want to define eligibility rules so that recommendations are accurate.
- Acceptance Criteria:
  - Admin can set eligibility parameters
  - Rules are validated for correctness
  - ML model uses updated criteria

**US10: View Analytics**
- As an admin, I want to view usage statistics so that I can understand platform performance.
- Acceptance Criteria:
  - Dashboard shows key metrics
  - Data is updated in real-time
  - Reports can be exported

## Technical Requirements

### Technology Stack

**Frontend:**
- React.js (v18+)
- React Router for navigation
- Axios for API calls
- CSS framework (Material-UI or Tailwind CSS)
- State management (Redux or Context API)

**Backend:**
- Node.js with Express.js OR Python Flask
- RESTful API architecture
- JWT for authentication
- Input validation middleware

**Database:**
- MongoDB (NoSQL) OR PostgreSQL (SQL)
- Database indexing for performance
- Connection pooling

**Machine Learning:**
- Python 3.8+
- Scikit-learn for ML models
- Pandas for data processing
- NumPy for numerical operations
- Joblib for model serialization

**Deployment:**
- Cloud platform: AWS / Azure / Render
- Docker for containerization
- CI/CD pipeline
- Load balancer for scalability

**Development Tools:**
- Git for version control
- Postman for API testing
- Jupyter Notebook for ML development

## System Constraints

### Technical Constraints
- TC1: ML model accuracy depends on quality of training data
- TC2: Prediction speed limited by model complexity
- TC3: Database performance depends on indexing strategy
- TC4: Cloud hosting costs scale with user base

### Business Constraints
- BC1: Government scheme data must be manually collected and updated
- BC2: Eligibility criteria may change frequently
- BC3: Official application links may become outdated
- BC4: Platform requires ongoing maintenance and updates

### Regulatory Constraints
- RC1: Must comply with data protection laws
- RC2: User consent required for data collection
- RC3: Cannot guarantee scheme approval for users
- RC4: Must display accurate government information

## Assumptions

- A1: Government scheme data is available in structured format
- A2: Users have internet access to use the platform
- A3: Users can provide accurate personal information
- A4: Official scheme websites remain accessible
- A5: ML model can be trained with sufficient historical data
- A6: Users understand basic eligibility concepts
- A7: Admin will regularly update scheme information
- A8: Cloud infrastructure will remain available and reliable

## Acceptance Criteria

### System-Level Acceptance Criteria

**AC1: User Registration & Authentication**
- Users can successfully register and login
- JWT authentication works correctly
- Session management is secure

**AC2: ML Recommendation Engine**
- Model predicts eligibility with >80% accuracy
- Predictions complete within 2 seconds
- Recommendations are relevant to user profile

**AC3: Scheme Management**
- Admin can add, update, and delete schemes
- Changes reflect immediately in system
- Bulk upload functionality works correctly

**AC4: User Experience**
- Platform is mobile responsive
- All forms have proper validation
- Error messages are clear and helpful

**AC5: Performance**
- System handles 1000 concurrent users
- Page load times under 3 seconds
- Database queries execute efficiently

**AC6: Security**
- All sensitive data is encrypted
- API endpoints are protected
- No security vulnerabilities in code

**AC7: Data Accuracy**
- Scheme information is complete and accurate
- Eligibility criteria are correctly implemented
- Application links are valid

## Success Criteria

- SC1: Platform successfully recommends schemes with >80% accuracy
- SC2: User registration and retention rate >60%
- SC3: Average prediction response time <2 seconds
- SC4: System uptime >99.5%
- SC5: Positive user feedback score >4/5
- SC6: Admin can manage 1000+ schemes efficiently
- SC7: Platform handles 10,000+ registered users
- SC8: Zero critical security vulnerabilities

## Future Scope

### Phase 2 Enhancements
- Multilingual support (Hindi, Telugu, Tamil, Bengali, etc.)
- Voice-based input for accessibility
- Integration with DigiLocker for document verification
- Real-time scheme updates via government APIs
- AI chatbot assistant for user queries
- Mobile application (iOS and Android)
- SMS/Email notifications for new schemes
- Scheme application tracking
- Community forum for user discussions
- Advanced analytics and reporting for admins
