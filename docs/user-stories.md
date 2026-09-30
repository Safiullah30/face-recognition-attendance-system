# User Stories
## Cloud-Based Face Recognition Attendance System

**Date:** 2026-09-30  
**Status:** Day 1 - Requirements Analysis

---

## Admin User Stories

### Enrollment & Management

**ADMIN-US-001: Enroll a New Student**
- As an **Admin**, I want to **enroll a new student by capturing 2–3 reference photos**, so that **the student's face is registered in the system and can be recognized in future attendance sessions**.

**ADMIN-US-002: Record Student Consent**
- As an **Admin**, I want to **record informed consent before storing a student's face data**, so that **the enrollment process is transparent and legally compliant**.

**ADMIN-US-003: Handle Minor Student Consent**
- As an **Admin**, I want to **record guardian consent for minor students**, so that **parent/guardian authorization is documented and auditable**.

**ADMIN-US-004: View All Enrolled Students**
- As an **Admin**, I want to **see a list of all enrolled students for my campus**, so that **I can verify enrollment status and track total enrollment**.

**ADMIN-US-005: Delete a Student's Data**
- As an **Admin**, I want to **delete a student's face embeddings and biometric data on request**, so that **the student's data is removed when they leave the institution (right to be forgotten)**.

**ADMIN-US-006: Verify Enrollment Photo Quality**
- As an **Admin**, I want to **preview enrollment photos before committing them**, so that **I can retake poor-quality photos if necessary**.

### Account Management

**ADMIN-US-007: Create Teacher Accounts**
- As an **Admin**, I want to **create login accounts for teachers in my campus**, so that **teachers can access the attendance system**.

**ADMIN-US-008: Manage My Campus Only**
- As an **Admin**, I want to **only see and manage data for my assigned campus**, so that **boys' and girls' campus data remain completely separate**.

**ADMIN-US-009: Prevent Cross-Campus Access**
- As an **Admin**, I want the system to **prevent me from accessing another campus's data**, so that **institutional data segregation is enforced**.

### Audit & Compliance

**ADMIN-US-010: View Audit Logs**
- As an **Admin**, I want to **view audit logs of all system actions (logins, enrollment, attendance changes, deletions)**, so that **I can verify system compliance and investigate issues**.

**ADMIN-US-011: Export Audit Logs**
- As an **Admin**, I want to **export audit logs for compliance and review**, so that **I have records for institutional audits and investigations**.

**ADMIN-US-012: Verify Data Retention Policy**
- As an **Admin**, I want to **know how long attendance records are retained**, so that **my campus complies with privacy and data protection policies**.

---

## Teacher User Stories

### Class & Session Management

**TEACHER-US-001: Select Class for Attendance**
- As a **Teacher**, I want to **select which class I am marking attendance for**, so that **attendance is recorded for the correct class and date**.

**TEACHER-US-002: Upload Classroom Photo**
- As a **Teacher**, I want to **upload a photograph of my classroom**, so that **the system can automatically detect and recognize students**.

**TEACHER-US-003: Capture Classroom Photo**
- As a **Teacher**, I want to **capture a classroom photo directly from the system (via camera)**, so that **I don't need to use a separate camera app**.

### Face Recognition & Manual Review

**TEACHER-US-004: See Recognized Students**
- As a **Teacher**, I want to **see which students the system has automatically recognized in the photo**, so that **I can verify the system is working correctly**.

**TEACHER-US-005: Review Confidence Scores**
- As a **Teacher**, I want to **see confidence scores for each recognized face**, so that **I understand which matches are certain and which are uncertain**.

**TEACHER-US-006: Review Flagged Faces**
- As a **Teacher**, I want to **review faces flagged by the system as uncertain**, so that **I can manually identify or correct the system's decisions**.

**TEACHER-US-007: Confirm System Match**
- As a **Teacher**, I want to **confirm that a system-matched student is correctly identified**, so that **the attendance session proceeds without correction**.

**TEACHER-US-008: Correct Wrong Identification**
- As a **Teacher**, I want to **change a system-matched student to the correct student**, so that **attendance records are accurate**.

**TEACHER-US-009: Manually Identify Unrecognized Face**
- As a **Teacher**, I want to **select a student name from a dropdown to manually identify a flagged face**, so that **present students are not missed**.

**TEACHER-US-010: Mark System-Missed Student**
- As a **Teacher**, I want to **manually mark a student as present if the system failed to detect their face**, so that **no student is incorrectly marked absent**.

**TEACHER-US-011: Override System Decision**
- As a **Teacher**, I want to **change a system-marked student from Present to Absent (or vice versa)**, so that **I can correct system errors before finalizing**.

### Session Finalization

**TEACHER-US-012: Review Session Summary Before Finalizing**
- As a **Teacher**, I want to **see a summary of present/absent/flagged counts before finalizing**, so that **I can double-check everything is correct**.

**TEACHER-US-013: Undo Corrections Before Finalizing**
- As a **Teacher**, I want to **go back and fix any mistake before finalizing the session**, so that **I don't accidentally commit wrong data**.

**TEACHER-US-014: Finalize Attendance Session**
- As a **Teacher**, I want to **finalize the attendance session and save it to the system**, so that **the attendance record is permanently recorded**.

**TEACHER-US-015: Confirm Before Finalizing**
- As a **Teacher**, I want to **see a confirmation dialog before finalizing**, so that **I don't accidentally finalize prematurely**.

### Dashboard & User Experience

**TEACHER-US-016: Real-Time Dashboard Feedback**
- As a **Teacher**, I want to **see real-time feedback from the system** (detected faces, confidence scores, flagged items), so that **I can quickly review and act on system results**.

**TEACHER-US-017: Visual Face Bounding Boxes**
- As a **Teacher**, I want to **see bounding boxes around detected faces on the uploaded photo**, so that **I can visually confirm which students the system has identified**.

**TEACHER-US-018: Navigate UI Easily**
- As a **Teacher**, I want the system to be **easy to navigate with clear buttons and labels**, so that **I don't need extensive training to use it**.

### Reporting & History

**TEACHER-US-019: View Past Attendance Sessions**
- As a **Teacher**, I want to **view previous attendance sessions I've conducted**, so that **I can review or re-verify past records**.

**TEACHER-US-020: Export Class Attendance Report**
- As a **Teacher**, I want to **export attendance records for my class in CSV format**, so that **I can share attendance with students or administration**.

---

## Student User Stories

### Enrollment

**STUDENT-US-001: Understand Enrollment Process**
- As a **Student**, I want to **understand what data is being collected and how my face will be used**, so that **I can make an informed decision about enrollment**.

**STUDENT-US-002: Provide Informed Consent**
- As a **Student** (or parent/guardian for minors), I want to **give explicit consent before my face data is stored**, so that **my privacy rights are respected**.

**STUDENT-US-003: Learn Data Protection Measures**
- As a **Student**, I want to **know how my face data is protected and encrypted**, so that **I trust the system with my biometric information**.

### Attendance & Records

**STUDENT-US-004: View My Attendance**
- As a **Student**, I want to **view my own attendance records**, so that **I can track my attendance status (implied; not explicitly in proposal)**.

**STUDENT-US-005: Appear in Classroom Photo**
- As a **Student**, I want the system to **automatically recognize me when I appear in the classroom photo**, so that **I don't need to do anything to mark myself present**.

**STUDENT-US-006: No Physical Interaction with Devices**
- As a **Student**, I want to **participate in attendance without touching any device (no scanner, no card reader, no RFID)**, so that **the process is hygenic and minimal-contact**.

### Data Rights

**STUDENT-US-007: Request Data Deletion**
- As a **Student**, I want to **request deletion of my face data if I leave the institution**, so that **my biometric information is not retained indefinitely**.

**STUDENT-US-008: View Audit Trail (Optional)**
- As a **Student**, I want to **see who accessed my attendance data**, so that **I can verify privacy compliance (optional; not in proposal)**.

---

## Implicit/Derived User Stories

### System Maintenance

**SYSTEM-US-001: Monitor System Performance**
- As a **System Administrator** (future role, not in FYP scope), I want to **monitor system performance and uptime**, so that **attendance sessions don't fail due to system outages**.

**SYSTEM-US-002: Handle System Failures Gracefully**
- As a **Teacher**, I want the system to **provide a fallback if face recognition fails**, so that **I can still mark attendance manually if necessary**.

### Privacy & Compliance

**PRIVACY-US-001: Maintain Data Encryption**
- As the **System**, I want to **automatically encrypt face embeddings at rest and in transit**, so that **user privacy is protected without user action**.

**PRIVACY-US-002: Log All Sensitive Operations**
- As the **System**, I want to **log every access to biometric data**, so that **privacy compliance can be audited**.

---

## User Story Grouping by Epic

### Epic 1: Student Enrollment
- ADMIN-US-001, ADMIN-US-002, ADMIN-US-003
- ADMIN-US-006
- STUDENT-US-001, STUDENT-US-002, STUDENT-US-003

### Epic 2: Teacher Attendance Session
- TEACHER-US-001, TEACHER-US-002, TEACHER-US-003
- TEACHER-US-004, TEACHER-US-005, TEACHER-US-006
- TEACHER-US-007, TEACHER-US-008, TEACHER-US-009, TEACHER-US-010, TEACHER-US-011
- TEACHER-US-012, TEACHER-US-013, TEACHER-US-014, TEACHER-US-015
- TEACHER-US-016, TEACHER-US-017

### Epic 3: Admin Account Management
- ADMIN-US-007, ADMIN-US-008, ADMIN-US-009

### Epic 4: Student Data Management
- ADMIN-US-004, ADMIN-US-005
- STUDENT-US-007

### Epic 5: Audit & Compliance
- ADMIN-US-010, ADMIN-US-011, ADMIN-US-012
- PRIVACY-US-001, PRIVACY-US-002

### Epic 6: Reporting & Analytics
- TEACHER-US-019, TEACHER-US-020

---

## Assumptions About Users

**Teachers:**
- Have basic computer literacy (can upload files, click buttons)
- Work with one campus only (no multi-campus accounts in FYP scope)
- Conduct attendance once per class per day
- Are willing to review and correct system matches
- Can handle 5-20 minutes per attendance session

**Admins:**
- Have some technical background
- Manage one campus only
- Conduct enrollment once per semester (bulk enrollment at start of year)
- Perform audits and compliance checks occasionally
- Familiar with user account management

**Students:**
- Provide consent directly (if 18+) or via parent (if minor)
- Appear in classroom during scheduled classes
- Are not required to interact with system (passive participants)
- Expect privacy and data protection

---

## Notes on Student User Stories

The proposal mentions students as data subjects but does not explicitly define a "student user role" in the system. The system is designed such that:
- Students don't log into the system
- Students don't actively use the dashboard
- Students provide face data during enrollment
- Students are automatically recognized during attendance (passive participation)

Therefore, **Student User Stories focus on privacy rights, consent, and data protection** rather than active system usage.

