# Requirements Analysis
## Cloud-Based Face Recognition Attendance System

**Date:** 2026-09-30  
**Status:** Day 1 - Requirements Analysis Only

---

## Project Overview

A cloud-based attendance system that automatically detects and identifies students from a single classroom photograph, marking them present in real-time. The system is designed for gender-segregated educational institutions across Pakistan, where each campus maintains completely isolated data.

---

## Problem Statement

- Manual attendance-taking consumes classroom time
- Existing roll-call is vulnerable to proxy attendance (friends marking each other present)
- Paper/manual records are cumbersome to compile into reliable reports
- Existing biometric alternatives (fingerprint scanners, RFID cards) require:
  - Per-student hardware cost
  - Physical contact with shared devices
  - Consistent deployment across every classroom (impractical)

---

## Target Users

1. **Teachers** - take classroom photo, verify attendance, mark present/absent
2. **Students** - enroll with face data (with consent), appear in attendance records
3. **Administrators** - manage campus data, user accounts, access control
4. **Parents/Guardians** - (implied) provide consent for minor students' biometric data

---

## User Roles

- **Admin** - per-campus account, manage teachers and enrollment data
- **Teacher** - per-campus account, conduct attendance sessions, override system decisions
- **Student** - enrolled in system, biometric data stored (no system login required)

---

## Functional Requirements

### FR-001: Teacher/Admin Authentication
- System must support separate login for Teachers and Admins
- Authentication must be per-campus (boys and girls campus data completely isolated)
- System must support role-based access control

### FR-002: Student Enrollment
- Admin or Teacher must be able to enroll a student with 2–3 reference photos
- Each student must have unique enrollment per campus
- Enrollment must capture informed consent before biometric data is stored
- Enrollment process must be one-time per student per campus

### FR-003: Face Data Storage (Embeddings)
- System must store face embeddings (mathematical representations), not raw photos
- Each student's embedded face data must be stored securely
- Embeddings must be encrypted at rest

### FR-004: Single Photo Upload
- Teacher must be able to upload or capture a single classroom photograph
- System must accept standard image formats (JPEG, PNG)
- Photo must contain multiple faces (20–60 students)

### FR-005: Face Detection
- System must detect all faces present in the classroom photograph
- System must localize face bounding boxes for each detected face
- System must handle partial occlusion (e.g., turned faces, face coverings)

### FR-006: Face Recognition (Embedding Matching)
- System must convert each detected face into a numeric embedding
- System must compare detected embeddings against enrolled students' stored embeddings
- System must use similarity matching to compute confidence score for each match
- System must mark matches above confidence threshold as "Likely Present"
- System must flag matches below confidence threshold for manual teacher review

### FR-007: Confidence Threshold
- System must have a configurable confidence threshold
- Faces above threshold → auto-marked present
- Faces below threshold → flagged for teacher manual review
- (Threshold value not specified in proposal; must be decided in Day 2+)

### FR-008: Manual Override
- Teacher must be able to review flagged faces
- Teacher must be able to mark a flagged face as Present
- Teacher must be able to mark a flagged face as Absent
- Teacher must be able to correct a system-marked face (change Present → Absent or vice versa)
- Teacher must be able to add manually-recognized students not detected by system

### FR-009: Attendance Session Finalization
- Teacher must be able to review all detected/flagged/corrected faces before finalizing
- Teacher must be able to finalize the attendance session (commit records to database)
- Once finalized, session must be immutable (audit trail)

### FR-010: Attendance Record Storage
- System must store attendance records with:
  - Date/Time of session
  - Class identifier
  - Student ID and presence status (Present/Absent)
  - Confidence score(s) for each match
  - Any manual overrides by teacher

### FR-011: Real-Time Teacher Dashboard
- Teacher dashboard must show:
  - Class selection
  - Upload/capture interface for classroom photo
  - Detected faces with bounding boxes
  - System-matched student names and confidence scores
  - Flagged faces requiring manual review
  - Manual override interface
  - Finalize session button

### FR-012: Admin Dashboard
- Admin dashboard must show:
  - List of enrolled students per campus
  - Enrollment status (enrolled/pending)
  - Ability to delete/unenroll a student
  - (Scope unclear: can admin trigger re-enrollment of a student?)

### FR-013: Attendance Reporting
- System must generate reports showing:
  - Per-student attendance over a term
  - Per-class attendance
  - Export functionality (CSV or similar)
  - (Format not specified in proposal)

### FR-014: Campus Data Isolation
- Boys' campus data must be completely isolated from girls' campus data
- No cross-campus queries allowed
- No cross-campus access by any user account
- Each campus must have separate Admin and Teacher accounts

### FR-015: Consent Management
- System must require explicit informed consent before enrolling a student
- For minor students, consent from parent/guardian must be recorded
- (Mechanism not specified: how is consent captured? Digital signature? Checkbox?)

### FR-016: Data Retention & Deletion
- System must allow deletion of a student's complete biometric data
- System must define retention period for attendance records
- (Specific retention period not specified in proposal)

### FR-017: Audit Trail
- All attendance sessions must be auditable
- Manual overrides must be logged with timestamp and teacher ID
- Deletion of student data must be logged

---

## Non-Functional Requirements

### NFR-001: Accuracy Under Real-World Conditions
- System must maintain measurable accuracy when:
  - Lighting varies (bright to dim)
  - Camera angle varies
  - Distance from camera varies
  - Partial occlusion present (face coverings, turned faces)
  - Class size varies (20, 40, 60 students per photo)

### NFR-002: Recognition Performance
- System must recognize all enrolled students in the class (or flag for manual review)
- System must minimize false positives (marking absent student as present)
- System must minimize false negatives (missing present students)
- (Specific accuracy targets not stated in proposal; to be established during testing)

### NFR-003: Response Time
- Face detection on classroom photo must complete in reasonable time
- Face recognition matching must complete in reasonable time
- (Specific SLAs not defined in proposal)

### NFR-004: Scalability
- System must handle class sizes up to 60 students per photo
- System must handle multiple campuses with separate data stores
- (Peak load requirements not specified)

### NFR-005: Data Encryption
- Face embeddings must be encrypted at rest
- Face embeddings must be encrypted in transit (HTTPS)
- (Encryption algorithm not specified)

### NFR-006: Cloud Deployment
- Backend must be deployed on cloud VM or serverless (AWS Lambda, etc.)
- System must be accessible from any enrolled campus without local infrastructure
- (Specific cloud provider not mandated; proposal mentions AWS but instruction says free-first)

### NFR-007: Privacy-by-Design
- Store embeddings, not raw photos, where feasible
- Minimize biometric data collection and storage
- Implement per-campus data isolation at database level

### NFR-008: Audit Compliance
- All attendance records must remain auditable
- All manual corrections must be logged
- No silent system changes allowed

---

## Student Enrollment Requirements

### ENROLL-001: Enrollment Initiation
- Admin or Teacher can initiate student enrollment
- Student identification (name, roll number, campus) required

### ENROLL-002: Reference Photos
- System must accept 2–3 reference photos per student
- Each photo must capture student's face clearly
- Photos must be captured from different angles/distances to handle real-world variation

### ENROLL-003: Consent Capture
- Before storing face data, system must record informed consent
- For minors, guardian consent required (mechanism TBD)
- Consent must be auditable

### ENROLL-004: Embedding Generation
- System must generate face embedding(s) from enrolled photos
- System must store embeddings, not raw photos (where feasible)
- (Whether all 2–3 photos become separate embeddings or single averaged embedding is TBD)

### ENROLL-005: One-Time Enrollment
- Each student enrolls once per campus
- Re-enrollment process not specified in proposal (TBD)

---

## Face Detection Requirements

### FD-001: Multi-Face Detection
- Must detect all faces in a classroom photo simultaneously
- Must not require sequential processing of individual students

### FD-002: Face Localization
- Must output bounding box for each detected face
- Must output face orientation/pose estimates (helps handle angle variation)

### FD-003: Occlusion Handling
- Must detect faces partially occluded by:
  - Face coverings (e.g., scarves, masks)
  - Turned/profile faces
  - Partial overlap with other students

### FD-004: False Positive Minimization
- Must not flag non-face objects as faces
- Must not create false face detections

### FD-005: Detection Confidence
- Must output confidence score for each face detection
- (Whether low-confidence detections are discarded or flagged for review is TBD)

---

## Face Recognition Requirements

### FR-RECOG-001: Embedding Generation
- Must convert detected face to fixed-size numeric embedding
- Embedding generation must be deterministic (same input → same output)

### FR-RECOG-002: Similarity Matching
- Must compute similarity between detected face embedding and enrolled student embeddings
- Must use standard distance metric (e.g., Euclidean, cosine similarity)

### FR-RECOG-003: Confidence Thresholding
- Must output confidence score (0–1 or 0–100) for each potential match
- Must support configurable threshold for auto-accept vs manual review

### FR-RECOG-004: No False Negatives (Priority)
- System must minimize missed detections (absent student marked as absent when they should be present)
- Better to flag for manual review than to miss a present student

### FR-RECOG-005: Limited False Positives
- System must minimize false matches (marking absent student as present)
- Manual override by teacher corrects false positives

### FR-RECOG-006: Real-World Robustness
- Recognition must work across:
  - Varying lighting conditions
  - Different camera angles
  - Different distances from camera
  - Partial occlusion scenarios

---

## Attendance Requirements

### ATT-001: Attendance Session
- Teacher selects a class
- Teacher captures/uploads classroom photo
- System detects and recognizes all faces
- Teacher reviews and corrects results
- Teacher finalizes session (commits to database)

### ATT-002: Present/Absent Status
- Each session records each student as Present or Absent
- Unmarked students must be explicitly marked Absent (not auto-filled)

### ATT-003: Confidence Scores
- System must record confidence scores for each match (for accuracy analysis)
- (Whether confidence scores are visible to teacher in dashboard is TBD)

### ATT-004: Session Audit
- Each attendance session must be immutable once finalized
- Any corrections must be logged separately (not overwritten)

### ATT-005: Multiple Sessions Per Day
- Teacher must be able to conduct multiple attendance sessions (multiple classes per day)
- Each session must be independent

---

## Manual Override Requirements

### OVERRIDE-001: Teacher Review Interface
- Teacher must see all detected faces with:
  - Bounding box on photo
  - Student name (if matched)
  - Confidence score (if available)

### OVERRIDE-002: Flagged Face Resolution
- Low-confidence faces must be flagged for teacher confirmation
- Teacher must choose: Mark Present, Mark Absent, or Identify Manually

### OVERRIDE-003: Manual Identification
- If system cannot recognize a face, teacher must be able to manually select student from dropdown/list

### OVERRIDE-004: Correction of Errors
- Teacher must be able to change system-marked Present → Absent
- Teacher must be able to change system-marked Absent → Present

### OVERRIDE-005: Override Logging
- Every manual override must be logged (timestamp, teacher ID, what changed)

---

## Teacher Dashboard Requirements

### DASH-TEACHER-001: Class Selection
- Teacher must select which class is being marked present
- Class must be associated with a date/time

### DASH-TEACHER-002: Photo Upload
- Teacher must be able to upload classroom photo OR capture from camera
- System must preview photo before processing

### DASH-TEACHER-003: Real-Time Results Display
- After upload, dashboard must display:
  - Detected faces with bounding boxes on photo
  - System-matched names and confidence scores
  - Flagged faces for manual review

### DASH-TEACHER-004: Override Interface
- Teacher must be able to interact with each face:
  - Confirm system match
  - Correct system match
  - Mark flagged face with manually-selected student name
  - Mark unrecognized student (if present but not detected)

### DASH-TEACHER-005: Session Summary
- Before finalizing, dashboard must show:
  - Total students present
  - Total students absent
  - Total students requiring manual review
  - Ability to go back and fix any mistake

### DASH-TEACHER-006: Session Finalization
- One-click finalization button
- Confirmation dialog to prevent accidental clicks
- Success message after finalization

---

## Admin Requirements

### ADMIN-001: Campus Management
- Admin must manage per-campus user accounts
- Admin must create Teacher accounts
- Admin must manage other Admin accounts (if multi-admin per campus allowed)
- (Whether teachers can be deactivated/deleted not specified)

### ADMIN-002: Student Management
- Admin must view list of enrolled students per campus
- Admin must enroll new students (via photo capture)
- Admin must delete/unenroll a student (triggering data deletion per FR-016)
- (Whether admin can re-enroll a deleted student or edit existing enrollment not specified)

### ADMIN-003: Access Control
- Admin must not be able to access another campus's data
- Admin must not be able to create accounts for another campus
- Admin must enforce role-based permissions

### ADMIN-004: Audit Log Viewing
- Admin must be able to view audit log of all system actions
- (Log retention duration not specified)

---

## Reporting Requirements

### REPORT-001: Per-Student Report
- Report showing student name, attendance date(s), Present/Absent status
- Must cover specified time period (e.g., one term)
- Must show attendance rate (percentage of classes attended)

### REPORT-002: Per-Class Report
- Report showing all students in a class, presence status for each class session
- Must show class date(s), teacher, attendance rate

### REPORT-003: Export Format
- Reports must be exportable to CSV or similar text format
- (Excel format not specified; PDF not specified)

### REPORT-004: Report Filtering
- Reports must support filtering by date range, class, student
- (Specific filter options not detailed in proposal)

---

## Authentication Requirements

### AUTH-001: Login Credentials
- Admin and Teacher must authenticate with username/password
- (MFA, OAuth, SAML not specified in proposal; TBD)

### AUTH-002: Session Management
- System must maintain secure sessions after login
- Sessions must expire after inactivity period
- (Inactivity timeout duration not specified)

### AUTH-003: Per-Campus Isolation
- Login must be per-campus; teacher at Boys Campus cannot access Girls Campus data
- (How multi-campus teachers are handled not specified; assume one campus per login session)

### AUTH-004: Password Security
- Passwords must meet security standards (length, complexity)
- (Specific policy not defined; TBD)

---

## Authorization Requirements

### AUTHZ-001: Role-Based Access Control (RBAC)
- Teacher role: can conduct attendance, access own-campus data only
- Admin role: can manage users and student enrollment, access own-campus data only
- (Student role: read-only access to own attendance records? Not specified)

### AUTHZ-002: Campus-Level Isolation
- No user can query or access another campus's data
- Campus isolation must be enforced at database query level, not just UI level

### AUTHZ-003: Cross-Campus Restrictions
- No cross-campus data sharing allowed
- No cross-campus API endpoints

---

## Campus Data Isolation Requirements

### ISO-001: Database-Level Isolation
- All tables must include campus identifier
- All queries must filter by campus
- No query must return cross-campus data

### ISO-002: Access Control
- User authentication must bind user to single campus
- All API requests must validate user's campus
- All UI must only show own-campus data

### ISO-003: Boys/Girls Campus Separation
- Boys campus data must never be visible to Girls campus users
- Girls campus data must never be visible to Boys campus users
- (Enforcement is same as general campus isolation; no special handling needed)

---

## Privacy and Consent Requirements

### PRIVACY-001: Informed Consent
- Before enrolling student, system must obtain and record informed consent
- For minors, consent from parent/guardian required
- Consent must be in local language (Urdu for Pakistan context)
- (Mechanism for capturing consent not specified: digital signature, checkbox, printed form?)

### PRIVACY-002: Data Minimization
- Collect and store only face embeddings, not raw enrollment photos
- Do not store attendance photos after processing
- (Whether system retains raw classroom photo used for attendance is TBD)

### PRIVACY-003: Biometric Data Encryption
- Face embeddings must be encrypted at rest
- Face embeddings must be encrypted in transit
- (Key management and rotation not specified; TBD)

### PRIVACY-004: Data Retention
- Define maximum retention period for attendance records
- Define process for automatic deletion of old records
- Define process for student data deletion upon request (right to be forgotten)
- (Specific retention duration not stated in proposal; must be defined in Day 2+)

### PRIVACY-005: Purpose Limitation
- Face embeddings must be used only for attendance
- No secondary use (surveillance, other applications) allowed
- No data sharing with third parties without explicit consent

### PRIVACY-006: Transparency
- Privacy policy must be available to users
- Users must understand what data is collected and how it's used
- Consent must be explicit, not implied

---

## Security Requirements

### SEC-001: Transport Security
- All communication must use HTTPS/TLS
- All API endpoints must require HTTPS
- No plaintext transmission of credentials or biometric data

### SEC-002: Data Encryption at Rest
- Face embeddings must be encrypted at rest (AES-256 or equivalent)
- Attendance records must be encrypted or protected
- (Key management strategy not specified; TBD)

### SEC-003: Authentication Security
- Passwords must never be stored in plaintext
- Passwords must be hashed with strong algorithm (bcrypt, scrypt, Argon2)
- (Password policy details TBD)

### SEC-004: Session Management
- Sessions must be stored securely (signed/encrypted tokens)
- Session hijacking must be prevented
- Sessions must timeout after inactivity

### SEC-005: Audit Logging
- All sensitive actions must be logged (login, attendance changes, data deletion)
- Logs must be immutable (append-only)
- Logs must include timestamp, user ID, action, result

### SEC-006: Input Validation
- All user inputs must be validated and sanitized
- Image uploads must be validated (size, format, content)
- (Specific validation rules not detailed; TBD)

### SEC-007: Error Handling
- System must not leak sensitive information in error messages
- Error logs must not expose system internals to users

---

## Accuracy Evaluation Requirements

### ACC-001: Test Dataset Structure
- System must test face recognition against structured set of classroom photos
- Photos must vary across multiple conditions (lighting, angle, distance, occlusion)
- (Number of test photos per condition not specified; to be decided during testing)

### ACC-002: Class Size Variations
- Testing must include class sizes:
  - 20 students per photo
  - 40 students per photo
  - 60 students per photo

### ACC-003: Lighting Conditions
- Testing must include:
  - Bright lighting conditions
  - Dim lighting conditions
  - (Specific lux levels not defined; TBD)

### ACC-004: Camera Angle Variations
- Testing must include:
  - Different seating angles (students at various positions in classroom)
  - Different camera angles (not just front-facing)

### ACC-005: Camera Distance Variations
- Testing must include:
  - Various distances from camera to student faces

### ACC-006: Occlusion Scenarios
- Testing must include:
  - Partially occluded faces (turned sideways)
  - Face coverings (scarves, masks, etc.)
  - Partial overlap with other students

### ACC-007: Ground Truth Labeling
- Test dataset must have manual ground truth labels
- Each face in test photo must be manually identified with student ID
- Manual labeling must be verified by independent reviewer

### ACC-008: Accuracy Metrics
- System must record for each test scenario:
  - True Positives (correctly identified present students)
  - False Positives (absent student marked present)
  - False Negatives (present student marked absent)
  - Undetected faces (faces not detected by system)

### ACC-009: Accuracy Report
- Final report must compile accuracy data by:
  - Lighting condition
  - Camera angle
  - Distance
  - Occlusion type
  - Class size

### ACC-010: No Accuracy Targets Specified
- Proposal does not mandate specific accuracy targets (e.g., "95% accuracy")
- Targets must be defined after initial testing (Day 3+)

---

## Cloud/Deployment Requirements

### DEPLOY-001: Cloud Hosting
- Backend must be deployed on cloud platform
- System must be accessible from any campus without local infrastructure
- (Specific cloud provider not mandated; AWS, GCP, Azure, or others acceptable)

### DEPLOY-002: Scalability
- Backend must support multiple concurrent teachers (multiple attendance sessions)
- Backend must support multiple campuses
- (Peak concurrency not specified; to be defined during capacity planning)

### DEPLOY-003: High Availability
- System must support campus multi-teacher usage simultaneously
- (Uptime SLA not specified; TBD)

### DEPLOY-004: Backup and Recovery
- Database backups must be performed regularly
- (Backup frequency and recovery time not specified; TBD)

### DEPLOY-005: Free/Low-Cost Approach
- System must prioritize free or low-cost cloud services
- Must use open-source or free alternatives where practical
- (Specific budget not stated; assumed "free tier" where available)

---

## Summary of Requirement IDs

**Functional Requirements:** FR-001 to FR-017  
**Non-Functional Requirements:** NFR-001 to NFR-008  
**Enrollment:** ENROLL-001 to ENROLL-005  
**Face Detection:** FD-001 to FD-005  
**Face Recognition:** FR-RECOG-001 to FR-RECOG-006  
**Attendance:** ATT-001 to ATT-005  
**Manual Override:** OVERRIDE-001 to OVERRIDE-005  
**Teacher Dashboard:** DASH-TEACHER-001 to DASH-TEACHER-006  
**Admin:** ADMIN-001 to ADMIN-004  
**Reporting:** REPORT-001 to REPORT-004  
**Authentication:** AUTH-001 to AUTH-004  
**Authorization:** AUTHZ-001 to AUTHZ-003  
**Campus Isolation:** ISO-001 to ISO-003  
**Privacy:** PRIVACY-001 to PRIVACY-006  
**Security:** SEC-001 to SEC-007  
**Accuracy Evaluation:** ACC-001 to ACC-010  
**Deployment:** DEPLOY-001 to DEPLOY-005  

**Total: 101 Requirements Documented**
