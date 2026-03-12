# Functional Testing Plan - Workplace Access System

## Test Environment
- **Frontend:** http://localhost:5173
- **Backend API:** http://localhost:8000
- **API Docs:** http://localhost:8000/docs

## Test Users

| Username | Password    | Role     | Purpose                           |
|----------|-------------|----------|-----------------------------------|
| admin    | admin123    | Admin    | Full system access                |
| approver | approver123 | Approver | Approval workflow testing         |
| employee | employee123 | Employee | Request creation & viewing        |

---

## Test Suite 1: Authentication & Authorization

### TC-1.1: Login with Valid Credentials
**Priority:** High  
**Steps:**
1. Navigate to http://localhost:5173
2. Enter username: `employee`, password: `employee123`
3. Click "Sign In"

**Expected Result:**
- Redirected to Dashboard
- Welcome message shows user's name
- Navigation menu displays

**Status:** [ ] Pass [ ] Fail

---

### TC-1.2: Login with Invalid Credentials
**Priority:** High  
**Steps:**
1. Navigate to login page
2. Enter username: `invalid`, password: `wrong123`
3. Click "Sign In"

**Expected Result:**
- Error message: "Login failed. Please try again."
- User remains on login page
- No redirect occurs

**Status:** [ ] Pass [ ] Fail

---

### TC-1.3: Role-Based Access - Employee
**Priority:** High  
**Steps:**
1. Login as `employee`
2. Check navigation menu items

**Expected Result:**
- See: Dashboard, My Requests, New Request
- NOT see: Admin Panel
- Can only view own requests

**Status:** [ ] Pass [ ] Fail

---

### TC-1.4: Role-Based Access - Approver
**Priority:** High  
**Steps:**
1. Login as `approver`
2. Navigate to Requests page
3. Check available actions

**Expected Result:**
- Can view all requests
- Can update request status
- Can approve/reject requests
- Cannot see Admin Panel

**Status:** [ ] Pass [ ] Fail

---

### TC-1.5: Role-Based Access - Admin
**Priority:** High  
**Steps:**
1. Login as `admin`
2. Check navigation menu

**Expected Result:**
- See all menu items including Admin Panel
- Can manage users and request types
- Has full system access

**Status:** [ ] Pass [ ] Fail

---

### TC-1.6: Logout Functionality
**Priority:** Medium  
**Steps:**
1. Login as any user
2. Click logout button

**Expected Result:**
- User logged out
- Redirected to login page
- Cannot access protected pages

**Status:** [ ] Pass [ ] Fail

---

## Test Suite 2: Request Management (Employee)

### TC-2.1: Create New Request - System Access
**Priority:** High  
**Steps:**
1. Login as `employee`
2. Click "New Request"
3. Select Type: "System Access"
4. Enter Title: "VPN Access for Remote Work"
5. Enter Description: "Need VPN access to connect from home"
6. Select Priority: "High"
7. Enter Justification: "Working remotely for project deadline"
8. Click "Submit Request"

**Expected Result:**
- Success message displayed
- Redirected to request details page
- Request status shows "Submitted"
- Expected turnaround time displayed

**Status:** [ ] Pass [ ] Fail

---

### TC-2.2: Create New Request - Equipment
**Priority:** High  
**Steps:**
1. Click "New Request"
2. Select Type: "Equipment Request"
3. Enter Title: "Laptop for Development"
4. Enter Description: "Need high-spec laptop for software development"
5. Select Priority: "Medium"
6. Enter Justification: "Current laptop is outdated"
7. Click "Submit Request"

**Expected Result:**
- Request created successfully
- Status: "Submitted"
- All fields saved correctly

**Status:** [ ] Pass [ ] Fail

---

### TC-2.3: View Own Requests List
**Priority:** High  
**Steps:**
1. Login as `employee`
2. Navigate to "My Requests"

**Expected Result:**
- Table displays all employee's requests
- Shows: ID, Title, Type, Status, Date
- Default sorting by most recent
- Pagination if > 10 requests

**Status:** [ ] Pass [ ] Fail

---

### TC-2.4: View Request Details
**Priority:** High  
**Steps:**
1. From requests list, click on a request
2. View full details

**Expected Result:**
- All request information displayed
- Status badge visible
- Timeline shows status history
- Comments section visible
- Export PDF button available

**Status:** [ ] Pass [ ] Fail

---

### TC-2.5: Search Requests
**Priority:** Medium  
**Steps:**
1. On requests list page
2. Enter search term (e.g., "VPN")
3. Press Enter

**Expected Result:**
- Filtered results show matching requests
- Search works on title and description
- Clear search button available

**Status:** [ ] Pass [ ] Fail

---

### TC-2.6: Filter by Status
**Priority:** Medium  
**Steps:**
1. On requests list page
2. Select Status filter: "Submitted"
3. Apply filter

**Expected Result:**
- Only submitted requests displayed
- Count updates correctly
- Can clear filter

**Status:** [ ] Pass [ ] Fail

---

### TC-2.7: Filter by Priority
**Priority:** Medium  
**Steps:**
1. Select Priority filter: "High"
2. Apply filter

**Expected Result:**
- Only high priority requests shown
- Filter persists on page refresh

**Status:** [ ] Pass [ ] Fail

---

## Test Suite 3: Approval Workflow (Approver)

### TC-3.1: Review Request - Approve
**Priority:** High  
**Steps:**
1. Login as `approver`
2. Navigate to requests list
3. Click on a "Submitted" request
4. Click "Update Status"
5. Select "Under Review"
6. Add comment: "Reviewing request details"
7. Save

**Expected Result:**
- Status changed to "Under Review"
- Comment added to timeline
- Status history updated with timestamp
- Notification (if implemented)

**Status:** [ ] Pass [ ] Fail

---

### TC-3.2: Approve Request
**Priority:** High  
**Steps:**
1. View request in "Under Review" status
2. Click "Update Status"
3. Select "Approved"
4. Add comment: "Request approved by management"
5. Save

**Expected Result:**
- Status: "Approved"
- Comment visible
- Timeline shows progression
- Requester can see updated status

**Status:** [ ] Pass [ ] Fail

---

### TC-3.3: Reject Request
**Priority:** High  
**Steps:**
1. View submitted request
2. Update Status to "Rejected"
3. Add reason: "Insufficient justification"
4. Save

**Expected Result:**
- Status: "Rejected"
- Rejection reason visible
- No further actions available
- Can still add comments

**Status:** [ ] Pass [ ] Fail

---

### TC-3.4: Mark Request as Fulfilled
**Priority:** High  
**Steps:**
1. View "Approved" request
2. Update Status to "Fulfilled"
3. Add comment: "VPN credentials sent via email"
4. Save

**Expected Result:**
- Status: "Fulfilled"
- Comment added
- Can move to "Closed"

**Status:** [ ] Pass [ ] Fail

---

### TC-3.5: Close Request
**Priority:** High  
**Steps:**
1. View "Fulfilled" request
2. Update Status to "Closed"
3. Add final comment
4. Save

**Expected Result:**
- Status: "Closed"
- No further status changes allowed
- Full history preserved

**Status:** [ ] Pass [ ] Fail

---

## Test Suite 4: Comments System

### TC-4.1: Add External Comment
**Priority:** Medium  
**Steps:**
1. View request details
2. Enter comment: "When can I expect access?"
3. Leave "Internal" unchecked
4. Click "Post Comment"

**Expected Result:**
- Comment added to thread
- Visible to all users
- Shows author and timestamp
- "External" tag visible

**Status:** [ ] Pass [ ] Fail

---

### TC-4.2: Add Internal Comment (Approver/Admin)
**Priority:** Medium  
**Steps:**
1. Login as `approver`
2. View request
3. Add comment with "Internal" checked
4. Post

**Expected Result:**
- Comment added
- Tagged as "Internal"
- Only visible to approvers/admins
- Not visible to employee

**Status:** [ ] Pass [ ] Fail

---

### TC-4.3: Comment Visibility - Employee
**Priority:** Medium  
**Steps:**
1. Login as `employee`
2. View request with internal comments

**Expected Result:**
- See only external comments
- Internal comments hidden
- Can add external comments only

**Status:** [ ] Pass [ ] Fail

---

## Test Suite 5: Export Functionality

### TC-5.1: Export Single Request as PDF
**Priority:** Medium  
**Steps:**
1. View request details
2. Click "Export PDF" button
3. Wait for download

**Expected Result:**
- PDF downloads successfully
- Contains all request details
- Properly formatted
- Includes status history
- Includes comments

**Status:** [ ] Pass [ ] Fail

---

### TC-5.2: Export Requests as CSV
**Priority:** Medium  
**Steps:**
1. On requests list page
2. Click "Export" dropdown
3. Select "CSV"

**Expected Result:**
- CSV file downloads
- Contains all visible requests
- All columns included
- Proper formatting

**Status:** [ ] Pass [ ] Fail

---

### TC-5.3: Export Requests as Excel
**Priority:** Medium  
**Steps:**
1. Click Export > "Excel"

**Expected Result:**
- Excel file (.xlsx) downloads
- Data properly formatted
- Headers included
- Opens in Excel/Sheets

**Status:** [ ] Pass [ ] Fail

---

## Test Suite 6: Admin Functions

### TC-6.1: Manage Request Types
**Priority:** High  
**Steps:**
1. Login as `admin`
2. Navigate to Admin Panel
3. View Request Types
4. Create new type

**Expected Result:**
- Can view all request types
- Can add new types
- Can set turnaround times
- Can enable/disable types

**Status:** [ ] Pass [ ] Fail

---

### TC-6.2: Manage Users (if implemented)
**Priority:** Medium  
**Steps:**
1. Navigate to Users management
2. View user list
3. Modify user role

**Expected Result:**
- Can view all users
- Can change roles
- Changes take effect immediately

**Status:** [ ] Pass [ ] Fail

---

## Test Suite 7: UI/UX Validation

### TC-7.1: Responsive Design - Mobile View
**Priority:** Medium  
**Steps:**
1. Resize browser to mobile size (375px)
2. Navigate through pages

**Expected Result:**
- Layout adjusts properly
- All features accessible
- Navigation menu responsive
- No horizontal scroll

**Status:** [ ] Pass [ ] Fail

---

### TC-7.2: Status Badge Colors
**Priority:** Low  
**Steps:**
1. View requests with different statuses

**Expected Result:**
- Submitted: Yellow
- Under Review: Blue
- Approved: Green
- Rejected: Red
- Fulfilled: Purple
- Closed: Gray

**Status:** [ ] Pass [ ] Fail

---

### TC-7.3: Loading States
**Priority:** Low  
**Steps:**
1. Perform actions requiring API calls
2. Observe loading indicators

**Expected Result:**
- Loading spinners display
- Buttons disabled during loading
- No duplicate submissions

**Status:** [ ] Pass [ ] Fail

---

### TC-7.4: Error Handling
**Priority:** Medium  
**Steps:**
1. Stop backend server
2. Try to create request

**Expected Result:**
- User-friendly error message
- No blank screens
- Graceful degradation

**Status:** [ ] Pass [ ] Fail

---

## Test Suite 8: Data Validation

### TC-8.1: Required Fields Validation
**Priority:** High  
**Steps:**
1. Try to submit request without title
2. Try without description

**Expected Result:**
- Validation errors shown
- Cannot submit incomplete form
- Error messages clear

**Status:** [ ] Pass [ ] Fail

---

### TC-8.2: Input Length Limits
**Priority:** Medium  
**Steps:**
1. Enter very long title (>500 chars)
2. Try to submit

**Expected Result:**
- Character limit enforced
- Error message if exceeds
- Database constraints respected

**Status:** [ ] Pass [ ] Fail

---

## Test Suite 9: Performance

### TC-9.1: Page Load Time
**Priority:** Medium  
**Steps:**
1. Measure time from URL entry to full page render
2. Test dashboard, requests list

**Expected Result:**
- Dashboard loads in < 2 seconds
- Request list loads in < 3 seconds
- API responses < 500ms

**Status:** [ ] Pass [ ] Fail

---

### TC-9.2: Pagination Performance
**Priority:** Low  
**Steps:**
1. Navigate through pages with 100+ requests
2. Check load times

**Expected Result:**
- Smooth pagination
- No lag
- Proper data loading

**Status:** [ ] Pass [ ] Fail

---

## Test Suite 10: API Integration

### TC-10.1: API Documentation Access
**Priority:** Low  
**Steps:**
1. Navigate to http://localhost:8000/docs

**Expected Result:**
- Swagger UI loads
- All endpoints documented
- Can test endpoints
- Authentication works

**Status:** [ ] Pass [ ] Fail

---

### TC-10.2: CORS Configuration
**Priority:** Medium  
**Steps:**
1. Frontend makes API calls from localhost:5173
2. Check browser console

**Expected Result:**
- No CORS errors
- API calls successful
- Proper headers set

**Status:** [ ] Pass [ ] Fail

---

## Test Execution Summary

### Test Statistics
- **Total Tests:** 43
- **Passed:** _____
- **Failed:** _____
- **Skipped:** _____
- **Pass Rate:** _____%

### Critical Issues Found
1. 
2. 
3. 

### Medium Issues Found
1. 
2. 
3. 

### Minor Issues Found
1. 
2. 

### Recommendations
1. 
2. 
3. 

---

## Test Sign-off

**Tested By:** _________________  
**Date:** _________________  
**Environment:** Windows / SQLite / Python 3.11+ / Node.js 18+  
**Build Version:** _________________  

**Approval:** _________________
