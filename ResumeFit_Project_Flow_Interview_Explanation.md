# ResumeFit -- Resume Analysis & Skill Gap System

## 1. Project Overview

**ResumeFit** is a Streamlit-based web application that analyzes a
candidate's resume and helps identify suitable entry-level job roles and
missing technical skills.

The system accepts a resume in PDF format, extracts its text, detects
known technical skills using keyword matching, compares those skills
with predefined job-role requirements, calculates a rule-based resume
score, identifies skill gaps, and provides learning recommendations.

The application also supports a **Job Description mode**, where the user
can paste a specific job description. ResumeFit extracts known technical
skills from the job description and compares them with the skills
detected in the resume.

The project uses:

-   Python
-   Streamlit
-   Pandas
-   Matplotlib
-   pypdf
-   JSON
-   MySQL
-   Git
-   GitHub
-   Streamlit Community Cloud

------------------------------------------------------------------------

# 2. Problem Statement

Students and fresh graduates often have difficulty understanding:

-   Which job roles their current skills match
-   Which skills are missing for a target role
-   How well their resume matches a particular job
-   What technical skills they should improve

ResumeFit solves this problem by providing a simple resume analysis and
skill-gap system.

Instead of manually comparing a resume with multiple job roles or job
descriptions, the application performs the comparison automatically
using a rule-based approach.

------------------------------------------------------------------------

# 3. Main Objectives

The main objectives of ResumeFit are:

1.  Extract text from a PDF resume.
2.  Detect technical skills from the resume.
3.  Recommend suitable entry-level roles.
4.  Allow the user to select a specific target role.
5.  Calculate the skill match percentage.
6.  Identify skills already present in the resume.
7.  Identify missing skills.
8.  Provide recommendations for improving missing skills.
9.  Calculate a rule-based resume score.
10. Visualize skill coverage using a donut chart.
11. Allow analysis of a custom job description.
12. Store completed analyses in MySQL.
13. Provide analysis history.
14. Deploy the application using Streamlit Community Cloud.

------------------------------------------------------------------------

# 4. High-Level Project Flow

The overall flow is:

``` text
                    USER
                     |
                     v
              Upload Resume PDF
                     |
                     v
             PDF Text Extraction
                     |
                     v
              Resume Text
                     |
                     v
             Skill Detection
                     |
                     v
          Detected Resume Skills
                     |
          +----------+----------+
          |                     |
          v                     v
   Predefined Role         Job Description
       Mode                     Mode
          |                     |
          v                     v
   Role Requirements       JD Skill Extraction
          |                     |
          +----------+----------+
                     |
                     v
              Skill Comparison
                     |
                     v
          Matched / Missing Skills
                     |
                     v
             Match Percentage
                     |
          +----------+----------+
          |                     |
          v                     v
    Recommendations       Donut Visualization
          |                     |
          +----------+----------+
                     |
                     v
              Save Analysis
                     |
                     v
                   MySQL
                     |
                     v
            Analysis History
```

------------------------------------------------------------------------

# 5. Detailed Application Workflow

## Step 1 -- Resume Upload

The user uploads a resume in PDF format through Streamlit's file
uploader.

The application accepts:

``` text
PDF
```

The uploaded file is processed using the `pypdf` library.

The relevant module is:

``` python
from pypdf import PdfReader
```

The application reads each page and extracts its text.

Conceptually:

``` text
PDF
 |
 | pypdf
 v
Extracted Text
```

------------------------------------------------------------------------

# 6. PDF Text Extraction

The application creates a `PdfReader` object from the uploaded file.

Each page is processed and its text is added to a single string.

The purpose is to convert the visual PDF resume into text that Python
can analyze.

Example:

``` text
Resume PDF
     |
     v
Page 1 → Text
Page 2 → Text
Page 3 → Text
     |
     v
Complete Resume Text
```

This extracted text is then passed to the skill detector.

------------------------------------------------------------------------

# 7. Skill Detection

The skill detection module is:

``` text
utils/skill_detector.py
```

The application maintains a predefined list of technical skills.

The list contains categories such as:

### Programming

-   Python
-   Java
-   C
-   C++
-   SQL
-   JavaScript
-   TypeScript

### Databases

-   MySQL
-   PostgreSQL
-   MongoDB
-   Oracle
-   SQL Server

### Data Analytics

-   Excel
-   Power BI
-   Tableau
-   Pandas
-   NumPy
-   Matplotlib

### Machine Learning / AI

-   Scikit-learn
-   Machine Learning
-   Deep Learning
-   Generative AI
-   NLP

### Web Development

-   HTML
-   CSS
-   React
-   Node.js
-   Express
-   Flask
-   FastAPI
-   Spring Boot
-   REST API

### Cloud / DevOps

-   AWS
-   Azure
-   Docker
-   Kubernetes
-   Git
-   GitHub
-   Linux

### Big Data

-   PySpark
-   Spark
-   Hadoop
-   Databricks
-   Kafka

### Computer Science

-   Data Structures
-   Algorithms
-   OOP
-   DBMS
-   Operating Systems
-   Computer Networks

### Blockchain

-   Blockchain
-   Solidity
-   Ethereum
-   Smart Contracts
-   Web3.js

The detector converts the resume text to lowercase and checks whether
each known skill occurs in the text.

Conceptually:

``` text
Resume Text
     |
     v
Convert to lowercase
     |
     v
Compare against SKILLS list
     |
     v
Detected Skills
```

For example, if a resume contains:

``` text
Python, SQL, Pandas, MySQL and Power BI
```

the system may detect:

``` text
Python
SQL
Pandas
MySQL
Power BI
```

------------------------------------------------------------------------

# 8. Role Library

ResumeFit contains a JSON-based role library.

The file is:

``` text
data/roles.json
```

It contains predefined entry-level roles and their required skills.

Examples include:

-   Software Engineer
-   Python Developer
-   Java Developer
-   Full Stack Developer
-   Frontend Developer
-   Backend Developer
-   Web Developer
-   Data Analyst
-   Business Analyst
-   Data Scientist
-   Data Engineer
-   Machine Learning Engineer
-   AI Engineer
-   NLP Engineer
-   Database Developer
-   Database Administrator
-   Cloud Engineer
-   AWS Cloud Engineer
-   DevOps Engineer
-   Cybersecurity Analyst
-   Network Engineer
-   Software Tester
-   QA Engineer
-   Automation Test Engineer
-   Mobile App Developer
-   UI/UX Designer
-   Product Analyst
-   Marketing Analyst
-   Operations Analyst
-   Cloud Data Engineer
-   Generative AI Engineer

This makes the role requirements configurable without hard-coding them
directly into the main application.

------------------------------------------------------------------------

# 9. Role Loading

The module:

``` text
utils/role_loader.py
```

loads the role definitions from:

``` text
data/roles.json
```

The JSON file is converted into a Python dictionary.

Conceptually:

``` text
roles.json
    |
    v
role_loader.py
    |
    v
Python Dictionary
```

This dictionary is then passed to the role-matching module.

------------------------------------------------------------------------

# 10. Role Matching

The module:

``` text
utils/role_matcher.py
```

compares the skills detected from the resume with the skills required by
each role.

The basic formula is:

``` text
Match Percentage =
(Number of Matched Required Skills /
 Number of Required Skills) × 100
```

For example, suppose a role requires:

``` text
Python
SQL
Pandas
Power BI
Excel
```

and the resume contains:

``` text
Python
SQL
Pandas
Excel
```

Then:

``` text
Matched Skills = 4
Required Skills = 5

Match = (4 / 5) × 100

Match = 80%
```

The system performs this comparison for all predefined roles.

------------------------------------------------------------------------

# 11. Automatic Career Recommendations

After calculating the match percentage for every role, the results are
sorted in descending order.

Example:

``` text
1. Python Developer       → 85%
2. Data Analyst            → 80%
3. Software Engineer       → 75%
4. Data Engineer           → 68%
5. ML Engineer             → 60%
```

The top roles are displayed as recommended career matches.

This helps the user understand which predefined roles are closest to
their current skill set.

------------------------------------------------------------------------

# 12. Resume Score

ResumeFit also calculates an overall resume score out of 100.

The scoring system is rule-based.

The score considers:

``` text
Skill Match             → 40%
Role Match              → 25%
Projects                → 15%
Resume Completeness     → 10%
Content Quality         → 10%
```

Therefore:

``` text
Overall Score =
Skill Score
+ Role Score
+ Project Score
+ Completeness Score
+ Content Quality Score
```

The result is displayed as:

``` text
Resume Score: XX / 100
```

## Important Interview Point

This is a **custom rule-based scoring system**, not an official ATS
score.

If the interviewer asks whether it is a real ATS score, explain:

> "No. It is a rule-based score that I designed for my project to
> provide a simple and transparent assessment of the resume."

------------------------------------------------------------------------

# 13. Resume Completeness

The scoring module checks for important resume sections such as:

``` text
Education
Skills
Experience
Projects
Certifications
```

The more expected sections that are found in the extracted resume text,
the higher the completeness component.

------------------------------------------------------------------------

# 14. Project and Content Quality Checks

The scoring logic also checks for project-related and action-oriented
keywords.

Examples include:

``` text
developed
built
implemented
designed
created
analyzed
optimized
achieved
```

This gives the application a simple way of evaluating whether the resume
contains meaningful project and achievement-oriented content.

------------------------------------------------------------------------

# 15. Target Role Analysis

After automatic role recommendations, the user can select a specific
predefined role.

For example:

``` text
Python Developer
```

The application then displays:

-   Current match percentage
-   Skills already present
-   Missing skills
-   Recommendations
-   Skill coverage visualization

This allows the user to analyze a specific career target rather than
relying only on automatic recommendations.

------------------------------------------------------------------------

# 16. Job Description Mode

One of the important features of ResumeFit is the **Job Description
mode**.

This is useful when a company posts a job that does not exactly match
one of the predefined roles.

The user can paste the company's job description directly into the
application.

Example:

``` text
We are looking for a Blockchain Developer with
experience in Solidity, Ethereum, Smart Contracts,
Web3.js and JavaScript.
```

The application then extracts known technical skills from the job
description.

------------------------------------------------------------------------

# 17. JD Skill Extraction

The module:

``` text
utils/jd_skill_extractor.py
```

uses the same centralized `SKILLS` list used by the resume skill
detector.

Conceptually:

``` text
Job Description
       |
       v
Convert to lowercase
       |
       v
Compare with SKILLS list
       |
       v
Required JD Skills
```

For the example JD, the system can detect:

``` text
Blockchain
Solidity
Ethereum
Smart Contracts
Web3.js
JavaScript
```

This keeps the resume and JD analysis consistent.

------------------------------------------------------------------------

# 18. Resume vs Job Description Comparison

After extracting the skills from the JD, the application compares them
with the skills already detected from the resume.

For example:

### Job Description Skills

``` text
Blockchain
Solidity
Ethereum
Smart Contracts
Web3.js
JavaScript
```

### Resume Skills

``` text
Python
SQL
JavaScript
Pandas
MySQL
```

### Matched Skills

``` text
JavaScript
```

### Missing Skills

``` text
Blockchain
Solidity
Ethereum
Smart Contracts
Web3.js
```

The JD match percentage is calculated as:

``` text
Matched JD Skills
----------------- × 100
Total JD Skills
```

------------------------------------------------------------------------

# 19. Skill Gap Analysis

The main purpose of ResumeFit is identifying the skill gap.

The application separates the required skills into:

``` text
Skills You Have
```

and

``` text
Skills Missing
```

The UI displays them separately.

For example:

``` text
Skills You Have
✓ Python
✓ SQL
✓ JavaScript
```

and:

``` text
Skills to Improve
• Docker
• AWS
• Kubernetes
```

This makes the result easy for a student or fresher to understand.

------------------------------------------------------------------------

# 20. Recommendation Engine

The module:

``` text
utils/recommendation_engine.py
```

generates learning recommendations based on the missing skills.

The basic flow is:

``` text
Missing Skills
      |
      v
Recommendation Engine
      |
      v
Learning Suggestions
```

For example:

``` text
Missing Skill → Docker

Recommendation:
Learn Docker fundamentals, images,
containers and basic deployment workflows.
```

The purpose is not to automatically teach the complete technology, but
to tell the user what areas should be improved.

------------------------------------------------------------------------

# 21. Skill Coverage Visualization

ResumeFit uses Matplotlib to create a donut chart.

The chart represents:

``` text
Skills You Have
+
Skills Missing
```

For example:

``` text
Required Skills = 10

Skills You Have = 7
Skills Missing = 3
```

The visualization represents:

``` text
70% → Skills You Have
30% → Skills Missing
```

This gives the user a quick visual understanding of their readiness for
the selected role or job description.

------------------------------------------------------------------------

# 22. MySQL Database Integration

ResumeFit uses MySQL to store completed analysis results.

The database contains an:

``` text
analysis_history
```

table.

Important fields include:

``` text
id
resume_name
selected_role
resume_score
role_match
matched_skills
missing_skills
created_at
```

The flow is:

``` text
Analysis Result
      |
      v
save_analysis()
      |
      v
MySQL
      |
      v
analysis_history
```

------------------------------------------------------------------------

# 23. Saving Analysis

When the user clicks:

``` text
Save Analysis to MySQL
```

the application stores:

-   Resume name
-   Selected role
-   Resume score
-   Role match
-   Matched skills
-   Missing skills
-   Timestamp

This allows previous analyses to be reviewed later.

------------------------------------------------------------------------

# 24. Analysis History

The application can retrieve previous records from MySQL.

The module:

``` text
utils/database.py
```

contains the database operations.

The flow is:

``` text
MySQL
   |
   v
get_analysis_history()
   |
   v
Previous Analyses
   |
   v
Streamlit Data Table
```

This provides a basic history of the user's previous resume analyses.

------------------------------------------------------------------------

# 25. Project File Structure

The main project structure is:

``` text
ResumeFit/
│
├── app.py
│
├── requirements.txt
│
├── data/
│   └── roles.json
│
├── utils/
│   ├── skill_detector.py
│   ├── jd_skill_extractor.py
│   ├── role_loader.py
│   ├── role_matcher.py
│   ├── resume_scorer.py
│   ├── recommendation_engine.py
│   └── database.py
│
├── database/
│
├── resumes/
│
└── assets/
```

------------------------------------------------------------------------

# 26. Purpose of Each Important File

## `app.py`

This is the main application file.

It handles:

-   Streamlit UI
-   PDF upload
-   Resume processing
-   Skill detection
-   Role selection
-   JD mode
-   Resume scoring
-   Recommendations
-   Visualization
-   MySQL interaction
-   Analysis history

------------------------------------------------------------------------

## `skill_detector.py`

Responsible for detecting known technical skills from resume text.

Input:

``` text
Resume Text
```

Output:

``` text
Detected Skills
```

------------------------------------------------------------------------

## `jd_skill_extractor.py`

Responsible for extracting known technical skills from a job
description.

Input:

``` text
Job Description
```

Output:

``` text
Required JD Skills
```

------------------------------------------------------------------------

## `role_loader.py`

Loads role requirements from:

``` text
data/roles.json
```

------------------------------------------------------------------------

## `role_matcher.py`

Compares:

``` text
Resume Skills
        vs
Role Required Skills
```

and calculates the match percentage.

------------------------------------------------------------------------

## `resume_scorer.py`

Calculates the rule-based resume score.

It evaluates:

-   Skill match
-   Role match
-   Projects
-   Completeness
-   Content quality

------------------------------------------------------------------------

## `recommendation_engine.py`

Generates recommendations based on missing skills.

------------------------------------------------------------------------

## `database.py`

Handles MySQL operations.

Main operations:

``` text
save_analysis()
get_analysis_history()
```

------------------------------------------------------------------------

## `roles.json`

Stores predefined job roles and their required skills.

------------------------------------------------------------------------

# 27. Technologies Used and Why

## Python

Python is the main programming language.

It is used for:

-   PDF processing
-   Skill detection
-   Matching algorithms
-   Scoring
-   Recommendations
-   Database interaction

------------------------------------------------------------------------

## Streamlit

Streamlit is used to create the web interface.

I selected Streamlit because:

-   It is simple to develop with Python.
-   It requires less frontend code.
-   It is suitable for data-oriented applications.
-   It supports file uploads and interactive UI components.
-   It can be deployed easily using Streamlit Community Cloud.

------------------------------------------------------------------------

## pypdf

Used to extract text from PDF resumes.

------------------------------------------------------------------------

## Pandas

Used as part of the Python data-processing ecosystem and for
displaying/working with structured analysis data where required.

------------------------------------------------------------------------

## Matplotlib

Used to create the skill coverage donut chart.

------------------------------------------------------------------------

## JSON

JSON is used to store predefined job roles and their required skills.

This makes the role library easy to maintain.

------------------------------------------------------------------------

## MySQL

MySQL is used for persistent storage of analysis history.

It demonstrates database integration in the project.

------------------------------------------------------------------------

## Git and GitHub

Git is used for version control.

GitHub is used to store and manage the source code.

------------------------------------------------------------------------

## Streamlit Community Cloud

Used to deploy the application so it can be accessed through a web
browser.

------------------------------------------------------------------------

# 28. Important Algorithms Used

## Algorithm 1 -- Skill Detection

``` text
Input: Resume Text

1. Convert text to lowercase.
2. Iterate through the predefined SKILLS list.
3. Check whether each skill exists in the text.
4. If found, add it to detected skills.
5. Return detected skills.
```

------------------------------------------------------------------------

## Algorithm 2 -- Role Matching

``` text
Input:
Resume Skills
Role Required Skills

1. Convert both skill sets to lowercase.
2. Find the intersection.
3. Count matched skills.
4. Count total required skills.
5. Calculate percentage.
6. Return matched and missing skills.
```

Formula:

``` text
Match % =
Matched Skills / Required Skills × 100
```

------------------------------------------------------------------------

## Algorithm 3 -- JD Matching

``` text
Input:
Resume Skills
Job Description

1. Extract known skills from the JD.
2. Convert resume skills to lowercase.
3. Compare JD skills against resume skills.
4. Separate matched and missing skills.
5. Calculate match percentage.
6. Display results.
```

------------------------------------------------------------------------

## Algorithm 4 -- Resume Scoring

``` text
Skill Match        → 40%
Role Match         → 25%
Projects           → 15%
Completeness       → 10%
Content Quality    → 10%

Total               → 100%
```

------------------------------------------------------------------------

# 29. Complete End-to-End Example

Suppose the user uploads:

``` text
Harsha_Resume.pdf
```

The resume contains:

``` text
Python
SQL
Pandas
MySQL
Power BI
Git
```

### Step 1

PDF is uploaded.

### Step 2

`pypdf` extracts the resume text.

### Step 3

`skill_detector.py` detects:

``` text
Python
SQL
Pandas
MySQL
Power BI
Git
```

### Step 4

ResumeFit compares these skills against all predefined roles.

Example:

``` text
Data Analyst → 75%
Python Developer → 60%
Data Engineer → 40%
```

### Step 5

The user selects:

``` text
Data Analyst
```

### Step 6

The application shows:

``` text
Match: 75%
```

### Step 7

The application separates:

``` text
Skills You Have
✓ Python
✓ SQL
✓ Pandas
✓ MySQL
✓ Power BI
```

and:

``` text
Skills Missing
• Excel
• Tableau
```

### Step 8

The recommendation engine suggests improving:

``` text
Excel
Tableau
```

### Step 9

The donut chart displays the skill coverage.

### Step 10

The user saves the analysis.

### Step 11

The result is inserted into MySQL.

### Step 12

The result becomes available in Analysis History.

------------------------------------------------------------------------

# 30. Job Description Example

Suppose a company provides:

``` text
We are looking for a Blockchain Developer with
experience in Solidity, Ethereum, Smart Contracts,
Web3.js and JavaScript.
```

The user selects:

``` text
Job Description
```

The application extracts:

``` text
Blockchain
Solidity
Ethereum
Smart Contracts
Web3.js
JavaScript
```

Suppose the resume contains:

``` text
JavaScript
Python
SQL
```

Then:

``` text
Matched:
JavaScript

Missing:
Blockchain
Solidity
Ethereum
Smart Contracts
Web3.js
```

The application calculates the JD match percentage and displays the
skill gap and recommendations.

------------------------------------------------------------------------

# 31. Why I Used Keyword Matching

The current version uses keyword matching instead of an external NLP or
LLM service.

Reasons:

1.  It is simple.
2.  It is fast.
3.  It is easy to understand.
4.  It does not require an external API.
5.  It does not require an API key.
6.  Results are deterministic.
7.  It is suitable for a student-level prototype.

The system is therefore intentionally designed as a **rule-based resume
analysis system**.

------------------------------------------------------------------------

# 32. Limitations

The current implementation has some limitations.

### 1. Keyword Dependency

If a resume uses a different term or synonym that is not present in the
skill dictionary, the application may not detect it.

For example:

``` text
ML
```

may not always be treated the same as:

``` text
Machine Learning
```

unless that mapping is explicitly implemented.

### 2. Context Understanding

The system checks whether a skill appears in text.

It does not deeply understand the context in which the skill was
mentioned.

### 3. No Advanced NLP

The current system does not use:

-   Large Language Models
-   Embeddings
-   Semantic similarity
-   Advanced NLP pipelines

### 4. Rule-Based Resume Score

The resume score is a custom project score, not an official ATS score.

### 5. PDF Extraction Dependency

Image-only or scanned PDFs may not provide usable text through normal
PDF text extraction.

------------------------------------------------------------------------

# 33. Future Enhancements

Possible future improvements include:

1.  NLP-based semantic skill extraction.
2.  LLM-based job description analysis.
3.  Synonym detection.
4.  Better resume section classification.
5.  More advanced ATS-style analysis.
6.  Skill importance weighting.
7.  Personalized learning roadmaps.
8.  More detailed resume improvement suggestions.
9.  Cloud-hosted database for production deployment.
10. User authentication and profiles.
11. Resume version comparison.
12. Historical score improvement charts.
13. Support for DOCX resumes.
14. Support for multiple resumes.
15. Better handling of scanned PDFs using OCR.

------------------------------------------------------------------------

# 34. Architecture Explanation for Interview

If the interviewer asks:

**"Explain the architecture of your project."**

Say:

> "ResumeFit follows a modular application architecture. The Streamlit
> application acts as the presentation and orchestration layer. When a
> user uploads a PDF resume, pypdf extracts the text. The skill detector
> identifies technical skills using a predefined skill dictionary. For
> predefined roles, the role loader reads role requirements from a JSON
> file and the role matcher calculates skill overlap and match
> percentage. For a custom job description, a separate JD skill
> extractor identifies known technical skills and compares them with the
> resume. The resume scorer calculates a rule-based score, while the
> recommendation engine generates suggestions for missing skills.
> Matplotlib is used for skill coverage visualization. Finally, selected
> analysis results can be stored in MySQL and retrieved as analysis
> history."

------------------------------------------------------------------------

# 35. One-Minute Project Explanation

If the interviewer says:

**"Explain your project briefly."**

Use this:

> "My project is ResumeFit, a Resume Analysis and Skill Gap System built
> using Python and Streamlit. The main goal is to help freshers
> understand how well their resume matches different job roles. The user
> uploads a PDF resume, and I use pypdf to extract the text. I then
> detect technical skills using a predefined skill dictionary and
> compare them with the requirements of more than 30 predefined
> entry-level roles. The system calculates match percentages, identifies
> matched and missing skills, generates learning recommendations, and
> calculates a rule-based resume score. I also implemented a Job
> Description mode where users can paste a specific job description and
> the system extracts known skills and compares them with the resume.
> Matplotlib is used for skill coverage visualization, and MySQL stores
> analysis history. The application is deployed using Streamlit
> Community Cloud."

------------------------------------------------------------------------

# 36. 30-Second Project Explanation

> "ResumeFit is a Streamlit-based resume analysis application. It
> extracts text from uploaded PDF resumes, detects technical skills
> using keyword matching, and compares those skills with predefined job
> roles or a custom job description. It shows the match percentage,
> missing skills, recommendations, and a rule-based resume score. I also
> integrated MySQL to store analysis history and deployed the
> application using Streamlit Community Cloud."

------------------------------------------------------------------------

# 37. If Asked "What Was Your Contribution?"

A suitable answer is:

> "I worked on the complete application flow, including PDF processing,
> skill detection, role matching, resume scoring, skill-gap analysis,
> recommendation generation, Job Description analysis, visualization,
> MySQL integration, and deployment. I also structured the application
> into separate utility modules so that different responsibilities such
> as skill detection, role matching, scoring, recommendations, and
> database operations were separated."

------------------------------------------------------------------------

# 38. If Asked "What Was the Most Challenging Part?"

A good answer is:

> "One challenging part was designing the matching logic so that the
> resume skills and role requirements could be compared consistently. I
> solved this by normalizing the skills to lowercase and using set-based
> comparison to identify common and missing skills. Another challenge
> was supporting custom job descriptions, because the application needed
> to extract relevant known skills from arbitrary text. I solved that
> using a centralized skill dictionary and a separate JD skill
> extraction module."

------------------------------------------------------------------------

# 39. If Asked "Why Did You Use Sets?"

Answer:

> "I used set-based comparison because I only need to determine which
> skills are common between the resume and the required skill list. Set
> intersection provides a simple and efficient way to find matched
> skills, while set difference can be used to identify missing skills."

Example:

``` python
resume_skills = {"Python", "SQL", "Pandas"}

required_skills = {
    "Python",
    "SQL",
    "Power BI"
}

matched = resume_skills.intersection(
    required_skills
)

missing = required_skills.difference(
    resume_skills
)
```

------------------------------------------------------------------------

# 40. If Asked "Why Did You Use JSON?"

Answer:

> "I used JSON to store predefined job roles and their required skills
> separately from the application logic. This makes the role library
> easier to update without changing the main Python application."

------------------------------------------------------------------------

# 41. If Asked "Why MySQL?"

Answer:

> "I used MySQL because the project needed persistent storage for
> analysis history. It also allowed me to demonstrate practical database
> integration rather than keeping the results only in application
> memory."

------------------------------------------------------------------------

# 42. If Asked "Why Streamlit?"

Answer:

> "I selected Streamlit because the project is primarily a Python-based
> data application. Streamlit allowed me to build the interface quickly
> without developing a separate frontend and backend, while still
> providing interactive components such as file uploaders, select boxes,
> buttons, metrics, charts, and tables."

------------------------------------------------------------------------

# 43. If Asked "Is This an AI Project?"

Be precise.

Say:

> "The project is mainly a rule-based resume analysis system rather than
> an LLM-based AI application. It uses Python-based skill detection,
> matching algorithms, scoring rules, and recommendations. The design
> can be extended later with NLP or LLM techniques for semantic skill
> extraction."

Do not claim that the current ResumeFit version uses an LLM if you did
not implement one.

------------------------------------------------------------------------

# 44. If Asked "Is Your Resume Score an ATS Score?"

Answer:

> "No. It is a custom rule-based score that I designed for the project.
> It combines skill match, role match, projects, resume completeness,
> and content quality. It is intended as an assessment indicator rather
> than an official ATS score."

------------------------------------------------------------------------

# 45. If Asked "What Happens When a Skill Is Missing?"

Answer:

> "The role matcher identifies the missing skills by comparing the
> required role skills against the skills detected in the resume. Those
> missing skills are passed to the recommendation engine, which
> generates learning suggestions. They are also displayed in the
> skill-gap section and represented in the donut chart."

------------------------------------------------------------------------

# 46. If Asked "What Happens If the Role Is Not in Your Role Library?"

Answer:

> "I implemented a Job Description mode for that case. The user can
> paste the job description, and ResumeFit extracts known technical
> skills from it using the same centralized skill dictionary. Those
> skills are then compared with the resume skills."

------------------------------------------------------------------------

# 47. If Asked "How Does the Job Description Feature Work?"

Answer:

> "The user pastes a job description into the application. The JD skill
> extractor converts the text to lowercase and checks it against the
> predefined technical skill dictionary. The detected JD skills are then
> compared with the skills extracted from the resume. From that
> comparison, the application calculates the match percentage and
> displays matched skills, missing skills, recommendations, and the
> visualization."

------------------------------------------------------------------------

# 48. If Asked "How Do You Calculate Match Percentage?"

Answer:

> "I calculate the percentage based on the number of required skills
> that are present in the resume. The formula is matched required skills
> divided by total required skills multiplied by 100."

``` text
Match % =
Matched Required Skills
----------------------- × 100
Total Required Skills
```

------------------------------------------------------------------------

# 49. If Asked "How Does MySQL Work in Your Project?"

Answer:

> "After the user completes an analysis, they can save it to MySQL. The
> database module inserts the resume name, selected role, resume score,
> role match, matched skills, missing skills, and timestamp into the
> analysis history table. The application can later retrieve those
> records and display them in the Analysis History section."

------------------------------------------------------------------------

# 50. Final Interview Flow to Remember

Remember this simple sequence:

``` text
UPLOAD
   ↓
EXTRACT
   ↓
DETECT SKILLS
   ↓
CHOOSE ROLE / JD
   ↓
COMPARE SKILLS
   ↓
CALCULATE MATCH
   ↓
IDENTIFY SKILL GAP
   ↓
GENERATE RECOMMENDATIONS
   ↓
CALCULATE RESUME SCORE
   ↓
VISUALIZE
   ↓
SAVE TO MYSQL
   ↓
VIEW HISTORY
```

If you remember this flow, you can reconstruct most of the project
explanation during an interview.

------------------------------------------------------------------------

# 51. Golden Rule During the Interview

Do not claim features that are not actually implemented.

For the current ResumeFit implementation, confidently discuss:

-   PDF text extraction
-   Keyword-based skill detection
-   Predefined role matching
-   Custom Job Description analysis
-   Rule-based resume scoring
-   Skill-gap analysis
-   Recommendation engine
-   Matplotlib donut visualization
-   MySQL analysis history
-   Git/GitHub
-   Streamlit deployment

If asked about advanced NLP, semantic matching, LLMs, or OCR, explain
them as **future enhancements**, unless they are actually added to the
project.

------------------------------------------------------------------------

# 52. Short Project Summary

**Project Name:** ResumeFit -- Resume Analysis & Skill Gap System

**Type:** Web-based resume analysis application

**Primary Language:** Python

**Frontend/Application Framework:** Streamlit

**PDF Processing:** pypdf

**Visualization:** Matplotlib

**Database:** MySQL

**Configuration/Data:** JSON

**Version Control:** Git/GitHub

**Deployment:** Streamlit Community Cloud

**Core Approach:** Rule-based skill detection and matching

**Main Output:**

``` text
Resume Skills
+
Recommended Roles
+
Role/JD Match %
+
Resume Score
+
Matched Skills
+
Missing Skills
+
Learning Recommendations
+
Skill Visualization
+
Analysis History
```

This completes the end-to-end explanation of the ResumeFit project.
