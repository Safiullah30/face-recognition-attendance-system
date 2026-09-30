# FYP Proposal: Cloud-Based Face Recognition Attendance System

## 1. Introduction

Manual, roll-call attendance is still the norm in most schools and colleges across Pakistan. It consumes several minutes of every class period, is easy to falsify (proxy attendance for a friend), and produces records that are hard to audit or analyze over a term. This project proposes a cloud-based attendance system where a teacher takes a single photograph of the class, and the system automatically detects and identifies every student in the photo, marking them present in real time—no scanning, no calling names, no physical contact with a device required.

This design is particularly well-suited to gender-segregated institutions (separate boys and girls campuses, and boys-only colleges), which are common across Pakistan. Since the system only needs a photo of an already-assembled class and does not require students to individually approach a scanner or camera, it fits naturally into how attendance is already taken in these settings, without introducing new physical interaction between students and shared devices.

Alongside building the recognition pipeline, the project investigates a practical question that matters directly for real deployment: how reliably the system recognizes students under real-world classroom conditions—varying lighting, seating angle, distance from the camera, and partial occlusion (e.g., face coverings, students turned sideways)—and how the system should behave (via manual override) when confidence is low.

## 2. Problem Statement

Manual attendance-taking wastes classroom time, is vulnerable to proxy attendance, and generates records that are cumbersome to compile into reliable reports. Existing biometric alternatives (fingerprint scanners, RFID cards) require physical contact or per-student hardware, add per-device cost, and are impractical to deploy consistently across every classroom in a school. A single-photo camera-based approach removes the need for per-student hardware while still producing an accurate, auditable, cloud-stored attendance record.

## 3. Objectives

1. Design and implement a system where a single classroom photo is used to detect and recognize all present students automatically.
2. Build a secure student enrollment process that captures reference face data with consent, appropriate for gender-segregated school/college environments.
3. Store and process attendance data in the cloud, with a real-time teacher dashboard showing recognized students and flagging unrecognized faces for manual review.
4. Deploy the recognition backend on a cloud-hosted service (e.g., AWS Lambda + Rekognition, or a Python/Flask server on a cloud VM) so the system is accessible from any campus without local infrastructure.
5. Systematically test recognition accuracy under realistic classroom conditions—varying lighting, camera angle, distance, class size (30–60 students per photo), and partial occlusion.
6. Address and document the privacy, consent, and data-handling considerations specific to storing student face data in an educational setting.

## 4. Scope

### In Scope

- Teacher and admin authentication and role-based access (per-campus accounts, so boys and girls campus data remains fully separate)
- One-time student enrollment: 2–3 reference photos per student, stored as secure face embeddings (not raw images, where possible)
- Class session flow: teacher captures one classroom photo → system detects all faces → matches against enrolled students → marks present/absent
- Manual override: teacher can correct any face the system misses or misidentifies before finalizing the session
- Cloud storage of attendance records, with reporting (per-student, per-class, exportable)
- Single cloud-hosted deployment of the recognition service, accessible securely from any enrolled campus
- Structured accuracy testing across varied lighting, angle, distance, and occlusion conditions

### Out of Scope (noted as future work)

- Real-time video-based recognition (project uses single still photos per session, not continuous video)
- Cross-campus face database sharing; by design, each campus's data stays isolated
- Full production rollout across an entire school network

## 5. Literature Review (Key Areas to Cover)

The final proposal should cite recent (last 5–6 years) sources under each of the following themes:

- Face detection and recognition techniques for group/classroom photos (multi-face detection, occlusion and angle handling)
- Existing face recognition attendance systems in education—implementations, reported accuracy, and limitations
- Factors affecting face recognition accuracy in uncontrolled, real-world group photos (lighting, pose, occlusion, camera distance)
- Privacy, consent, and ethical/legal considerations of biometric data collection from students, including data minimization and storage practices
- Cloud-based storage and access-control models for sensitive, per-institution data separation

## 6. Proposed Methodology

### 6.1 Recognition Pipeline

Face detection will locate all faces in a submitted classroom photo. Each detected face is converted into a numeric embedding and compared against enrolled students' stored embeddings using similarity matching. Matches above a confidence threshold are marked present. Faces below the threshold are flagged for the teacher to confirm manually, rather than guessed automatically—this keeps the system honest about its limits.

### 6.2 System Development

An agile, iterative approach: build enrollment → recognition pipeline → attendance session flow → dashboard/reporting incrementally, testing each stage with real sample class photos before moving on.

### 6.3 Accuracy Evaluation

The recognition pipeline will be tested against a structured set of classroom photos captured under varying conditions:

- Different lighting (bright/dim)
- Seating angles
- Camera distances
- Partial occlusion (e.g., turned faces, face coverings)

For each condition, correct identifications, missed detections, and false matches will be recorded against manually-verified ground truth, and results compiled into an accuracy report broken down by condition and by class size (20, 40, 60 students per photo).

## 7. Tools & Technologies

| Layer | Technology | Notes |
|-------|-----------|-------|
| Frontend | React.js | Teacher/admin enrollment UI, dashboard |
| Face Recognition Detection | AWS Rekognition & Python + face_recognition (dlib) | Pre-trained models; no training from scratch needed |
| Backend | Python/Flask or Node.js/Express, cloud-hosted | Deployed on cloud VM (e.g., EC2/DigitalOcean) or AWS Lambda + Rekognition |
| Database | Firebase Firestore or AWS DynamoDB | Attendance records, embeddings stored separately, encrypted |
| Storage | AWS S3 / Firebase Storage | Enrollment photos, controlled per-campus access |
| Authentication | Firebase Auth or AWS Cognito | Role-based: admin/teacher, isolated per campus |
| Testing | Custom Python scripts, manual ground-truth review | For the accuracy evaluation study |

## 8. Privacy & Ethical Considerations

Because this system handles student biometric data, the proposal explicitly addresses:

- **Informed Consent:** Students (and/or guardians, for minors) must consent to enrollment before their face data is captured.
- **Data Minimization:** Store face embeddings (mathematical representations) rather than raw photos wherever feasible, and encrypt stored data.
- **Per-Campus Data Isolation:** Boys' and girls' campus data are stored and accessed completely separately, with no cross-campus access.
- **Retention & Deletion Policy:** Define how long data is kept and how a student's data is removed if they leave the institution.
- **Human-in-the-Loop:** The system flags uncertain matches for teacher confirmation rather than making silent automatic decisions.

## 9. Expected Outcomes

- A working, deployed system where a single classroom photo produces accurate, real-time attendance for an entire class.
- A documented, data-backed accuracy report showing how recognition performance holds up under real classroom conditions.
- A privacy-conscious design suitable for real deployment at gender-segregated campuses.
- A final report and defense presentation covering design, implementation, accuracy results, and benchmark data.
