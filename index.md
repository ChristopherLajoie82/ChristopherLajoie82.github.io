# Christopher R Lajoie  
### Computer Science ePortfolio — SNHU CS 499

Welcome to my Computer Science ePortfolio for CS 499.  
This site contains my selected artifact, the enhancement plans for the three rubric areas, my code review, the professional self assessment, and links to source code hosted on GitHub.

---

## About Me
I am a data focused software developer based in Rhode Island. I enjoy building practical tools that support operations and decision making.  
This ePortfolio centers on a Paint Tracking Program that I am enhancing during the capstone to demonstrate software design, algorithms, and databases.

- Email: lajoiechristopher82@gmail.com  
- GitHub: https://github.com/ChristopherLajoie82  
- LinkedIn: https://www.linkedin.com/in/christopher-lajoie-82063430b/

---

## Capstone Artifact
**Project:** Paint Tracking Program  
**Stack:** Python, PySide Six, SQLite, PyInstaller  
**Goal:** Deliver a reliable desktop application for tracking product intake, mix creation by ratio, and job usage with persistent records and light analytics.

- Source repository: *(to be added when pushed)*  
- Installer build: *(to be added when built with PyInstaller)*

---

## Enhancement Plans

### Category One — Software Engineering and Design
**Artifact Origin:** Personal project prototype in Python.  
**Planned Enhancement:**  
- Three layer structure: `app.ui`, `app.services`, `app.models`  
- PySide Six interface for intake, mixing, and usage  
- Structured logging and input validation  
- Packaging with PyInstaller  
- Unit and integration tests

**Outcome Alignment:** Outcomes 2, 3, and 4

---

### Category Two — Algorithms and Data Structures
**Planned Enhancement:** Ratio based batch calculation with validation and reusable recipes.  
**Pseudocode:**

<pre>
function compute_mix(total_volume_ml, components):
    assert total_volume_ml > 0
    total_parts = sum(c.parts for c in components)
    assert total_parts > 0

    batch = []
    for c in components:
        share = c.parts / total_parts
        component_volume = total_volume_ml * share
        batch.append({
            "product_id": c.product_id,
            "volume_ml": round(component_volume, 2)
        })

    return batch
</pre>

**Outcome Alignment:** Outcomes 3 and 4

---

### Category Three — Databases
**Planned Enhancement:** Move from flat files to a relational SQLite database with transactions and constraints.  
**Tables:** products, inventory, mixes, mix_components, job_usage  
**Outcome Alignment:** Outcomes 3, 4, and 5

---

## Code Review
The code review will show architecture changes, refactoring decisions, and testing strategy. It will include examples of separation of concerns, parameterized SQL, exception handling, and version control discipline with clear commit messages and an ADR document.

---

## Professional Self Assessment
The self assessment will describe my growth across design, algorithms, and databases and how the capstone improved my practice of clean architecture, testing, secure coding, and documentation. It will also cover next steps for career goals in software engineering with a strong data focus.

---

## Downloads and Links
- Project repository: *(link to be added)*  
- Executable installer: *(link to be added)*  
- Resume: *(link to be added)*

---

_Last updated: {{ site.time | date: "%B %d, %Y" }}_
