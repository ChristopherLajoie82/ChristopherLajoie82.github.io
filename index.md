# Christopher R. Lajoie

### Computer Science ePortfolio — SNHU CS 499

Welcome to my Computer Science ePortfolio for CS 499. This site showcases the Paint Tracking System, an application I developed to solve real workflow and data tracking problems at the sign and graphics shop where I work. What started as a 400-line prototype has evolved through three systematic enhancements into a production-ready application exceeding 14,000 lines of professionally architected code.

---

## About Me

I'm a career changer with 21 years in the skilled trades—17 years as an automotive paint technician and 4 years in sign manufacturing—now transitioning into software development. My background isn't something I'm leaving behind; it's a foundation I'm building on. The precision required for color matching, the systematic thinking needed for manufacturing processes, and the understanding of how one component affects the entire system—these skills transfer directly to software development.

I'm currently completing my Computer Science degree at Southern New Hampshire University with a concentration in data analysis. I build practical tools that solve real problems, and I bring domain expertise that most entry-level developers don't have.

- **Email:** lajoiechristopher82@gmail.com  
- **GitHub:** [ChristopherLajoie82](https://github.com/ChristopherLajoie82)  
- **LinkedIn:** [christopher-lajoie-82063430b](https://www.linkedin.com/in/christopher-lajoie-82063430b/)

---

## Professional Self-Assessment

Before exploring the technical enhancements, I encourage you to read my [Professional Self-Assessment](professional-self-assessment.md), which describes my journey through the Computer Science program and how this capstone project demonstrates my readiness to enter the field as a software developer with a unique combination of technical skills and domain expertise.

---

## Capstone Artifact: Paint Tracking System

**Stack:** Python, PySide6/Qt6, SQLite, PostgreSQL, bcrypt  
**Transformation:** 400 lines → 14,000+ lines across three enhancements  
**Purpose:** A desktop application for tracking paint usage, calculating mix ratios, and managing production data in a sign and graphics manufacturing environment.

This isn't just a capstone artifact—it's a working application that solves real problems I encountered in my professional life, built to professional standards with security, scalability, and maintainability in mind.

---

## Code Review

The code review video walks through the original artifact's architecture, identifies areas for improvement, and outlines the enhancement plan for all three categories. This review demonstrates my ability to critically evaluate code and communicate technical decisions.

- [Watch the Code Review Video](code-review.md)

---

## Enhancement One: Software Design and Engineering

I took a 400-line monolithic script where UI, database, and business logic were all mashed together and refactored it into a professionally architected application with clear separation of concerns.

**Key Accomplishments:**
- Implemented three-layer architecture (DAL, BLL, Presentation)
- Applied the Repository pattern for all database access
- Migrated from Tkinter to PySide6/Qt6 for a professional interface
- Added comprehensive error handling and logging
- Parameterized all SQL queries to prevent injection attacks

**Course Outcomes:** 4 (Software Engineering/Design), 5 (Security Mindset)

[Read the Full Enhancement One Narrative →](enhancement-one-software-design.md)

---

## Enhancement Two: Algorithms and Data Structures

This is where industry knowledge meets computer science. The PaintMixCalculator encodes 17 years of paint industry experience into working algorithms that compute component breakdowns using standard mixing ratios.

**Key Accomplishments:**
- PaintMixCalculator with O(1) constant-time complexity
- Industry-standard ratios: 3:1:3 (interior), 3:1:1 (exterior)
- Dictionary structures for efficient component breakdowns
- Job summary aggregation for dashboard display
- Mix Detail Dialog for viewing calculation breakdowns

**Course Outcomes:** 3 (Computing Solutions), 4 (Well-Founded Techniques)

[Read the Full Enhancement Two Narrative →](enhancement-two-algorithms.md)

---

## Enhancement Three: Databases

Enhancement Three completed the transformation into a production-ready application with enterprise-grade security features and flexible database support.

**Key Accomplishments:**
- bcrypt password hashing with work factor 12 (~250ms per hash)
- Account lockout after 5 failed attempts (15-minute ban)
- Role-based access control (admin vs. user)
- Foreign key relationships for audit trails
- Repository Factory pattern for SQLite/PostgreSQL support
- Constant-time password comparison to prevent timing attacks

**Course Outcomes:** 4 (Well-Founded Techniques), 5 (Security Mindset)

[Read the Full Enhancement Three Narrative →](enhancement-three-databases.md)

---

## Artifacts and Source Code

### Original Artifact
- [View Original Source Code](https://github.com/ChristopherLajoie82/ChristopherLajoie82.github.io/tree/main/artifacts/original)

### Enhanced Artifacts
- [Enhancement One: Software Design](https://github.com/ChristopherLajoie82/ChristopherLajoie82.github.io/tree/main/artifacts/enhancement-one)
- [Enhancement Two: Algorithms](https://github.com/ChristopherLajoie82/ChristopherLajoie82.github.io/tree/main/artifacts/enhancement-two)
- [Enhancement Three: Databases](https://github.com/ChristopherLajoie82/ChristopherLajoie82.github.io/tree/main/artifacts/enhancement-three)

### Enhancement Narratives (PDF)
- [Enhancement One Narrative](narratives/enhancement-one-narrative.pdf)
- [Enhancement Two Narrative](narratives/enhancement-two-narrative.pdf)
- [Enhancement Three Narrative](narratives/enhancement-three-narrative.pdf)

---

*Last updated: December 2025*
