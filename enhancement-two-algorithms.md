# Enhancement Two: Algorithms and Data Structures

## Artifact Description

For Enhancement Two, I added algorithmic functionality to the Paint Tracking System, the artifact I improved during Enhancement One. I originally wrote the Paint Tracking System as a Python desktop application to assist in the paint mixing process at my place of employment, a sign and graphics production shop. The original prototype, developed earlier this year, provided a simple interface for recording paint mixes, tracking job history, and maintaining consistent records between departments in the production workflow. I was able to prototype it quickly in my spare time at work as I was already intimately familiar with the processes and business requirements of the shop, from 17 years of experience as an automotive paint technician and 4 years of experience in the sign manufacturing field.

Enhancement One (Software Engineering and Design) improved the original script, which was a single 400 line file, by refactoring it into a properly architected application with nearly 2,500 lines across a three-layer architecture using PySide6/Qt6, the Repository pattern, and service-oriented business logic. Enhancement Two built directly on top of that foundation by adding the core algorithmic functionality which provides real operational value in an environment like ours.

---

## Justification for Inclusion

I chose this artifact to represent Algorithms and Data Structure as paint mixing in a production shop environment involves precise mathematical relationships that can be interpreted as computational problems. I had to design an algorithm to calculate the component weights and amounts for a given paint mix based on specific mixing ratios.

### Specific Components Showcasing Skills

**Paint Mix Calculator Algorithm:** I implemented the calculator as a system of ratio-based equations using the 3:1:3 (paint:catalyst:additive) ratio for interior jobs and 3:1:1 (paint:catalyst:reducer) ratio for exterior jobs. I made the interior and exterior mix the defaults in the ratio selector in the job form. The algorithm takes a given input for the amount of base paint and calculates the exact quantity of each component needed to meet those mixing ratios.

**Constant Time Complexity:** The core calculations in the PaintMixCalculator class run in constant time with respect to the paint amount input. Since we're dealing with a fixed number of components (always 3 components regardless of job type), even the validation methods effectively run in O(1) time. This reflects a sound understanding of algorithmic efficiency as the calculations are simple ratio operations with no loops that scale with input size.

**Service Layer Integration:** I built a PaintCalculatorService which encapsulates the PaintMixCalculator algorithm and integrates it with the three-layer architecture of the system. This demonstrates knowledge of how to fit algorithms into larger software systems and design for appropriate separation of concerns.

**Data Structure Design:** The PaintMixCalculator class returns a dictionary with sub-dictionaries to hold the breakdown of components and total amount in a given mix, as well as metadata like total amount and job notes. The job summaries displayed on the dashboard are a different data structure which aggregates the entries by job number, with calculated totals and first/last mixed dates.

**Mix Detail Dialog:** I designed and implemented a dedicated detail view for each entry, which opens on a double-click and displays the full entry information including the notes in a scrollable text area. This was a usability improvement to make data more accessible without overcrowding the main table view.

**Coverage Estimation Algorithm:** While not yet exposed in the UI, I also implemented a coverage estimation algorithm that calculates square footage based on paint amount and number of coats. This uses industry-standard coverage rates (350 sq ft per gallon) and demonstrates forward-thinking design - building the algorithmic foundation for features that might be needed down the road.

---

## How the Artifact Was Improved

The basic paint tracking application from the original prototype was a record-keeping tool but included no calculation or reporting capabilities, requiring users to manually do math for each job or rely on memory for estimates. The calculator automates the component weights based on the amount entered in the job form and immediately populates the breakdown of paint/catalyst/additive or reducer quantities, eliminating estimation and human error. 

The dashboard was reorganized to present job summaries with aggregated data (total amount mixed, number of mixes, last mixed date) rather than individual mix entries, making it more immediately useful for operators to scan for status at a glance. The specific columns now show Job Number, Last Mixed, Total Amount Mixed, Number of Mixes, and Mixed By, which is exactly what production staff need to see first.

---

## Course Outcomes

Enhancement Two addresses the outcomes I intended to target from this course that I documented in Module One, specifically Outcome 3 and Outcome 4.

### Outcome 3
*Design and evaluate computing solutions that solve a given problem using algorithmic principles and computer science practices and standards appropriate to its solution, while managing the trade-offs involved in design choices*

The paint mixing algorithm was designed to have both mathematical correctness and real-world usability, with a trade-off between exact calculation results and rounding to increments that are useful for real production operators. Exact calculations would produce amounts like 142.857 grams, but the application is not useful if it displays numbers that operators cannot practically measure (nobody is measuring to three decimal places in a production shop). Rounding to one decimal place was necessary to match what operators can actually measure using the digital scales we have. I feel this shows an understanding that algorithms are tools to solve real problems and not an end unto themselves.

### Outcome 4
*Demonstrate an ability to use well-founded and innovative techniques, skills, and tools in computing practices for the purpose of implementing computer solutions that deliver value and accomplish industry-specific goals*

The mixing ratios themselves (3:1:3 for interior, 3:1:1 for exterior) are paint line specific ratios. By hard-coding that same domain knowledge into the algorithm as part of the software solution, I am able to provide immediate value to the production environment that the application runs in. The Paint Calculator does not just do math, it does the math that professional painters do every day in their jobs. I think this is the ideal overlap of computer science skills with industry-specific knowledge producing genuinely useful software rather than pointless busywork.

---

## Reflection on the Enhancement Process

### What I Learned

The biggest thing I learned in this enhancement process was the value of iterative improvement based on actual use. I went through multiple rounds of refinement on the integration of the calculator as I was testing the application, from just getting it to work to getting it to work just right. This is an example of subtle issues that only show when the application is running, such as date format not being consistent with industry conventions or the column ordering not matching the natural reading pattern for production operators.

I also found that designing and implementing algorithms involves more than just the math part. The math was actually the easy part, basic ratio calculations. The tricky part was in integration with the other components and getting the results to flow back to the UI: how to connect the calculator to the job form and services, display the calculated results as the user types, connect the service to the dashboard model, update the view, and make sure it all worked together seamlessly. Overall, this just reinforced that an algorithm does not exist in a vacuum, it is part of a system, and properly designing it involves considering that context.

### Challenges Faced

The first challenge was simply keeping track of related code. When I modified the dashboard to show job summaries instead of individual entries, I had to change the column definitions as well as the population code, the search function, and both delete functions (by entry ID and by job number) to all work with the new aggregated structure. Fixing one without fixing all would have caused functionality to break.

Another challenge was in managing feature creep within the scope of the enhancement. Any change suggested further changes: the calculator required me to rethink dashboard layout, rethinking dashboard layout suggested adding the detail dialog, adding the detail dialog suggested reorganizing columns. I had to make decisions about what was part of this particular enhancement and what could wait for future improvements, in order to maintain focus while still delivering value. Some features, like the coverage estimation, I decided to implement in the algorithm layer but not expose in the UI yet - building the foundation for later without expanding the current scope too much.

---

## Conclusion

Enhancement Two allows me to demonstrate my ability to design and implement algorithms that solve problems in a real-world production environment. The paint mixing calculator, by correctly applying the mixing ratios and practical rounding in a user interface integrated into the production workflow, shows that I am able to apply algorithmic principles to real problems. Dashboard improvements with properly aggregated job summaries show understanding of data aggregation and presentation for a particular industry's operational needs. The systematic debugging process and iterative refinement I used to deliver the enhancement also shows competency in computer science practices and software engineering principles.

---

## Source Code

- [View Enhancement Two Source Code](https://github.com/ChristopherLajoie82/CS499-Capstone/tree/main/paint_tracker_v3.0/paint_tracker/algorithms)
- [View PaintMixCalculator Class](https://github.com/ChristopherLajoie82/CS499-Capstone/blob/main/paint_tracker_v3.0/paint_tracker/algorithms/paint_calculator.py)

---

[← Back to Home](index.md) | [View Enhancement One](enhancement-one.md) | [View Enhancement Three →](enhancement-three.md)
