# Enhancement Three: Databases

## Artifact Description

I chose the Paint Tracking System that I have been developing through all three enhancement milestones as the Artifact for the Databases category. Enhancement Three in particular adds the database security, user authentication, and multi-database support that would be required for a production deployment.

The Paint Tracking System was originally written as a Python desktop application for paint mixing support at my place of employment, a sign and graphics production shop. The original prototype was a single file with just under 400 lines of code and provided a rudimentary GUI for data entry and job history tracking. It was a "version 1.0" kind of thing -- a working prototype, but one that wouldn't pass code review if it were actually used in a professional setting.

Enhancement One (Software Engineering and Design) refactored the prototype into a proper 2,400+ line of code application with a three-layer architecture (PySide6/Qt6, Repository pattern, service-oriented business logic). Enhancement Two (Algorithms and Data Structure) added the paint mixing calculator with industry-standard ratios (3:1:3 for interior, 3:1:1 for exterior) and reorganized the dashboard to show job summaries and aggregated data. Enhancement Three builds on top of that foundation by adding comprehensive database security features that any production system would require.

---

## Justification for Inclusion

I chose this artifact to represent the Databases category because a paint tracking system in a real shop would have to consider multi-user support, security of sensitive data, and the possibility of scaling from a single workstation to a networked multi-user deployment. The original prototype, being a single file on a single workstation, had no concept of multiple users at all -- anyone who had access to the computer had access to all the data. In a real shop environment, though, you need to know who mixed what, when, and have mechanisms to prevent unauthorized access to production records. Enhancement Three addresses all of those requirements.

### Specific Components Showcasing Skills

**User Authentication with bcrypt:** I implemented a complete user authentication system using bcrypt password hashing with a work factor of 12. This is the industry standard recommended way to store passwords -- bcrypt will automatically generate a random salt for each password and include it with the hash so even if two people have the same password they will get different hashes. A work factor of 12 means each hash takes about 250ms to calculate, which is invisible to users but will require a brute-force attack to be infeasible.

![Login Dialog](images/login-dialog.png)
*Login dialog showing account lockout protection*

**Account Lockout Protection:** The system tracks failed logins per user and bans the user for 15 minutes after 5 consecutive failures. This information is stored in the database and used to set the failed_attempts and locked_until fields in the users table. The user is told "Invalid username or password. X attempts remaining." This information lets legitimate users know when they are about to get locked out, while also keeping hackers from making unlimited password guesses.

**Role-Based Access Control:** I implemented a role system with "admin" and "user" roles. Admin users have access to functions like creating new users, viewing all user accounts, changing database settings, etc. Regular users can only do standard operations like add jobs and view data. The role is stored in the users table and checked on every privileged operation.

![Main Dashboard](images/dashboard.png)
*Main dashboard showing "Mixed By" column*

![Admin Menu](images/admin-menu.png)
*Tools menu showing role-based access control with admin-only functions*

**Foreign Key Relationships:** The jobs table now includes a created_by field that references the users table. This creates an audit trail -- every job record has to reference the user who created it. The "Mixed By" column shows which user created each job. The status bar at the bottom also shows "Logged in as: USERNAME (Role)" indicating both username and role.

**Repository Factory Pattern for Multi-Database Support:** I implemented a Repository Factory that can create either SQLite repositories for single-user local deployments or PostgreSQL repositories for multi-user network environments. Both implementations share the same interface, so the rest of the application code does not need to know which database is being used.

![Database Settings](images/database-settings.png)
*Database Settings dialog showing SQLite and PostgreSQL configuration options*

**Attachments Table for PDF Storage:** I added an attachments system that allows users to associate PDF files with job records. The attachments table stores metadata including the original filename, file size, description, uploader username, and upload date. The actual PDF files are stored on the filesystem with the database maintaining references. This hybrid approach keeps the database performant while still maintaining referential integrity.

**Database Indexes:** I added indexes on frequently queried columns like job_number and created_by to improve query performance. These are not visible in the UI but show understanding of database optimization for production workloads.

---

## Course Outcomes

### Outcome 4
*Demonstrate an ability to use well-founded and innovative techniques, skills, and tools in computing practices for the purpose of implementing computer solutions that deliver value and accomplish industry-specific goals*

The database enhancement shows well-founded techniques through use of bcrypt for password hashing (industry standard), parameterized queries throughout (no SQL injection possible), and the Repository Factory pattern for database abstraction. The PostgreSQL support in particular delivers value for industry deployment -- a single-user SQLite database is fine for a prototype but any real production shop with multiple workstations will need a centralized database multiple users can access simultaneously. This is exactly the kind of practical, industry-specific consideration that separates professional software from student projects.

### Outcome 5
*Develop a security mindset that anticipates adversarial exploits in software architecture and designs to expose potential vulnerabilities, mitigate design flaws, and ensure privacy and enhanced security of data and resources*

This enhancement is fundamentally about security. The bcrypt implementation with work factor 12 makes password cracking computationally expensive even if an attacker obtains a copy of the database. The account lockout system prevents brute-force attacks. Role-based access control ensures users can only perform actions appropriate to their role. I also implemented constant-time password comparison to prevent timing attacks, where an attacker could potentially deduce information about a password based on how long the comparison takes. These are not theoretical concerns, they are the kinds of attacks that happen in the real world, and addressing them demonstrates a security mindset that considers adversarial behavior from the design phase.

---

## Reflection on the Enhancement Process

### What I Learned

The biggest thing I learned in this enhancement process is that security is not something you bolt on at the end, it has to be designed in from the beginning. When I was designing the users table schema, I had to consider not just basic authentication fields but also what I would need for account lockout (failed_attempts, locked_until), for role management (role), and for audit purposes (created_at). If I had just created a simple username/password table and then tried to bolt these features on later I would have had to migrate existing data and potentially break things.

I also learned a lot about password hashing that I did not know before. I thought hashing was just a matter of running the password through a function, but bcrypt is much more sophisticated. The work factor determines how computationally expensive each hash is (defense against brute-force attacks), the salt is generated automatically and stored with the hash (so even identical passwords produce different hashes), and the constant-time comparison is critical because otherwise an attacker could measure how long the comparison takes and use that to deduce information about the password. This is the kind of detail that separates secure software from software that looks secure but is not.

### Challenges Faced

The first challenge was implementing the Repository Factory pattern to support both SQLite and PostgreSQL. The two have subtle differences in SQL syntax -- for example SQLite uses ? for parameter placeholders while PostgreSQL uses $1, $2, etc. I had to create separate repository implementations that share a common interface. This was more work up front but means the rest of the application does not need to know or care which database is being used. The Business Logic Layer calls the same methods regardless, and the Factory decides which implementation to create based on the configuration.

Another challenge was the account lockout logic. I had to track failed attempts per user, reset the counter on successful login, implement a time-based lockout, and make sure the lockout actually prevented login attempts even if the correct password was entered. Testing this was tedious because I had to deliberately fail logins multiple times, wait for lockouts to expire, verify the counter reset correctly, etc. But this is exactly the kind of thorough testing security features require.

The attachment system presented an interesting design decision about whether to store files directly in the database as BLOBs or on the filesystem with database references. I chose the hybrid approach because storing large files in the database can cause performance issues and make backups unwieldy. The database stores metadata and a reference to file location, while the actual PDFs live in a dedicated attachments directory. This keeps the database compact while still maintaining the relationship between attachments and jobs.

---

## Conclusion

Enhancement Three completes the transformation of the Paint Tracking System from simple prototype to production-ready application. The original 400-line script is now over 14,000 lines of professionally architected code across three enhancement milestones. More importantly, the application has the security features and scalability options it would require for actual deployment in a production shop environment.

This enhancement demonstrates my ability to implement databases that deliver real value -- not just storing data, but protecting it with industry-standard security measures, scaling it with multi-database support, and maintaining audit trails through proper relational design. The systematic approach I have used throughout this capstone project, building each enhancement on the foundation of the previous one, shows that I can plan and execute a multi-phase software development effort while maintaining code quality and architectural integrity.

---

## Source Code

- [View Enhancement Three Source Code](https://github.com/ChristopherLajoie82/CS499-Capstone/tree/main/paint_tracker_v3.0/paint_tracker/data)
- [View User Repository](https://github.com/ChristopherLajoie82/CS499-Capstone/blob/main/paint_tracker_v3.0/paint_tracker/data/user_repository.py)
- [View Authentication Service](https://github.com/ChristopherLajoie82/CS499-Capstone/blob/main/paint_tracker_v3.0/paint_tracker/core/auth_service.py)

---

[← Back to Home](index.md) | [View Enhancement One](enhancement-one.md) | [View Enhancement Two](enhancement-two.md)
