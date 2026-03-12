# Final Copilot Prompt -- AI Code Quality Analyzer with Checklist

Act as a senior AI architect and expert Python developer.

Build a complete production-ready **AI Code Quality Analyzer similar to
SonarQube** with a modern dashboard, multi-agent AI architecture, and a
code quality checklist validation system.

The application must analyze a repository, detect code quality issues,
validate against a checklist, and generate a professional report.

Use **Python**, **Streamlit** for the UI, and **LangGraph** to
orchestrate multiple AI agents.

------------------------------------------------------------------------

# Application Overview

Create a web dashboard where the user can:

1.  Enter a **local repository folder path OR a GitHub repository URL**
2.  Click **Run Analysis**
3.  The system scans the repository
4.  AI agents analyze the code
5.  The system evaluates the code against a **Code Quality Checklist**
6.  A professional **SonarQube-style report** is generated
7.  The user can view metrics and checklist results on the dashboard
8.  The user can download the report as **HTML or PDF**

------------------------------------------------------------------------

# Technology Stack

-   Python\
-   Streamlit (UI dashboard)\
-   LangGraph (agent orchestration)\
-   OpenAI API for AI analysis\
-   Matplotlib for charts\
-   GitPython for cloning GitHub repositories

------------------------------------------------------------------------

# Project Structure

    ai-code-quality-agent/

    app.py

    scanner/
      repo_scanner.py
      github_scanner.py

    agents/
      bug_agent.py
      security_agent.py
      performance_agent.py
      maintainability_agent.py

    workflow/
      langgraph_workflow.py

    reports/
      report_generator.py
      report_templates.py

    prompts/
      prompts.py

    utils/
      metrics_calculator.py
      checklist_evaluator.py

    requirements.txt

------------------------------------------------------------------------

# Feature 1 -- Repository Input

User can choose:

-   Local Folder Path
-   GitHub Repository URL

If GitHub URL is provided, clone the repository locally using
**GitPython**.

------------------------------------------------------------------------

# Feature 2 -- Repository Scanner

Recursively scan source code files.

Supported languages:

-   Python (.py)
-   Java (.java)
-   JavaScript (.js)
-   TypeScript (.ts)
-   Go (.go)
-   C# (.cs)

Example format:

FILE: src/service/UserService.java `<code>`{=html}

FILE: src/controller/UserController.java `<code>`{=html}

------------------------------------------------------------------------

# Feature 3 -- Multi-Agent AI Analysis

Use **LangGraph** to orchestrate multiple AI agents.

Agents include:

-   **Bug Detection Agent** -- Detect logic bugs and runtime risks
-   **Security Agent** -- Detect vulnerabilities such as hardcoded
    credentials and injection risks
-   **Performance Agent** -- Detect inefficient loops and heavy
    operations
-   **Maintainability Agent** -- Detect code smells, duplication, long
    methods, and poor naming

Each agent produces **structured findings**.

------------------------------------------------------------------------

# Feature 4 -- LangGraph Workflow

Workflow nodes:

1.  Repository Scanner
2.  Bug Agent
3.  Security Agent
4.  Performance Agent
5.  Maintainability Agent
6.  Checklist Evaluator
7.  Metrics Calculator
8.  Report Generator

The workflow must pass state between nodes.

------------------------------------------------------------------------

# Feature 5 -- Code Quality Checklist

Evaluate the repository against the following **software engineering
checklist**:

1.  Code follows Single Responsibility Principle
2.  Functions or methods are not excessively long
3.  Proper exception handling is implemented
4.  No hardcoded credentials or secrets
5.  Input validation is implemented
6.  Logging exists for critical operations
7.  Code duplication is minimized
8.  Variables and functions use meaningful names
9.  Methods include comments or documentation
10. No obvious performance bottlenecks

Return checklist results in the format:

  Checklist Item   Status   Explanation   Severity
  ---------------- -------- ------------- ----------

Status values:

-   PASS
-   FAIL
-   PARTIAL

Severity levels:

-   Critical
-   High
-   Medium
-   Low

------------------------------------------------------------------------

# Feature 6 -- Metrics Calculator

Generate code quality metrics:

-   Total Bugs
-   Total Vulnerabilities
-   Code Smells
-   Files Analyzed
-   Lines of Code
-   Maintainability Rating (A--E)
-   Security Rating (A--E)
-   Reliability Rating (A--E)
-   Technical Debt Estimate

Also compute:

**Overall Quality Score (0--100)**

Example scoring logic:

Quality Score = 100\
- (Critical Issues × 10)\
- (High Issues × 5)\
- (Medium Issues × 2)\
- (Low Issues × 1)

------------------------------------------------------------------------

# Feature 7 -- Professional Report

Generate a professional **HTML report similar to SonarQube**.

Report sections:

-   Project Overview
-   Code Quality Metrics
-   Checklist Validation Results
-   Detailed Issue Table
-   Security Analysis
-   Performance Analysis
-   Maintainability Observations
-   Code Improvement Suggestions
-   Final Quality Score

Include:

**QUALITY GATE: PASSED / FAILED**

Example rule:

Fail if: - Critical issues \> 0 - OR Quality Score \< 70

------------------------------------------------------------------------

# Feature 8 -- Dashboard UI (Streamlit)

Dashboard title:

**AI Code Quality Analyzer**

Inputs:

-   Repository Folder Path
-   GitHub Repository URL

Buttons:

-   Run Analysis

Display:

-   Quality Score
-   Quality Gate Status
-   Metrics Summary
-   Charts for issue distribution
-   Checklist Validation Table

------------------------------------------------------------------------

# Feature 9 -- Report Download

Save reports inside a **reports** directory.

Allow downloading:

-   HTML report
-   PDF report

------------------------------------------------------------------------

# Feature 10 -- Error Handling

Handle:

-   Invalid repository path
-   Empty repository
-   Unsupported file types
-   Git clone failures

------------------------------------------------------------------------

# Output Format

Provide **FULL WORKING CODE** for all files in the project.

Display each file separately like:

### app.py

`<code>`{=html}

### repo_scanner.py

`<code>`{=html}

### bug_agent.py

`<code>`{=html}

etc.

The project must run after installing dependencies and executing:

    streamlit run app.py

Ensure the UI looks **clean and professional**, similar to a lightweight
SonarQube dashboard.
