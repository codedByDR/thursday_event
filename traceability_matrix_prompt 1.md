# Prompt: Generate Requirements Traceability Matrix

## Objective

Analyze the provided project repository and generate a **Requirements
Traceability Matrix (RTM)** that maps requirements to their
implementation and test coverage.

------------------------------------------------------------------------

## Inputs

The AI agent will receive:

-   Source code repository folder
-   README or project documentation
-   API specifications (if available)
-   Test files
-   Requirement documents or user stories (if available)

------------------------------------------------------------------------

## Instructions

### 1. Identify Requirements

Extract all system requirements from:

-   Requirement documents
-   README files
-   User stories
-   API descriptions
-   Code comments

If a requirement ID is not available, create one using this format:

    RQ-001
    RQ-002
    RQ-003

------------------------------------------------------------------------

### 2. Map Requirements to Implementation

For each requirement, identify the corresponding:

-   Module
-   Class
-   API
-   Function
-   Service

Also capture the **file path** where the implementation exists.

------------------------------------------------------------------------

### 3. Map Requirements to Test Cases

Scan test files and identify:

-   Unit tests
-   Integration tests
-   API tests

Map test cases to the related requirement.

If no test case exists, mark as:

    Missing Test

------------------------------------------------------------------------

### 4. Generate Traceability Matrix

Create a matrix with the following columns:

  ------------------------------------------------------------------------------------------
  Requirement   Requirement   Module/API   File Path  Test Case  Implementation   Test
  ID            Description                                      Status           Status
  ------------- ------------- ------------ ---------- ---------- ---------------- ----------

  ------------------------------------------------------------------------------------------

------------------------------------------------------------------------

### 5. Determine Status

**Implementation Status**

-   Implemented
-   Partially Implemented
-   Missing Implementation

**Test Status**

-   Passed
-   Failed
-   Pending
-   Missing Test

------------------------------------------------------------------------

## Output Format

Generate output in **two formats**.

### 1. CSV Format

    Requirement ID,Requirement Description,Module/API,File Path,Test Case,Implementation Status,Test Status
    RQ-001,User login,AuthController,/src/auth/AuthController.java,TC-01,Implemented,Passed
    RQ-002,Create ticket,TicketController,/src/ticket/TicketController.java,TC-02,Implemented,Pending
    RQ-003,Assign ticket,AssignmentService,/src/ticket/AssignmentService.java,NA,Implemented,Missing Test

------------------------------------------------------------------------

### 2. Table Format

  ---------------------------------------------------------------------------------------------------------------------------
  Requirement   Requirement   Module/API          File Path                            Test Case  Implementation   Test
  ID            Description                                                                       Status           Status
  ------------- ------------- ------------------- ------------------------------------ ---------- ---------------- ----------
  RQ-001        User login    AuthController      /src/auth/AuthController.java        TC-01      Implemented      Passed

  RQ-002        Create ticket TicketController    /src/ticket/TicketController.java    TC-02      Implemented      Pending

  RQ-003        Assign ticket AssignmentService   /src/ticket/AssignmentService.java   NA         Implemented      Missing
                                                                                                                   Test
  ---------------------------------------------------------------------------------------------------------------------------

------------------------------------------------------------------------

## Additional Analysis

Provide a summary including:

-   Total number of requirements
-   Number of implemented requirements
-   Requirements without implementation
-   Requirements without test coverage

Example:

    Total Requirements: 10
    Implemented: 8
    Missing Implementation: 2
    Test Coverage: 70%

------------------------------------------------------------------------

## Expected Output Files

    traceability_matrix.csv
    traceability_matrix_report.md
