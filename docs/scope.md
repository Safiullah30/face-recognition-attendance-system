# Scope Definition
## Cloud-Based Face Recognition Attendance System

**Date:** 2026-09-30  
**Status:** Day 1 - Requirements Analysis

---

## In Scope

The following features and functionalities are explicitly included in this FYP project:

### Core System Features

✓ **Single Classroom Photo Workflow**
- Teacher captures or uploads one photo of classroom
- System detects all faces in photo simultaneously
- No scanning, no calling names, no per-student hardware required

✓ **Face Detection**
- Multi-face detection (20–60 students per photo)
- Face localization with bounding boxes
- Handle partial occlusion (turned faces, face coverings)
- Output confidence score for each detected face

✓ **Face Recognition**
- Generate face embeddings from detected faces
- Compare embeddings against enrolled student embeddings
- Similarity matching with confidence threshold
- Auto-mark above threshold, flag below threshold for review

✓ **Student Enrollment**
- One-time enrollment per student per campus
- Capture 2–3 reference photos per student
- Generate and store face embeddings
- Record informed consent before storing biometric data

✓ **Manual Override & Correction**
- Teacher reviews system-detected faces
- Teacher can confirm, correct, or manually identify flagged faces
- Teacher can mark unrecognized students manually
- All corrections logged with timestamp and teacher ID

✓ **Attendance Session Management**
- Teacher selects class and date
- System processes classroom photo
- Teacher reviews and corrects results
- Teacher finalizes session (commits to database)
- Session becomes immutable after finalization

✓ **Real-Time Teacher Dashboard**
- Class selection interface
- Photo upload/capture interface
- Live display of detected faces with student names and confidence scores
- Flagged faces for manual review
- Manual override controls
- Session summary (present/absent/review counts)
- Session finalization button

✓ **Admin Dashboard**
- Student enrollment interface (capture photos, manage enrollment)
- Teacher account management
- Audit log viewing
- (Specific admin features limited in scope; Day 2 to detail)

✓ **Attendance Records Storage**
- Store attendance session data: date, class, teacher, student presence status
- Store confidence scores for accuracy analysis
- Store manual override logs
- Cloud-based persistent storage

✓ **Attendance Reporting**
- Per-student attendance report (date, class, present/absent over term)
- Per-class attendance report (all students, presence per session)
- Export to CSV or text format
- Date range and class filtering

✓ **Authentication & Authorization**
- Teacher and Admin login (per campus)
- Role-based access control (Teacher role, Admin role)
- Session management (login/logout, inactivity timeout)
- Per-campus account isolation

✓ **Campus Data Isolation**
- Boys campus and girls campus data completely separate
- No cross-campus data access or queries
- Enforced at database and API levels
- Each campus has independent admin and teacher accounts

✓ **Privacy & Data Protection**
- Store face embeddings, not raw enrollment photos
- Encrypt face embeddings at rest
- Encrypt data in transit (HTTPS)
- Informed consent capture and recording
- Guardian consent for minor students
- User audit trails for sensitive operations

✓ **Accuracy Evaluation**
- Structured testing with varied conditions:
  - Different lighting (bright, dim)
  - Different camera angles
  - Different distances from camera
  - Partial occlusion (turned faces, coverings)
- Test class sizes: 20, 40, 60 students per photo
- Manual ground-truth labeling of test dataset
- Accuracy metrics: TP, FP, FN, detection rate
- Accuracy report breakdown by condition and class size

✓ **Cloud Deployment**
- Backend hosted on cloud VM or serverless platform
- Accessible from any campus without local infrastructure
- Database hosted on cloud (Supabase PostgreSQL or similar)
- Free-tier or low-cost cloud services prioritized

---

## Out of Scope

The following features are explicitly **NOT** included in this FYP project:

✗ **Real-Time Video Attendance**
- Project uses single still photos per session, not continuous video streams
- No live video feed processing
- No frame-by-frame face detection and recognition

✗ **Cross-Campus Data Sharing**
- No data synchronization between campus databases
- No cross-campus queries or analytics
- By design, each campus's data remains isolated

✗ **Cross-Campus Teacher Support**
- Teachers do not work across multiple campuses in this version
- Each teacher account tied to single campus
- No multi-campus account management

✗ **Mobile Application**
- No dedicated iOS or Android app
- System accessed via web browser on desktop/laptop
- Mobile web responsiveness optional

✗ **Facial Recognition Beyond Enrollment & Attendance**
- No general surveillance system
- No searching for specific students across videos
- No real-time alerting or tracking
- Face recognition used only for: enrollment and attendance sessions

✗ **Biometric Template Protection Standards**
- No ISO 19794 or ISO 29794 compliance in scope
- Standards compliance noted but not implemented

✗ **Full Production Rollout**
- Project is proof-of-concept or research prototype
- Not a complete production system ready for nationwide deployment
- Targeted at single school/college for pilot testing

✗ **Advanced Access Control**
- No multi-factor authentication (MFA)
- No biometric authentication for teachers
- Simple username/password sufficient

✗ **Advanced Reporting**
- No predictive analytics (e.g., predicting absenteeism trends)
- No machine learning-based insights
- Basic statistical reports only

✗ **Integration with Existing School Systems**
- No integration with school management systems (SMS)
- No integration with payroll, timetabling, or other educational systems
- Standalone application

✗ **Multi-Language Support**
- Initial version in English documentation
- UI language support not specified; may be English or Urdu (TBD)
- Not translated to multiple languages in scope

✗ **Teacher or Student Mobile Notifications**
- No push notifications, SMS, or email alerts
- No attendance reminder notifications
- No absence alerts to parents

✗ **Blockchain or Distributed Ledger**
- No blockchain-based attendance records
- Centralized database sufficient
- No cryptographic verification beyond HTTPS

✗ **Facial Recognition via Social Media or Third-Party Data**
- No integration with Facebook, Google Images, or other external face databases
- Data collection limited to institutional enrollment

✗ **Disability Accommodations (Beyond Accessibility)**
- No special face recognition for individuals with facial differences
- No prosthetic recognition
- Standard face detection applies to all students

---

## Future Work

The following features are identified as potential enhancements for future phases (post-FYP):

### Phase 2: Video-Based Recognition
- Real-time video stream processing
- Continuous attendance monitoring in classroom
- Frame-level detection and recognition
- Live streaming dashboard

### Phase 2: Cross-Campus Analytics
- Aggregate reports across multiple campuses
- System-wide attendance trends
- Comparative analytics between campuses
- (Maintaining per-campus data isolation)

### Phase 2: Mobile Application
- iOS and Android native apps
- Teacher can mark attendance from mobile device
- Admin can manage enrollment from mobile
- Offline attendance (sync when online)

### Phase 2: Advanced Privacy Features
- Biometric template protection standards (ISO compliance)
- Federated learning (models trained locally per campus)
- Differential privacy for analytics
- Zero-knowledge proofs for verification

### Phase 2: Integration with School Systems
- Connect to school management system (SMS) for student data
- Sync timetable and class schedule
- Export attendance to payroll system
- Integration with student information system (SIS)

### Phase 2: Notifications and Alerts
- Email/SMS to parents for absence
- Automated absence summary reports
- Real-time alerts to teachers if attendance fails
- Parent mobile app for attendance viewing

### Phase 2: Advanced Reporting
- Predictive analytics for at-risk students
- Attendance pattern analysis
- Correlation with academic performance
- Automated reports for administrators

### Phase 2: Multi-Language Support
- UI in Urdu, English, and other local languages
- Multi-language consent forms
- Multi-language documentation

### Phase 3: Distributed Deployment
- Full production rollout across entire school network
- Central administration dashboard for school administration
- Capacity planning for 1000+ students
- Disaster recovery and business continuity

### Phase 3: Advanced Face Recognition
- Gait recognition as supplementary identification
- Age estimation for student verification
- Emotion detection (optional, for research)
- Head pose and eye gaze tracking

### Phase 3: Blockchain & Auditability
- Immutable attendance ledger on blockchain
- Cryptographic signing of attendance records
- Third-party verification of attendance
- Tamper-proof audit trail

### Future Research Topics
- Recognition accuracy across different ethnic groups (bias analysis)
- Recognition performance with makeup, hairstyles, fashion changes
- Long-term stability of embeddings (does recognition degrade over time?)
- Impact of aging on face recognition accuracy
- Recognition in extreme occlusion (full hijab, niqab)
- Seasonal variation in attendance patterns

---

## Scope Boundaries

### What Constitutes "Done" for This FYP

**System is complete when:**

1. ✓ Enrollment system captures 2–3 reference photos per student
2. ✓ Face embeddings generated and stored securely
3. ✓ Teacher dashboard accepts classroom photos
4. ✓ Face detection works on multi-face classroom photos (20–60 students)
5. ✓ Face recognition matches detected faces to enrolled students
6. ✓ Manual override interface allows teacher corrections
7. ✓ Attendance records stored and exportable
8. ✓ Accuracy evaluated across lighting, angle, distance, occlusion conditions
9. ✓ Accuracy report compiled with metrics by condition and class size
10. ✓ Privacy and security requirements documented and implemented
11. ✓ Cloud deployment working and accessible

**System is NOT complete until:**

- ✗ Full production rollout across school network
- ✗ Video-based real-time recognition
- ✗ Cross-campus data sharing or analytics
- ✗ Integration with school management systems
- ✗ Mobile applications developed
- ✗ Multi-language UI implemented
- ✗ Advanced machine learning insights provided

---

## Scope vs. Proposal Alignment

| Proposal Section | Scope Status | Notes |
|------------------|--------------|-------|
| Introduction | In Scope | Single photo, automatic detection/recognition, no scanning |
| Problem Statement | In Scope | Solves roll-call waste, proxy vulnerability, record compilation issues |
| Objectives 1-5 | In Scope | All core objectives included |
| Objective 6 (Privacy) | In Scope | Privacy requirements documented; implementation follows principles |
| Scope - In Scope | In Scope | All listed items included |
| Scope - Out of Scope | Out of Scope | Correctly excluded (video, cross-campus sharing, full rollout) |
| Recognition Pipeline | In Scope | Embedding-based matching with confidence threshold |
| System Development | In Scope | Agile iterative approach within FYP timeline |
| Accuracy Evaluation | In Scope | Structured testing with multiple conditions |
| Tools & Technologies | Partial | Open-source face recognition prioritized; commercial APIs noted but optional |
| Privacy Considerations | In Scope | All privacy principles documented |
| Expected Outcomes | In Scope | Working system, accuracy report, privacy-conscious design |

---

## Revision History

| Date | Version | Change | Author |
|------|---------|--------|--------|
| 2026-09-30 | 1.0 | Initial scope definition from proposal | Day 1 Analysis |

