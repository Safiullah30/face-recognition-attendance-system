# Acceptance Criteria
## Cloud-Based Face Recognition Attendance System

**Date:** 2026-09-30  
**Status:** Day 1 - Requirements Analysis

---

## Overview

Acceptance criteria define measurable conditions that must be met for each major feature to be considered "done." These criteria are testable, specific, and based on the requirements and user stories documented in `requirements.md` and `user-stories.md`.

---

## Feature 1: Student Enrollment

### AC-ENROLL-001: Admin Can Capture Reference Photos
**Given** an admin is logged in to the system  
**When** the admin navigates to the enrollment section  
**Then** the system provides a camera interface to capture student photos

**Acceptance Criteria:**
- [ ] Camera capture interface is available
- [ ] Admin can capture photos from device camera
- [ ] Admin can retake photos if not satisfied
- [ ] At least 2 photos can be captured per enrollment session
- [ ] Photos are displayed for preview before confirmation

### AC-ENROLL-002: System Accepts 2–3 Reference Photos Per Student
**Given** an admin has captured student reference photos  
**When** the admin attempts to save the enrollment  
**Then** the system accepts exactly 2–3 photos

**Acceptance Criteria:**
- [ ] System accepts minimum 2 photos
- [ ] System accepts maximum 3 photos
- [ ] System enforces at least 2 photos (rejects enrollment with <2)
- [ ] System enforces maximum 3 photos (does not accept >3)
- [ ] Clear error message shown if photo count is invalid

### AC-ENROLL-003: System Generates Face Embeddings
**Given** an admin has submitted 2–3 reference photos for enrollment  
**When** the system processes the enrollment  
**Then** face embeddings are generated from the photos

**Acceptance Criteria:**
- [ ] System detects face in each submitted photo
- [ ] System generates numeric embedding for each detected face
- [ ] Embeddings are stored in database (not raw photos, where feasible)
- [ ] Embedding generation completes within 30 seconds per student
- [ ] Duplicate/similar embeddings are handled correctly

### AC-ENROLL-004: Informed Consent Is Recorded
**Given** an admin is enrolling a student  
**When** the enrollment form is submitted  
**Then** the system requires explicit consent to be recorded

**Acceptance Criteria:**
- [ ] Consent form is presented before enrollment is saved
- [ ] Consent form clearly states what data is collected
- [ ] Consent form explains how face data will be used (attendance only)
- [ ] Consent includes option for students <18 to have guardian consent
- [ ] Checkbox or signature mechanism records consent
- [ ] Consent cannot be bypassed; form blocks submission without consent
- [ ] Consent date and administrator name are logged

### AC-ENROLL-005: One-Time Enrollment Per Student Per Campus
**Given** a student is already enrolled at a campus  
**When** an admin attempts to re-enroll the same student  
**Then** the system prevents duplicate enrollment or offers re-enrollment workflow

**Acceptance Criteria:**
- [ ] System checks for existing enrollment before allowing new enrollment
- [ ] If student already enrolled, system displays warning
- [ ] Admin must explicitly confirm re-enrollment if allowed
- [ ] Only one active embedding set per student per campus
- [ ] Enrollment status clearly indicates "Already Enrolled"

### AC-ENROLL-006: Enrollment Data Is Encrypted
**Given** an admin has completed student enrollment  
**When** face embeddings are stored in database  
**Then** embeddings are encrypted

**Acceptance Criteria:**
- [ ] Face embeddings stored with encryption (AES-256 or equivalent)
- [ ] Encryption key is not stored with encrypted data
- [ ] Raw enrollment photos are not retained (if feasible)
- [ ] Database query for embeddings returns decrypted data
- [ ] Encryption/decryption is transparent to API consumers

---

## Feature 2: Teacher Authentication & Dashboard Access

### AC-AUTH-001: Teacher Can Log In
**Given** a teacher is created by admin  
**When** the teacher navigates to login page and enters credentials  
**Then** the teacher gains access to the teacher dashboard

**Acceptance Criteria:**
- [ ] Login form accepts username/email and password
- [ ] Invalid credentials show "Invalid credentials" error (generic, no user enumeration)
- [ ] Valid login redirects to teacher dashboard
- [ ] Session token is created and stored securely
- [ ] Session persists across page refreshes (within inactivity timeout)

### AC-AUTH-002: Teacher Session Expires After Inactivity
**Given** a teacher is logged in  
**When** the teacher is inactive for [TBD: default 30 minutes]  
**Then** the session expires and teacher is logged out

**Acceptance Criteria:**
- [ ] Inactivity timer is configurable (default 30 minutes)
- [ ] Each user action resets the timer
- [ ] Session is invalidated after timeout
- [ ] Page refresh after timeout redirects to login
- [ ] Teacher receives warning before session expiration (optional)

### AC-AUTH-003: Teacher Can Only Access Own-Campus Data
**Given** a teacher is logged in  
**When** the teacher attempts to access any campus data  
**Then** the system only shows data for their assigned campus

**Acceptance Criteria:**
- [ ] Teacher login binds user to single campus
- [ ] All API requests include campus identifier
- [ ] Database queries filter by teacher's campus
- [ ] Teacher cannot change campus via URL or API manipulation
- [ ] Attempting to access another campus's data returns "Unauthorized" error

### AC-AUTH-004: Teacher Can Logout
**Given** a teacher is logged in  
**When** the teacher clicks "Logout" button  
**Then** the session is terminated and teacher is redirected to login page

**Acceptance Criteria:**
- [ ] Logout button is visible on dashboard
- [ ] Logout clears session token
- [ ] Back button after logout does not return to dashboard
- [ ] Cached session data is cleared from browser

---

## Feature 3: Class Selection & Photo Upload

### AC-CLASS-001: Teacher Can Select Class
**Given** a teacher is on the attendance dashboard  
**When** the teacher navigates to start a new attendance session  
**Then** the teacher can select which class to mark attendance for

**Acceptance Criteria:**
- [ ] Dropdown or list of available classes is displayed
- [ ] Each class shows name, class code, and/or section
- [ ] Class list is pre-populated from campus data
- [ ] Teacher can search/filter classes by name
- [ ] Selected class is displayed in the session
- [ ] Class name is included in attendance record

### AC-CLASS-002: Teacher Can Upload Classroom Photo
**Given** a teacher has selected a class  
**When** the teacher attempts to upload a photo  
**Then** the system accepts valid image files (JPEG, PNG)

**Acceptance Criteria:**
- [ ] File upload button is visible
- [ ] Only JPEG and PNG formats are accepted
- [ ] File size limit enforced (e.g., max 10 MB)
- [ ] Unsupported formats show error: "Only JPEG/PNG allowed"
- [ ] File too large shows error: "File exceeds 10 MB limit"
- [ ] Upload progress bar displayed for large files
- [ ] Preview of uploaded photo shown before processing

### AC-CLASS-003: Teacher Can Capture Photo via Camera
**Given** a teacher has selected a class  
**When** the teacher chooses "Capture from Camera" option  
**Then** the system opens camera interface to capture classroom photo

**Acceptance Criteria:**
- [ ] Camera access is requested from browser
- [ ] Live camera preview displayed
- [ ] "Capture" button triggers photo snapshot
- [ ] Captured photo is previewed before processing
- [ ] Teacher can retake photo if unsatisfied
- [ ] Fallback to file upload if camera not available

### AC-CLASS-004: System Processes Photo and Shows Results
**Given** a teacher has uploaded or captured a classroom photo  
**When** the system processes the photo  
**Then** face detection and recognition results are displayed

**Acceptance Criteria:**
- [ ] Processing status displayed ("Analyzing photo...")
- [ ] Processing completes within [TBD: 30-60 seconds]
- [ ] Detected faces shown with bounding boxes on photo
- [ ] Student names and confidence scores displayed for each face
- [ ] Flagged faces (low confidence) visually marked differently
- [ ] Error message if no faces detected
- [ ] Error message if photo quality is poor (e.g., too dark)

---

## Feature 4: Face Detection & Recognition

### AC-FD-001: System Detects Multiple Faces
**Given** a classroom photo with 20–60 students  
**When** the system processes the photo  
**Then** all visible faces are detected

**Acceptance Criteria:**
- [ ] Bounding boxes drawn around each detected face
- [ ] Bounding boxes accurately surround face regions
- [ ] No false positives (non-face objects detected as faces)
- [ ] Faces partially outside frame edges are still detected
- [ ] Detection count displayed to teacher (e.g., "52 faces detected")

### AC-FD-002: System Matches Detected Faces to Enrolled Students
**Given** faces are detected in classroom photo  
**When** face recognition matching is performed  
**Then** each face is matched against enrolled student embeddings

**Acceptance Criteria:**
- [ ] Confidence score computed for each face (0–100 or 0–1 scale)
- [ ] Matched student name displayed with confidence score
- [ ] Top-N matches considered (e.g., top 3 candidates) for ambiguous faces
- [ ] Matching completes within [TBD: 30-60 seconds]
- [ ] Matching accounts for variations (lighting, angle, occlusion)

### AC-FD-003: Confidence Threshold Separates Auto-Marked from Flagged
**Given** face recognition matches have been computed  
**When** confidence scores are evaluated  
**Then** matches above threshold are auto-marked Present, below threshold are flagged

**Acceptance Criteria:**
- [ ] Configurable confidence threshold (default [TBD])
- [ ] Faces with confidence >= threshold marked "Likely Present" (auto-marked)
- [ ] Faces with confidence < threshold marked "Needs Review" (flagged)
- [ ] Flagged faces visually distinguished from auto-marked faces
- [ ] Teacher can adjust threshold before finalizing (optional, TBD)

### AC-FD-004: System Handles Partial Occlusion
**Given** a classroom photo with partially occluded faces (turned faces, coverings)  
**When** face detection is performed  
**Then** partially occluded faces are still detected and recognized

**Acceptance Criteria:**
- [ ] Faces turned at 30–45 degree angles are detected
- [ ] Faces with face coverings (scarves, masks) are detected
- [ ] Confidence score may be lower for occluded faces, but detection occurs
- [ ] Occluded faces are flagged for manual review (lower confidence)
- [ ] System does not incorrectly identify occluded faces as different person

### AC-FD-005: System Recognizes Students Under Varying Lighting
**Given** classroom photos taken under bright or dim lighting  
**When** face recognition is performed  
**Then** students are still recognized across lighting conditions

**Acceptance Criteria:**
- [ ] Recognition works in bright classroom lighting
- [ ] Recognition works in dim/overcast lighting
- [ ] Confidence scores may vary by lighting but recognition still occurs
- [ ] Lighting variations do not cause excessive false negatives
- [ ] [Accuracy metrics to be measured during testing phase]

---

## Feature 5: Manual Override & Correction

### AC-OVERRIDE-001: Teacher Sees Flagged Faces for Review
**Given** low-confidence faces are flagged in attendance session  
**When** teacher reviews the dashboard  
**Then** flagged faces are highlighted and require action

**Acceptance Criteria:**
- [ ] Flagged faces displayed separately or visually marked
- [ ] Each flagged face shows bounding box on photo
- [ ] Confidence score displayed for flagged matches
- [ ] Interface indicates "This face needs your review"
- [ ] Teacher can click on each flagged face to take action

### AC-OVERRIDE-002: Teacher Can Confirm System Match
**Given** a face is flagged for review  
**When** teacher confirms the system's match is correct  
**Then** the student is marked Present and match is logged

**Acceptance Criteria:**
- [ ] "Confirm" button available for flagged faces
- [ ] Clicking "Confirm" marks student as Present
- [ ] Confirmation logged with teacher ID and timestamp
- [ ] Face is removed from flagged list
- [ ] Student name appears in final attendance record

### AC-OVERRIDE-003: Teacher Can Correct Misidentified Face
**Given** a face is incorrectly matched to a student  
**When** teacher corrects the identification  
**Then** the correct student is marked Present and correction is logged

**Acceptance Criteria:**
- [ ] "Correct" button or dropdown available for flagged faces
- [ ] Dropdown shows list of enrolled students (searchable)
- [ ] Teacher selects correct student name
- [ ] System marks correct student as Present
- [ ] Correction logged (original match, correct student, teacher ID, timestamp)
- [ ] Incorrect student is not marked Present

### AC-OVERRIDE-004: Teacher Can Manually Identify Undetected Student
**Given** a student is present in classroom but not detected  
**When** teacher manually marks the student present  
**Then** the student is added to attendance record

**Acceptance Criteria:**
- [ ] "Add Student Manually" button available
- [ ] Dropdown or search to find student by name
- [ ] Student is marked Present after selection
- [ ] Manual addition logged (student name, teacher ID, timestamp)
- [ ] Student appears in final attendance record

### AC-OVERRIDE-005: Teacher Can Change Auto-Marked Student to Absent
**Given** a student is auto-marked Present but is actually absent  
**When** teacher corrects the error  
**Then** the student is marked Absent

**Acceptance Criteria:**
- [ ] "Mark as Absent" button available for auto-marked students
- [ ] Clicking button changes status to Absent
- [ ] Change logged (student name, previous status, new status, teacher ID, timestamp)
- [ ] Student does not appear in final Present attendance record

### AC-OVERRIDE-006: All Corrections Are Logged
**Given** teacher performs any manual correction  
**When** correction is submitted  
**Then** action is logged with full context

**Acceptance Criteria:**
- [ ] Log includes: timestamp, teacher ID, action type, student ID, confidence score (if applicable)
- [ ] Logs are immutable (append-only)
- [ ] Admin can view correction logs in audit trail
- [ ] Corrections are distinguishable from auto-marked attendance

---

## Feature 6: Session Finalization & Attendance Record

### AC-SESSION-001: Teacher Reviews Session Summary Before Finalizing
**Given** teacher has completed all corrections  
**When** teacher views session summary  
**Then** total counts of Present/Absent/Flagged are displayed

**Acceptance Criteria:**
- [ ] Summary shows: Total Present, Total Absent, Total Flagged (if any)
- [ ] List of all students with their status (Present/Absent)
- [ ] No remaining students in "Flagged" status (all reviewed)
- [ ] Teacher can scroll and review full list before finalizing
- [ ] Summary clearly shows which students are marked Present

### AC-SESSION-002: Teacher Can Go Back and Correct Mistakes Before Finalizing
**Given** teacher notices a mistake in session summary  
**When** teacher returns to review step  
**Then** corrections can be made before finalizing

**Acceptance Criteria:**
- [ ] "Edit" or "Go Back" button returns to review screen
- [ ] Corrections made are reflected in summary
- [ ] Teacher can correct multiple students before finalizing
- [ ] Back-and-forth navigation is allowed (no confirmation loops)

### AC-SESSION-003: Teacher Must Confirm Before Finalizing
**Given** teacher is ready to finalize session  
**When** teacher clicks "Finalize" button  
**Then** confirmation dialog is shown

**Acceptance Criteria:**
- [ ] Confirmation dialog states: "Are you sure? This cannot be undone."
- [ ] Dialog shows session details (class, date, number of students)
- [ ] "Confirm" button to proceed with finalization
- [ ] "Cancel" button to return to review
- [ ] Accidental clicks do not finalize session

### AC-SESSION-004: Attendance Record Is Permanently Stored
**Given** teacher confirms finalization  
**When** session is finalized  
**Then** attendance record is saved to database

**Acceptance Criteria:**
- [ ] All student attendance statuses saved (Present/Absent)
- [ ] Confidence scores saved for accuracy analysis
- [ ] Correction logs saved with full context
- [ ] Timestamp of finalization recorded
- [ ] Teacher ID recorded with session
- [ ] Session becomes immutable (read-only) after finalization
- [ ] Success message displayed: "Attendance recorded successfully"

### AC-SESSION-005: Session Data Includes Necessary Metadata
**Given** session is finalized  
**When** record is stored in database  
**Then** all required metadata is included

**Acceptance Criteria:**
- [ ] Class ID stored with record
- [ ] Date and time of attendance session stored
- [ ] Teacher ID stored
- [ ] Campus ID stored (for isolation)
- [ ] For each student: ID, name, status (Present/Absent), confidence score
- [ ] Audit trail of all corrections included

---

## Feature 7: Attendance Reporting

### AC-REPORT-001: Teacher Can Export Per-Class Attendance Report
**Given** teacher has conducted multiple attendance sessions  
**When** teacher requests export of class attendance  
**Then** CSV file is generated with attendance data

**Acceptance Criteria:**
- [ ] Report includes date, class name, and student names
- [ ] Report shows attendance status for each student (Present/Absent)
- [ ] Report covers specified date range (e.g., last month, current term)
- [ ] CSV format is standard (comma-separated, UTF-8 encoded)
- [ ] Report is downloadable via browser
- [ ] Filename includes class name and date range for easy identification

### AC-REPORT-002: Teacher Can Export Per-Student Attendance Report
**Given** teacher wants to see one student's attendance history  
**When** teacher generates per-student report  
**Then** attendance record for that student is exported

**Acceptance Criteria:**
- [ ] Report shows student name, class, and attendance dates
- [ ] Report shows Present/Absent status for each class
- [ ] Attendance rate calculated (e.g., "80% attendance")
- [ ] Report covers date range specified by teacher
- [ ] CSV format easily imported to spreadsheet applications

### AC-REPORT-003: Reports Can Be Filtered by Date Range
**Given** teacher wants attendance for specific period  
**When** teacher sets date range filter  
**Then** report includes only sessions within date range

**Acceptance Criteria:**
- [ ] Date range picker available ("From" and "To" dates)
- [ ] Default date range is current term or month
- [ ] Report respects date filter
- [ ] Invalid date ranges show error message
- [ ] Report summary shows filtered date range

---

## Feature 8: Campus Data Isolation

### AC-ISO-001: Teacher Cannot Access Another Campus's Data
**Given** Teacher A is assigned to Boys Campus  
**When** Teacher A attempts to access Girls Campus data (via URL, API, or other means)  
**Then** access is denied with "Unauthorized" error

**Acceptance Criteria:**
- [ ] Database queries include campus filter
- [ ] Teacher login binds user to single campus
- [ ] API endpoints validate user's campus before returning data
- [ ] UI does not show other campus's data in dropdowns or lists
- [ ] Attempting direct API call with wrong campus_id returns 403 Forbidden

### AC-ISO-002: Admin Cannot Access Another Campus's Data
**Given** Admin A manages Boys Campus  
**When** Admin A attempts to enroll student at Girls Campus  
**Then** enrollment fails with "Unauthorized" error

**Acceptance Criteria:**
- [ ] Admin login binds user to single campus
- [ ] Student enrollment validates campus before saving
- [ ] Enrollment list shows only own-campus students
- [ ] Audit logs prevent cross-campus access attempts

### AC-ISO-003: Database Enforces Campus-Level Isolation
**Given** database contains data from multiple campuses  
**When** query is executed for a user  
**Then** results include only that user's campus data

**Acceptance Criteria:**
- [ ] All tables include campus_id column
- [ ] All queries filter by campus_id
- [ ] Database-level constraints prevent cross-campus queries
- [ ] No cross-campus foreign keys allowed (unless explicit design)

---

## Feature 9: Privacy & Security

### AC-PRIVACY-001: Face Embeddings Are Encrypted at Rest
**Given** face embeddings stored in database  
**When** database file is examined  
**Then** embeddings are encrypted and unreadable without decryption key

**Acceptance Criteria:**
- [ ] Embeddings stored with AES-256 encryption (or equivalent)
- [ ] Encryption key is stored separately (not in database)
- [ ] Raw embeddings cannot be read from database backup
- [ ] Decryption is transparent to authorized API calls

### AC-PRIVACY-002: Data in Transit Is Encrypted (HTTPS)
**Given** data is transmitted between client and server  
**When** API request is made  
**Then** data is encrypted with HTTPS/TLS

**Acceptance Criteria:**
- [ ] All API endpoints use HTTPS only (no HTTP)
- [ ] Valid SSL/TLS certificate installed
- [ ] HTTP requests redirected to HTTPS
- [ ] Browser shows "secure" indicator
- [ ] No plaintext transmission of credentials or biometric data

### AC-PRIVACY-003: Passwords Are Hashed
**Given** admin or teacher password is stored  
**When** password is checked against stored hash  
**Then** plaintext password is not stored, only secure hash

**Acceptance Criteria:**
- [ ] Passwords hashed with bcrypt, scrypt, or Argon2
- [ ] Hash cannot be reversed to obtain plaintext
- [ ] Salt is used for password hashing
- [ ] Database breach does not expose plaintext passwords

### AC-PRIVACY-004: Raw Enrollment Photos Are Not Retained
**Given** enrollment photos are uploaded to generate embeddings  
**When** embeddings are generated and stored  
**Then** raw photos are deleted (if feasible)

**Acceptance Criteria:**
- [ ] Raw enrollment photos not stored in database
- [ ] Raw classroom photos (from attendance) not retained long-term
- [ ] Only embeddings retained for long-term use
- [ ] Privacy policy documents this practice

### AC-PRIVACY-005: Audit Trail of Data Access
**Given** biometric data is accessed  
**When** user queries embeddings or attendance records  
**Then** access is logged

**Acceptance Criteria:**
- [ ] Logs include: timestamp, user ID, action, data accessed
- [ ] Logs are immutable (append-only)
- [ ] Logs retained for [TBD] duration
- [ ] Admin can view audit logs for compliance

---

## Feature 10: Accuracy Evaluation (Testing)

### AC-ACC-001: Test Dataset Captures Varied Lighting Conditions
**Given** test dataset is prepared  
**When** photos are captured under different lighting  
**Then** test set includes bright and dim conditions

**Acceptance Criteria:**
- [ ] Bright lighting photos captured (~500+ lux)
- [ ] Dim lighting photos captured (~100-200 lux)
- [ ] Each condition has multiple test photos (count TBD)
- [ ] Photos labeled with lighting condition
- [ ] Manual ground-truth labels verified by independent reviewer

### AC-ACC-002: Test Dataset Captures Varied Camera Angles
**Given** test dataset is prepared  
**When** photos are captured from different angles  
**Then** test set includes frontal, side, and varied angles

**Acceptance Criteria:**
- [ ] Frontal-view photos (camera facing classroom)
- [ ] Side-angle photos (camera at 30-45 degree angle)
- [ ] Photos from different heights/positions
- [ ] Each angle condition tested
- [ ] Ground-truth labels documented

### AC-ACC-003: Test Dataset Captures Varied Student Distances
**Given** test dataset is prepared  
**When** photos are captured with varying camera distances  
**Then** test set includes near, medium, and far distances

**Acceptance Criteria:**
- [ ] Near distance photos (camera ~2 meters from students)
- [ ] Medium distance photos (camera ~4-5 meters)
- [ ] Far distance photos (camera ~7+ meters, if applicable)
- [ ] Each distance affects face size in image
- [ ] Ground-truth verified for distance variation

### AC-ACC-004: Test Dataset Captures Partial Occlusion
**Given** test dataset is prepared  
**When** photos include partially occluded faces  
**Then** test set includes turned faces and face coverings

**Acceptance Criteria:**
- [ ] Photos with faces turned 30-45 degrees
- [ ] Photos with face coverings (scarves, masks)
- [ ] Photos with students partially overlapping
- [ ] Ground-truth identifies each occluded face
- [ ] Recognition accuracy measured separately for occlusion

### AC-ACC-005: Test Dataset Includes Multiple Class Sizes
**Given** accuracy is tested across different class sizes  
**When** photos are captured  
**Then** test set includes 20, 40, and 60 students per photo

**Acceptance Criteria:**
- [ ] Test photos with ~20 students
- [ ] Test photos with ~40 students
- [ ] Test photos with ~60 students
- [ ] Each class size condition tested with multiple photos
- [ ] Ground-truth manually labeled for all students in each photo

### AC-ACC-006: System Generates Accuracy Metrics
**Given** test photos are processed by system  
**When** results are compared to ground-truth  
**Then** accuracy metrics are computed and logged

**Acceptance Criteria:**
- [ ] True Positives (TP) counted: correctly identified students
- [ ] False Positives (FP) counted: absent student marked present
- [ ] False Negatives (FN) counted: present student marked absent
- [ ] Undetected faces counted: faces not detected by system
- [ ] Metrics computed per condition (lighting, angle, distance, occlusion, class size)
- [ ] Metrics compiled into accuracy report

### AC-ACC-007: Accuracy Report Is Comprehensive
**Given** all test metrics are collected  
**When** accuracy report is compiled  
**Then** report includes breakdown by condition

**Acceptance Criteria:**
- [ ] Report shows accuracy by lighting condition
- [ ] Report shows accuracy by camera angle
- [ ] Report shows accuracy by distance
- [ ] Report shows accuracy by occlusion type
- [ ] Report shows accuracy by class size (20, 40, 60)
- [ ] Overall accuracy percentage calculated
- [ ] Report identifies weak areas (low accuracy conditions)
- [ ] Report includes recommendations for improvement

---

## Summary of Test Coverage

| Feature | # of Criteria | Status |
|---------|---------------|--------|
| Enrollment | 6 | Defined |
| Authentication | 4 | Defined |
| Class & Photo | 4 | Defined |
| Face Detection | 5 | Defined |
| Manual Override | 6 | Defined |
| Session Finalization | 5 | Defined |
| Reporting | 3 | Defined |
| Campus Isolation | 3 | Defined |
| Privacy & Security | 5 | Defined |
| Accuracy Evaluation | 7 | Defined |
| **Total** | **48** | **Defined** |

---

## Notes on Acceptance Criteria

1. **[TBD] placeholders:** Some specific values (e.g., processing time limits, default thresholds, inactivity timeout) are marked [TBD] and must be decided during Day 2 (Technical Design phase).

2. **Measurability:** Each criterion includes testable conditions (can/cannot, shows/does not show, etc.) rather than subjective requirements.

3. **User-Centric:** Criteria focus on user-visible behavior and outcomes, not implementation details.

4. **Coverage:** Criteria cover happy path (normal flow) and some error cases, but exhaustive negative testing will be detailed during QA planning (post-Day 1).

5. **Accuracy Testing:** Accuracy evaluation criteria are high-level; detailed test protocols will be defined during Day 3 (Testing & Evaluation Planning).

