# AI-Based ATS Resume Analyzer — Project Requirements

## 1. Project Status

- **Phase:** Ongoing development
- **Current milestone:** Day 1 — Project scope finalization
- **Working title:** AI-Based ATS-Friendly Resume Evaluation and Improvement System

## 2. Project Overview

The system will analyze a candidate's resume against a supplied job description and generate an **ATS-readiness estimate**. It will combine document parsing, section detection, keyword matching, semantic similarity, formatting checks, and explainable recommendations.

The tool is intended to help users improve their resumes before applying for a job. It does **not** reproduce the private ranking logic of any particular employer or Applicant Tracking System (ATS), and it does not guarantee selection, interviews, or employment.

## 3. Day 1 Learning Summary

### 3.1 What an ATS Does

An Applicant Tracking System helps employers collect, organize, search, filter, and review job applications. Depending on the product and employer configuration, an ATS may parse resume content, identify sections and skills, support recruiter searches, apply screening questions, or rank candidates using job-related criteria.

### 3.2 Keyword Matching

Keyword matching checks whether words or phrases from the job description also appear in the resume.

Example:

- Job description: `Java`, `Spring Boot`, `REST API`
- Resume: `Java`, `Spring Boot`, `RESTful services`

This method is fast and explainable but may miss equivalent expressions, abbreviations, synonyms, and contextual meaning.

### 3.3 Semantic Matching

Semantic matching compares the meaning of resume content with the job description. It can recognize that phrases such as “developed backend web services” and “built REST APIs” are closely related even when the exact words differ.

Semantic matching should supplement keyword matching rather than replace it. Exact skills, certifications, tool names, and mandatory qualifications still require explicit verification.

### 3.4 Why the Result Is Only an ATS-Readiness Estimate

There is no universal ATS scoring formula. Employers use different platforms, configurations, screening questions, search filters, recruiter preferences, and hiring criteria. The system therefore provides a transparent estimate based on observable resume quality and job-description alignment. Its score must always be labelled **ATS-readiness estimate**, not “actual ATS score,” “selection probability,” or “guaranteed result.”

## 4. Project Objectives

The system shall:

1. Accept a resume in PDF or DOCX format.
2. Accept a target job description as text.
3. extract readable resume text and identify common sections.
4. Compare resume skills and content with job requirements.
5. Identify important missing skills or concepts.
6. Perform basic ATS-related formatting checks.
7. Generate an explainable readiness score and score breakdown.
8. Provide prioritized, evidence-based improvement suggestions.

## 5. Target Users

- Students and fresh graduates
- Internship applicants
- Job seekers tailoring resumes to specific roles
- Placement-cell users seeking preliminary resume feedback

## 6. Functional Requirements

### FR-01: Resume Upload

- The system shall accept `.pdf` and `.docx` files.
- It shall validate the extension, MIME type, file size, and whether meaningful text can be extracted.
- It shall reject unsupported, empty, corrupted, or password-protected files with a clear message.
- For the first version, image-only or scanned resumes may be reported as unsupported unless OCR is added.

### FR-02: Job-Description Input

- The user shall be able to paste a job description into a text field.
- The system shall validate that the input contains sufficient meaningful text.
- The system shall extract relevant skills, qualifications, responsibilities, experience expectations, and role-specific terms.

### FR-03: Resume Text and Section Extraction

- The system shall extract text while preserving useful line and paragraph boundaries where possible.
- It shall attempt to identify: Contact Information, Summary/Objective, Skills, Education, Experience, Projects, Certifications, and Achievements.
- It shall show which expected sections were found, missing, or uncertain.

### FR-04: Skill Matching

- The system shall normalize skill names, capitalization, and common aliases.
- It shall compare resume skills with job-description skills.
- Results shall separate exact matches, normalized/alias matches, and semantic matches.
- A semantic match must not be presented as proof that the candidate possesses an explicitly required skill.

### FR-05: Missing-Skill Detection

- The system shall identify job-relevant skills or qualifications that are not supported by resume evidence.
- Missing items shall be prioritized as required, preferred, or low priority when the job description provides enough context.
- The system shall never encourage users to claim skills or experience they do not possess.
- Suggestions shall recommend adding a missing skill only if the user genuinely has that skill.

### FR-06: Explainable Scoring

- The result shall be displayed as an ATS-readiness estimate from 0 to 100.
- Every score shall include a category-level breakdown and reasons for deductions.
- The first-version scoring weights shall be configurable rather than permanently hard-coded.
- Proposed initial scoring model:

| Category | Weight |
| --- | ---: |
| Job-description keyword and skill coverage | 30% |
| Semantic relevance | 25% |
| Resume section completeness | 15% |
| ATS-readable formatting | 15% |
| Experience/project evidence and impact | 10% |
| Language clarity and action-oriented writing | 5% |

- Suggested interpretation:

| Score | Meaning |
| --- | --- |
| 80–100 | Strong estimated ATS readiness |
| 60–79 | Moderate readiness; targeted improvements recommended |
| 0–59 | Significant alignment, content, or formatting improvements needed |

### FR-07: Formatting Checks

- The system shall check for conditions that may reduce reliable parsing, including complex multi-column layouts, tables, text boxes, headers/footers containing important information, excessive graphics or icons, unusual section headings, missing contact information, and unreadable or empty extracted text.
- Checks shall be phrased as potential risks because parser behavior varies across ATS products.
- The initial version shall not claim perfect visual-layout analysis when only extracted text is available.

### FR-08: Resume-Improvement Suggestions

- The system shall provide prioritized and actionable suggestions.
- Suggestions may cover missing sections, better skill alignment, clearer wording, measurable outcomes, strong action verbs, job-relevant ordering, and formatting risks.
- Rewritten bullet suggestions shall preserve the user's facts and must not invent metrics, technologies, responsibilities, qualifications, or achievements.
- Each suggestion should explain why the change may improve readability or job alignment.

### FR-09: Results Report

- The report shall contain the overall estimate, category scores, matched skills, missing skills, detected sections, formatting warnings, semantic relevance, and improvement suggestions.
- The report shall distinguish evidence found in the resume from inferred semantic relevance.
- Error and uncertainty messages shall be understandable to non-technical users.

## 7. Proposed Processing Workflow

`Resume + Job Description → File Validation → Text Extraction → Section Detection → Job Requirement Extraction → Keyword/Skill Matching → Semantic Matching → Formatting Checks → Explainable Score → Improvement Suggestions`

## 8. Non-Functional Requirements

### Accuracy and Transparency

- The same inputs and configuration should produce consistent results.
- The system shall expose scoring weights, detected evidence, and reasons for recommendations.
- Unsupported conclusions and fabricated resume content are prohibited.

### Performance

- A typical one- or two-page resume should be analyzed within a reasonable interactive response time.
- Slow stages should provide a visible processing status in the user interface.

### Security and Privacy

- Uploaded resumes shall be treated as sensitive personal data.
- The application shall validate files and sanitize extracted content.
- Files and extracted text should not be retained longer than required unless the user explicitly chooses to save them.
- Sensitive values shall not be written to application logs.

### Usability and Accessibility

- The interface shall provide clear upload instructions, validation messages, score explanations, and prioritized recommendations.
- The core workflow should be usable with keyboard navigation and readable labels.

### Maintainability

- Text extraction, section detection, skill matching, semantic matching, scoring, formatting checks, and suggestions should be separate modules.
- Skill dictionaries, thresholds, aliases, and scoring weights should be configurable.

## 9. Scope Boundaries for the First Version

### Included

- English-language resumes and job descriptions
- Text-based PDF and DOCX resumes
- Common resume-section extraction
- Hybrid keyword and semantic matching
- Basic formatting-risk detection
- Explainable scoring and improvement suggestions

### Excluded or Deferred

- Guaranteed reproduction of a commercial ATS score
- Recruiter or employer decision prediction
- Automatic job application submission
- Fabrication of candidate information
- Full visual redesign of resumes
- OCR for scanned/image-only resumes
- Multilingual analysis
- Employer-specific ATS integrations

## 10. Suggested Technical Direction

- **PDF extraction:** PyMuPDF or `pdfplumber`
- **DOCX extraction:** `python-docx`
- **Text processing:** Python, regular expressions, and optionally spaCy
- **Semantic matching:** Sentence Transformers/Sentence-BERT embeddings with cosine similarity
- **Backend:** Flask
- **Frontend:** HTML, CSS, and JavaScript
- **Testing:** Pytest with sample PDF, DOCX, job-description, and malformed-file cases

The final library or model choice should be confirmed during implementation based on licensing, local resource needs, accuracy, latency, and deployment constraints.

## 11. Acceptance Criteria for the Minimum Viable Product

The MVP is complete when:

1. A user can upload a valid text-based PDF or DOCX resume.
2. A user can enter a meaningful job description.
3. The application extracts resume text and identifies common sections.
4. It lists exact/normalized skill matches and separately identifies semantic matches.
5. It lists prioritized missing requirements without encouraging false claims.
6. It reports basic parsing and formatting risks.
7. It produces a 0–100 ATS-readiness estimate with a visible category breakdown.
8. It gives actionable suggestions linked to detected evidence or missing content.
9. It clearly displays the limitation that the result is an estimate and not a hiring guarantee.
10. It handles unsupported or invalid input without crashing.

## 12. Day 1 Deliverable Checklist

- [x] Define what an ATS does.
- [x] Explain keyword matching versus semantic matching.
- [x] State why the score is only an ATS-readiness estimate.
- [x] Define the eight core product features.
- [x] Establish first-version scope and exclusions.
- [x] Add functional and non-functional requirements.
- [x] Define MVP acceptance criteria.

## 13. Recommended Day 2 Goal

Design the project architecture and folder structure, select parsing libraries, define the result JSON schema, and create representative test resumes and job descriptions before implementing the analysis pipeline.
