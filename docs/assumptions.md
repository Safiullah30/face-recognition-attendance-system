# Assumptions and Ambiguities
## Cloud-Based Face Recognition Attendance System

**Date:** 2026-09-30  
**Status:** Day 1 - Requirements Analysis

---

## Explicit Assumptions

### User Assumptions

**ASSUM-USER-001:** Students are enrolled with 2–3 reference photos captured in controlled lighting
- Implication: Initial enrollment photos are higher quality than real-world classroom photos
- Impact: Face embeddings may not generalize well to poor lighting in classroom attendance sessions

**ASSUM-USER-002:** Teachers are willing to manually review and correct system matches
- Implication: System does NOT need to be 100% accurate; human-in-the-loop is acceptable
- Impact: Lower confidence threshold is acceptable if teacher can catch errors

**ASSUM-USER-003:** Teachers have basic technical literacy (can upload photos, click buttons)
- Implication: UI does not need to support voice commands, gesture recognition, etc.
- Impact: Standard web UI is sufficient

**ASSUM-USER-004:** Students appear in classroom photos in recognizable poses
- Implication: System does not need to handle extreme poses (back of head, face completely turned away)
- Impact: Some students may require manual marking if head is completely turned

**ASSUM-USER-005:** Each campus has only one gender (either boys or girls, not mixed)
- Implication: Gender-segregation is enforced by institution structure, not system
- Impact: No need for gender-based filtering; campus isolation is sufficient

**ASSUM-USER-006:** Admins perform one-time enrollment of all students at semester start
- Implication: Re-enrollment is rare (only for new students mid-term, or if photos are poor)
- Impact: Enrollment system does not need high throughput

### Technical Assumptions

**ASSUM-TECH-001:** Open-source face detection models (e.g., MediaPipe, MTCNN) are sufficient for classroom photos
- Implication: No need for commercial APIs like AWS Rekognition (per Day 1 constraints)
- Impact: Face detection accuracy depends on model choice; must be tested

**ASSUM-TECH-002:** Face embedding models (e.g., FaceNet, ArcFace) can be run locally or on cloud VM
- Implication: No requirement for specialized hardware (GPU) if throughput is acceptable
- Impact: CPU-only inference may be slower; latency depends on image size and model

**ASSUM-TECH-003:** Supabase PostgreSQL Free Tier is sufficient for data storage
- Implication: No need for expensive database licenses or AWS DynamoDB
- Impact: Free tier has storage and performance limits; may need optimization

**ASSUM-TECH-004:** React.js is appropriate for teacher/admin UI
- Implication: Desktop web browser is primary platform (no mobile app required)
- Impact: Mobile responsiveness helpful but not critical

**ASSUM-TECH-005:** Python/Flask backend can be deployed on free cloud tier (e.g., Vercel, Heroku free tier)
- Implication: Startup time and cold starts acceptable for attendance workflow
- Impact: First request after inactivity may be slow; acceptable for daily attendance sessions

**ASSUM-TECH-006:** HTTPS and standard encryption (AES-256) provide sufficient security for biometric data
- Implication: No specialized hardware security modules (HSMs) required
- Impact: Keys must be managed securely in environment variables; no HSM backup

**ASSUM-TECH-007:** Classroom photos are uploaded as JPEG or PNG, up to 5-10 MB
- Implication: No support for RAW images, video files, or very large photos
- Impact: Image size limits should be enforced in UI

### Data Assumptions

**ASSUM-DATA-001:** Each student is enrolled in only one campus
- Implication: No support for transferring between campuses or dual-campus enrollment
- Impact: Student record deletion is simple (no cross-campus cleanup)

**ASSUM-DATA-002:** Each teacher works for only one campus
- Implication: No support for teachers teaching at multiple campuses
- Impact: User sessions are tied to single campus

**ASSUM-DATA-003:** Class identifiers are unique within a campus
- Implication: No need for globally unique class IDs; campus-local IDs sufficient
- Impact: Foreign key design includes campus_id + class_id

**ASSUM-DATA-004:** Attendance sessions are one-time per class per day
- Implication: No repeated attendance sessions for same class on same day (no rollcall recount)
- Impact: (class_id, date) is unique key for attendance session

**ASSUM-DATA-005:** Face embeddings are deterministic (same photo always produces same embedding)
- Implication: If re-processing enrollment photo, same embedding is generated
- Impact: No need to store multiple embeddings for redundancy

**ASSUM-DATA-006:** Ground truth labels for test dataset are created manually and independently verified
- Implication: No algorithmic labeling of test data; human experts label all test photos
- Impact: Testing will be time-consuming; test dataset size limited by manual effort

### Environmental Assumptions

**ASSUM-ENV-001:** Pakistan institutions operate in standard classrooms with artificial or natural lighting
- Implication: Extreme conditions (pitch black, strobe lighting) are out of scope
- Impact: Testing covers "bright" and "dim" but not edge cases

**ASSUM-ENV-002:** Classroom cameras have ~1-2 meter working distance
- Implication: Camera is mounted at front of room or hand-held
- Impact: Extreme distances (students 10m+ away) not tested

**ASSUM-ENV-003:** Classroom attendance is conducted during daytime hours (approximately 7 AM - 5 PM)
- Implication: No extreme low-light scenarios at night
- Impact: Nighttime attendance not required

**ASSUM-ENV-004:** Gender-segregated institutions enforce separation via campus structure
- Implication: No mixed-gender classrooms
- Impact: System does not need to handle gender diversity within single classroom

**ASSUM-ENV-005:** Classroom photos are taken indoors (no outdoor or extreme weather conditions)
- Implication: Weather, rain, sun glare not concerns
- Impact: Simplified environmental assumptions

### Project/Scope Assumptions

**ASSUM-SCOPE-001:** Project focuses on accuracy evaluation, not on full production deployment
- Implication: System is a proof-of-concept or research prototype, not production-grade
- Impact: Scalability beyond 100-200 students per campus not required

**ASSUM-SCOPE-002:** Testing is conducted with volunteer students and carefully prepared test datasets
- Implication: Not real-world continuous monitoring; controlled lab conditions
- Impact: Results may not reflect production accuracy; more testing needed before deployment

**ASSUM-SCOPE-003:** Privacy compliance is Pakistani institutional context only
- Implication: Not GDPR, CCPA, or other international compliance
- Impact: Consent mechanisms follow Pakistani norms; may not be portable to other regions

---

## Important Ambiguities (Decisions Deferred to Day 2+)

### Ambiguity 1: Confidence Threshold Value
**Question:** What confidence score threshold should distinguish "auto-mark present" from "flag for manual review"?

**Impact:** 
- Too high (e.g., 95%): Many students flagged for review, teacher workload increases
- Too low (e.g., 60%): More false positives, teacher catches fewer errors
- Critical for balancing false positives vs. false negatives

**Decision Required:** Day 2 (Technical Design)  
**Stakeholders:** Teachers (who perform reviews), project lead

---

### Ambiguity 2: Enrollment Photo Quality & Quantity
**Question:** Are 2–3 reference photos sufficient, or should more be captured? Should photos be from different lighting conditions?

**Impact:**
- More photos → better face embeddings but higher enrollment time
- Variety in lighting → better generalization to classroom conditions
- Fixed at 2–3 per proposal, but quantity-vs-quality tradeoff not resolved

**Decision Required:** Day 2 (Technical Design) + Day 3 (Testing)  
**Stakeholders:** Admins (conduct enrollment), face recognition engineers

---

### Ambiguity 3: Face Embedding Storage Strategy
**Question:** Store one averaged embedding per student, or separate embeddings for each reference photo?

**Impact:**
- One averaged: simpler matching logic, less storage, but loses photo-specific information
- Multiple embeddings: richer representation, better matching, but more storage and complex matching logic

**Decision Required:** Day 2 (Technical Design)  
**Stakeholders:** Backend engineers, database designer

---

### Ambiguity 4: Classroom Photo Retention
**Question:** Should raw classroom photos used for attendance be retained, or deleted after processing?

**Impact:**
- Retain: allows re-processing if embedding algorithm changes, but increases privacy concerns and storage
- Delete: reduces privacy exposure and storage, but prevents re-processing

**Decision Required:** Day 2 (Technical Design) + Privacy review  
**Stakeholders:** Privacy officer, teachers, system architects

---

### Ambiguity 5: Attendance Record Data Retention Period
**Question:** How long should attendance records be kept? Delete automatically after term, year, or forever?

**Impact:**
- Long retention: supports academic appeals, historical analysis
- Short retention: reduces privacy exposure, data breach impact
- Not specified in proposal; no legal mandate given

**Decision Required:** Day 2 (Technical Design) + Legal/Admin input  
**Stakeholders:** School administration, legal counsel

---

### Ambiguity 6: Consent Capture Mechanism
**Question:** How is informed consent recorded? Digital signature, checkbox, printed form with photo, QR code, verbal confirmation?

**Impact:**
- Digital: easier but less culturally familiar in Pakistan
- Printed: more formal and culturally appropriate but harder to audit
- Verbal: simplest but hardest to prove consent was given

**Decision Required:** Day 2 (Technical Design) + Cultural/Legal review  
**Stakeholders:** Legal counsel, school administrators, students/parents

---

### Ambiguity 7: Multi-Campus Teacher Support
**Question:** Can a teacher teach at multiple campuses (e.g., contract teacher at 2 schools)? If so, how are separate logins handled?

**Impact:**
- One campus per login: simpler, but teacher must logout/login to switch
- Multiple campuses per user: complex access control, but more convenient
- Proposal assumes one campus per user, but ambiguous

**Decision Required:** Day 2 (Technical Design)  
**Stakeholders:** School administration, teachers, system architects

---

### Ambiguity 8: Admin Account Hierarchy
**Question:** Is there a single super-admin across all campuses, or separate admins per campus only?

**Impact:**
- Super-admin: central control, but breaks campus isolation principle
- Per-campus admins only: maintains isolation, but no cross-campus admin functions possible

**Decision Required:** Day 2 (Technical Design)  
**Stakeholders:** School leadership, system architects

---

### Ambiguity 9: Manual Override Logging Detail
**Question:** What level of detail is logged for manual overrides? Just "changed Present to Absent" or also include reason/comment?

**Impact:**
- Minimal logging: simple but limits auditability
- Detailed logging: better audit trail but adds UI complexity

**Decision Required:** Day 2 (Technical Design)  
**Stakeholders:** Auditors, teachers (who make overrides), admins

---

### Ambiguity 10: Accuracy Targets
**Question:** What recognition accuracy must the system achieve? 90%? 95%? Per-class average or per-student minimum?

**Impact:**
- High target (95%+): requires expensive models and extensive tuning
- Lower target (70-80%): acceptable for human-in-the-loop system
- Proposal does not set target; must be defined experimentally

**Decision Required:** Day 3 (Testing + Initial Accuracy Results)  
**Stakeholders:** Project lead, school administration, teachers

---

### Ambiguity 11: Student Data Deletion
**Question:** When a student is deleted, should attendance records be kept (anonymized) or deleted entirely?

**Impact:**
- Keep anonymized: supports historical analysis but raises privacy concerns
- Delete entirely: cleaner for GDPR-like compliance but loses data
- Conflicts with audit trail requirements

**Decision Required:** Day 2 (Technical Design) + Legal review  
**Stakeholders:** Legal counsel, administrators, privacy officer

---

### Ambiguity 12: Multi-Admin Per Campus
**Question:** Can a campus have multiple admins, or single admin per campus?

**Impact:**
- Multiple admins: better resilience and load sharing, but complex access control and audit trails
- Single admin: simpler but single point of failure

**Decision Required:** Day 2 (Technical Design)  
**Stakeholders:** School administration, system architects

---

### Ambiguity 13: Export Report Formats
**Question:** What specific export formats are required? CSV, Excel, PDF, JSON?

**Impact:**
- CSV: simplest, but limited formatting
- Excel: users familiar with spreadsheets, but library dependency
- PDF: good for formal reports, but library dependency
- Proposal just says "exportable"

**Decision Required:** Day 2 (Technical Design)  
**Stakeholders:** Teachers (who use reports), administrators

---

### Ambiguity 14: Accuracy Testing - Sample Size
**Question:** How many test photos per condition (lighting, angle, distance, occlusion)? 10? 50? 100?

**Impact:**
- Small sample (10): quick testing but noisy results
- Large sample (100+): statistically robust but time-consuming
- Proposal does not specify; must balance thoroughness vs. time

**Decision Required:** Day 3 (Testing Planning)  
**Stakeholders:** Project lead, testing team

---

### Ambiguity 15: Face Detection Thresholding
**Question:** Should low-confidence face detections (e.g., partially visible faces) be included in matching or discarded?

**Impact:**
- Include: catches edge cases but may include false detections
- Discard: reduces false detections but may miss partially visible students
- Affects both false positive and false negative rates

**Decision Required:** Day 2 (Technical Design) + Testing  
**Stakeholders:** Face recognition engineers, testers

---

## Technical Decisions Required Before Implementation

| Decision | Impact | Timeline |
|----------|--------|----------|
| Confidence threshold value | System behavior, teacher workload | Day 2 |
| Embedding storage strategy | Database design, matching logic | Day 2 |
| Classroom photo retention | Privacy, storage, re-processing ability | Day 2 |
| Consent capture mechanism | Legal compliance, UX | Day 2 + Legal review |
| Attendance record retention | Privacy, storage, analytics | Day 2 + Admin input |
| Multi-campus teachers | Access control design | Day 2 |
| Admin hierarchy (super-admin?) | System governance | Day 2 |
| Export report format | UI design, library choices | Day 2 |
| Accuracy targets | Testing scope, deployment readiness | Day 3 after initial testing |
| Face detection thresholding | False positive/negative rates | Day 2 + Testing |

---

## Risks Related to Ambiguities

**Risk 1: Over-Flagging**
- If confidence threshold is too high, teacher must review many faces
- Teachers become fatigued and make mistakes
- Mitigation: Start with lower threshold, adjust based on feedback

**Risk 2: Silent Failures**
- If confidence threshold is too low, false positives (absent marked present) go unnoticed
- Proxy attendance possible if system confidence is high for wrong student
- Mitigation: Log all matches with confidence scores; implement audit mechanism

**Risk 3: Enrollment Quality Variability**
- If enrollment photos are poor quality, embeddings are weak
- Attendance recognition fails even though student is present
- Mitigation: Quality control during enrollment; retake photos if needed

**Risk 4: Privacy Compliance Gaps**
- Consent mechanism unclear; may not be legally valid
- Retention policy undefined; may violate institutional policy or privacy law
- Mitigation: Legal review early; document all privacy decisions

**Risk 5: Testing Bias**
- If test photos are all from same camera, angle, or students, results may not generalize
- Accuracy report may be misleading
- Mitigation: Diverse test dataset; multiple independent evaluators

---

## Open Questions for Stakeholder Discussion

1. **What is the maximum acceptable teacher workload for manual review?** (e.g., review <5% of faces, or <20 faces per session)

2. **Should the system prioritize reducing false positives (avoid marking absent as present) or false negatives (avoid missing present students)?**

3. **Is re-taking enrollment photos an acceptable process if initial quality is poor?**

4. **What is the target deployment date? Does testing timeline allow for iterative improvements?**

5. **Should the system support batch uploads of classroom photos, or single-photo sessions only?**

6. **Are there specific face coverings (e.g., hijabs, niqabs) the system must recognize? Or is partial occlusion testing sufficient?**

7. **Can teachers contact technical support if recognition fails completely for a class?** (e.g., abort session and fall back to manual roll-call)

8. **What is the expected class size range?** Proposal says 20-60; is this realistic for all classes at target institutions?

