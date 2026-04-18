# nepal-bulletin-board
Online News Portal for Nepal Bulletin Board - Capstone Project CPRO306
# Navigate to your desktop or projects folder
cd Desktop

# Clone the repository
git clone https://github.com/Jesan gurung/nepal-bulletin-board.git

# Navigate into the project folder
cd nepal-bulletin-board
# Create main folders
mkdir docs
mkdir wireframes
mkdir database
mkdir presentation
mkdir src

# Create subfolders for source code
mkdir src/css
mkdir src/js
mkdir src/images

# Create subfolders for documentation
mkdir docs/requirements
mkdir docs/research
mkdir docs/testing
# Create and edit README.md
echo "# Nepal Bulletin Board - Online News Portal" >> README.md
echo "" >> README.md
echo "## Project Overview" >> README.md
echo "This is a capstone project for CPRO306 at Kent Institute Australia." >> README.md
echo "" >> README.md
echo "### Features" >> README.md

echo "- Real-time news updates" >> README.md
echo "- Category-based browsing (Politics, Business, Sports, Entertainment, Technology)" >> README.md
echo "- Breaking news ticker" >> README.md
echo "- User comments with moderation" >> README.md
echo "- Newsletter subscription" >> README.md
echo "- Mobile-responsive design" >> README.md
echo "" >> README.md
echo "### Tech Stack" >> README.md
echo "- Frontend: HTML5, CSS3, JavaScript" >> README.md
echo "- Backend: PHP/Node.js" >> README.md
echo "- Database: MySQL" >> README.md
echo "- Version Control: Git & GitHub" >> README.mdcat > docs/requirements/functional-requirements.md << 'EOF'
# Functional Requirements for Nepal Bulletin Board

## FR1: User Registration and Authentication
- Users can create an account using email and password
- Users can log in and log out
- Password recovery via email

## FR2: News Browsing
- Users can view news articles by category (Politics, Business, Sports, Entertainment, Technology)
- Breaking news ticker displays latest urgent headlines
- Search functionality by keyword, date, or category

## FR3: Article Management
- Journalists can submit draft articles
- Editors can approve, reject, or request revisions
- Admin can publish, edit, or delete any article

## FR4: User Engagement
- Registered users can comment on articles
- Comments are moderated by editors
- Users can like and save articles

## FR5: Newsletter
- Users can subscribe to daily/weekly email newsletters
- Unsubscribe option available in every email

## FR6: Responsive Design
- Portal works on mobile, tablet, and desktop devices
- Dark mode option available
EOF
cat > docs/requirements/non-functional-requirements.md << 'EOF'
# Non-Functional Requirements for Nepal Bulletin Board

## NFR1: Performance
- Page load time < 2 seconds
- API response time < 500ms

## NFR2: Security
- HTTPS encryption
- Password hashing using bcrypt
- Protection against SQL injection and XSS attacks

## NFR3: Availability
- 99.9% uptime
- Maximum downtime 8.5 hours per year

## NFR4: Scalability
- Support up to 10,000 concurrent users
- Use CDN for static assets

## NFR5: Usability
- Mobile-first design
- WCAG 2.1 AA accessibility compliance

## NFR6: Maintainability
- Modular code structure (MVC pattern)
- Well-documented code and API
EOF
cat > docs/research/market-analysis.md << 'EOF'
# Market Analysis Report - Nepal Bulletin Board

## Executive Summary
The Nepalese digital news landscape is undergoing rapid transformation, driven by increasing internet penetration, widespread smartphone adoption, and a young, tech-savvy population. Currently, there are over 1,672 active online news portals registered in Nepal, making the market highly fragmented and competitive.

## Key Competitors
- Ratopati News Network
- Kathmandu Post (Kantipur Media Group)
- Roshan Shrestha App
- Haatma
- Nagarik News

## SWOT Analysis
**Strengths:** Modern tech stack, mobile-first design, no political baggage
**Weaknesses:** No brand recognition, limited budget, small team
**Opportunities:** Growing demand for credible news, declining trust in political media
**Threats:** Intense competition, social media dominance, low digital ad rates

## Differentiation Strategy
- Credibility first (fact-checking section)
- Political neutrality
- Modern user experience (dark mode, fast loading)
- Youth-focused content and social integration

## Conclusion
The market presents significant opportunity for a credible, neutral, modern news platform.
EOF
cat > database/schema.sql << 'EOF'
-- Nepal Bulletin Board Database Schema
-- MySQL Database

CREATE DATABASE IF NOT EXISTS nepal_bulletin_board;
USE nepal_bulletin_board;

-- Users table
CREATE TABLE users (
    user_id INT PRIMARY KEY AUTO_INCREMENT,
    full_name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    role ENUM('visitor', 'journalist', 'editor', 'admin') DEFAULT 'visitor',
    status ENUM('active', 'suspended', 'banned') DEFAULT 'active',
    profile_picture VARCHAR(255),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Categories table
CREATE TABLE categories (
    category_id INT PRIMARY KEY AUTO_INCREMENT,
    category_name VARCHAR(50) NOT NULL,
    slug VARCHAR(50) UNIQUE NOT NULL,
    description TEXT
);

-- Articles table
CREATE TABLE articles (
    article_id INT PRIMARY KEY AUTO_INCREMENT,
    title VARCHAR(200) NOT NULL,
    slug VARCHAR(200) UNIQUE NOT NULL,
    content LONGTEXT NOT NULL,
    featured_image VARCHAR(255),
    category_id INT,
    author_id INT,
    status ENUM('draft', 'pending', 'published', 'rejected', 'archived') DEFAULT 'draft',
    view_count INT DEFAULT 0,
    published_at DATETIME,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (category_id) REFERENCES categories(category_id),
    FOREIGN KEY (author_id) REFERENCES users(user_id)
);

-- Comments table
CREATE TABLE comments (
    comment_id INT PRIMARY KEY AUTO_INCREMENT,
    article_id INT,
    user_id INT,
    content TEXT NOT NULL,
    status ENUM('pending', 'approved', 'spam', 'deleted') DEFAULT 'pending',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (article_id) REFERENCES articles(article_id),
    FOREIGN KEY (user_id) REFERENCES users(user_id)
);

-- Subscriptions table
CREATE TABLE subscriptions (
    subscription_id INT PRIMARY KEY AUTO_INCREMENT,
    email VARCHAR(100) NOT NULL,
    frequency ENUM('daily', 'weekly') DEFAULT 'daily',
    is_active BOOLEAN DEFAULT TRUE,
    subscribed_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Insert sample categories
INSERT INTO categories (category_name, slug, description) VALUES
('Politics', 'politics', 'Political news and analysis'),
('Business', 'business', 'Economy, finance and business news'),
('Sports', 'sports', 'Sports news and updates'),
('Entertainment', 'entertainment', 'Movies, music and celebrity news'),
('Technology', 'technology', 'Tech news and digital trends');
EOF
cat > docs/testing/test-plan.md << 'EOF'
# Test Plan - Nepal Bulletin Board

## Test Objectives
- Verify all functional requirements are met
- Ensure system handles expected load
- Validate security measures

## Test Types

### 1. Unit Testing
| Module | Test Cases |
|--------|------------|
| User Authentication | Valid login, invalid login, registration, password reset |
| Article CRUD | Create, read, update, delete operations |
| Comment System | Post, edit, delete, moderate comments |

### 2. Integration Testing
- Database connection tests
- API endpoint tests
- Email notification system

### 3. User Acceptance Testing (UAT)
| Scenario | Expected Result |
|----------|-----------------|
| Visitor reads article | Article loads within 2 seconds |
| User registers account | Verification email sent |
| Journalist submits article | Status changes to pending |
| Editor approves article | Article published immediately |

### 4. Performance Testing
- Load test: 10,000 concurrent users
- Stress test: 20,000 users
- Response time: < 2 seconds

### 5. Security Testing
- SQL injection prevention
- XSS attack prevention
- HTTPS enforcement
- Password strength validation

## Test Environment
- Browsers: Chrome, Firefox, Safari, Edge
- Devices: iPhone, Android, Windows, Mac
- Network: 3G, 4G, WiFi
EOF
cat > presentation/presentation-outline.md << 'EOF'
# SRS Presentation Outline - Nepal Bulletin Board

## Slide 1: Title Slide
- Project Name: Nepal Bulletin Board
- Team Members Names
- Course: CPRO306 Capstone Project

## Slide 2: Project Background
- Problem: Fragmented, unreliable news in Nepal
- Solution: Centralized, credible online news portal

## Slide 3: Project Scope & Deliverables
- Core features: News browsing, commenting, subscriptions
- Deliverables: Working portal, documentation, presentation

## Slide 4: Requirements Analysis
- 8 Functional Requirements
- 7 Non-Functional Requirements

## Slide 5: Use Case Diagram
- Actors: Visitor, User, Journalist, Editor, Admin
- Use Cases: View, comment, submit, approve, manage

## Slide 6: Database Design
- 5 core tables: Users, Categories, Articles, Comments, Subscriptions

## Slide 7: Budget
- Professional: $27,459 AUD
- Student: $15 AUD

## Slide 8: Risk Management
- Top 3 risks: Downtime, cyber attacks, fake news

## Slide 9: Timeline & Gantt Chart
- Weeks 1-2: Requirements & Design
- Weeks 3-6: Development
- Weeks 7-8: Testing
- Week 9: Deployment

## Slide 10: Q&A
EOF
