# Enhancement One: Software Design and Engineering

## Artifact Overview

The Paint Tracking System is a Python desktop application designed to solve real-world workflow and data tracking challenges in a sign and graphics manufacturing environment. The original artifact was a working prototype—a single 255-line file that mixed UI code, database access, and business logic together in a way typical of early learning projects. While functional, it represented beginner-level code that would not survive professional code review.

**Original Technology Stack:**
- Python with Tkinter for the user interface
- SQLite for database persistence
- Monolithic single-file architecture

**Enhanced Technology Stack:**
- Python with PySide6/Qt6 for a modern, professional interface
- SQLite with Repository pattern for data access
- Three-layer architecture with clear separation of concerns

## The Transformation

### Before: Monolithic Structure

The original `main.py` combined everything in one file:

```python
# Original: UI, database, and logic intertwined
class SearchFrame(ttk.Frame):
    def __init__(self, parent, database_path, **kwargs):
        super().__init__(parent, **kwargs)
        self.database_path = database_path  # Database path in UI class
        
    def search(self):
        # Direct database queries from UI code
        search_term = self.search_var.get()
        # SQL mixed with presentation logic...
```

Problems with this approach:
- **Tight coupling**: Changing the database required changing UI code
- **No separation of concerns**: Business rules scattered throughout
- **Security vulnerabilities**: No input validation layer, potential SQL injection
- **Untestable**: Cannot test business logic without UI

### After: Three-Layer Architecture

The enhanced version separates the application into distinct layers, each with a single responsibility:

```
paint_tracker_v2.0/
├── main.py                          # Application entry point
├── paint_tracker/
│   ├── data/
│   │   └── repository.py            # Data Access Layer (DAL)
│   ├── core/
│   │   └── services.py              # Business Logic Layer (BLL)
│   ├── ui/
│   │   ├── main_window.py           # Presentation Layer
│   │   └── dialogs/
│   │       └── job_form.py          # UI Components
│   ├── algorithms/
│   │   └── paint_calculator.py      # Calculation Engine
│   └── config/
│       └── settings.py              # Configuration Management
```

**Total transformation: 255 lines → 2,489 lines** across a properly organized codebase.

## Key Implementation Details

### Data Access Layer: Repository Pattern

The `JobRepository` class encapsulates all database operations, ensuring SQL statements are centralized and parameterized:

```python
class JobRepository:
    """
    SQLite repository. Each row represents one paint mix event.
    All database access is isolated in this layer.
    """
    
    def __init__(self, db_path: str):
        self.db_path = db_path
        self._conn = sqlite3.connect(self.db_path)
        self._conn.row_factory = sqlite3.Row
        self.ensure_schema()
    
    def create_job(self, job_data: Dict) -> int:
        """Create a new job entry with parameterized query."""
        cur = self._conn.cursor()
        cur.execute("""
            INSERT INTO jobs (job_number, manufacturer, paint_code, ...)
            VALUES (?, ?, ?, ...)
        """, (job_data['job_number'], job_data['manufacturer'], ...))
        self._conn.commit()
        return cur.lastrowid
```

Benefits of the Repository pattern:
- All SQL in one location for easy security auditing
- Parameterized queries prevent SQL injection
- Connection management handled consistently
- Easy to swap database backends (SQLite → PostgreSQL)

### Business Logic Layer: Service Classes

The BLL acts as gatekeeper between UI and data, handling validation and business rules:

```python
class ValidationService:
    @staticmethod
    def validate_job_payload(data: Dict) -> Tuple[bool, str]:
        """Validate job data before it reaches the database."""
        required = ["job_number", "manufacturer", "paint_code", "mixed_amount"]
        for k in required:
            if not str(data.get(k, "")).strip():
                return False, f"Missing field: {k}"
        
        # Version format validation
        pv = (data.get("paint_version") or "1.0").strip()
        if not pv or not all(p.isdigit() for p in pv.split(".") if p != ""):
            return False, "Invalid paint version format"
        
        # Numeric validation
        try:
            float(data.get("mixed_amount", 0))
        except ValueError:
            return False, "mixed_amount must be numeric"
        
        return True, "ok"
```

The separation allows business rules to change independently of UI or database code.

### Presentation Layer: PySide6/Qt6

Migration from Tkinter to PySide6 provides a modern, professional interface:

```python
class MainWindow(QMainWindow):
    """Main application window using Qt6 framework."""
    
    def __init__(self, job_service: JobService):
        super().__init__()
        self.job_service = job_service  # Dependency injection
        self.setup_ui()
    
    def add_job(self):
        """Handle job creation through the service layer."""
        data = self.collect_form_data()
        
        # Validation happens in service layer, not here
        success, message = self.job_service.create_job(data)
        
        if success:
            self.refresh_display()
        else:
            self.show_error(message)
```

The UI layer knows nothing about SQL or business rules—it simply collects user input and displays results.

## Security Enhancements

Security became a primary focus during the enhancement:

1. **Parameterized Queries**: Every database query uses parameterized statements to prevent SQL injection
2. **Multi-Layer Validation**: Input validated in UI (for user feedback) and again in BLL (defense in depth)
3. **Error Handling**: Comprehensive try-except blocks with logging that doesn't expose sensitive information
4. **Authentication Framework**: Structure prepared for bcrypt password hashing (implemented in Enhancement Three)
5. **Session Management**: User session tracking with proper timeout handling

## Course Outcomes Addressed

### Outcome 4: Demonstrating Ability to Use Well-Founded Techniques

The enhancement demonstrates mastery of professional software engineering patterns:
- **SOLID Principles**: Each class has a single, well-defined responsibility
- **Repository Pattern**: Industry-standard data access abstraction
- **Dependency Injection**: Services injected into UI components
- **Separation of Concerns**: Changes to one layer don't affect others

### Outcome 5: Developing a Security Mindset

Security considerations permeate the entire design:
- Defense in depth with validation at multiple layers
- Secure by default: parameterized queries everywhere
- Assumption that all input could be malicious
- Error messages that inform without revealing system details

## Reflection

### What I Learned

The most important thing I learned was to experience for myself the purpose of proper architecture. When I added functionality to allow for CSV exporting, I only had to make changes in the BLL and Presentation layers -- the DAL layer was not even touched. Likewise, when I migrated from Tkinter to PySide6, only the Presentation layer changed -- the BLL and DAL layers were 100% the same. This was a great way to see separation of concerns in action, not just in theory.

I also learned that good code doesn't have to have a limited number of lines of code. Admittedly, the nearly 2,500 lines is a jump from 255, but now each discrete piece of functionality is much easier to understand on its own. Because of the Repository pattern, making sure the application is secure is now easier -- every SQL statement is in a single place, and I can easily check that every query is parameterized.

### Challenges Faced

The largest challenge, which wasn't really a challenge while I was doing the work but was one in retrospect, was that of working with a monolithic application. In other words, every module depends on every other module, and there's nothing that can be refactored in isolation. I couldn't just refactor the database layer; I also had to consider how the BLL will need to be written in order to access it, and how the UI will need to be written in order to call the BLL, and so on and so forth. In fact, there's a danger in doing it one step at a time like that: you can spend so much time getting one thing right that you never move on to the next task. The solution to that was to be extremely methodical in my work: refactor one layer, test it to death, then move on to the next layer.

Another challenge was that the migration to PySide6 was a more complicated project than I thought it would be. Qt is a very powerful framework, but that power comes at the cost of complexity. Widgets act in different ways than Tkinter does, the layout management is much more sophisticated, and frankly, there's more to configure overall. In the end, it was well worth it to have an application that looks and feels professional.

### Connection to My Background

This entire enhancement was a reminder of something I learned back when I worked in the paint industry: "proper prep is 80% of the job." In this case, it's true that the initial architectural decisions really set up the maintainability and extensibility of the code. I also think the amount of attention to detail that I have developed through years of being able to mix paint (getting off by just a few drops can result in a miss match) applies well to software development. Likewise, in both cases, small details can make the difference between introducing bugs and security issues or not.

## Links

- [View Original Artifact](https://github.com/ChrisLajoworksNcodes/ChrisLajoworksNcodes.github.io/tree/main/artifacts/original)
- [View Enhanced Artifact (v2.0)](https://github.com/ChrisLajoworksNcodes/ChrisLajoworksNcodes.github.io/tree/main/artifacts/enhancement-one)
- [Enhancement One Narrative (PDF)](narratives/enhancement-one-narrative.pdf)
- [Back to Portfolio Home](index.md)
