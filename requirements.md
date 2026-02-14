# DeadLine AI – Requirements Document

## 1. Problem Statement

Students often struggle with heavy coursework and tight deadlines across multiple subjects. This results in stress, cognitive overload, and late submissions because tracking is manual and fragmented across PDFs and calendars. [file:12]

## 2. Goals & Objectives

- Automatically collect academic deadlines from common sources (syllabus PDFs, Google Calendars). [file:12]
- Prioritize tasks using a smart effort–urgency model instead of simple date ordering. [file:12]
- Deliver timely, multilingual reminders through WhatsApp to reduce missed deadlines. [file:12]
- Work reliably in low-bandwidth, real-world Indian conditions. [file:12]

## 3. User Roles

- **Student**
  - Uploads syllabi, connects calendars, configures language and reminder preferences.
  - Receives prioritized to‑do lists and reminders. [file:12]

- **Admin (Internal)**
  - Monitors system metrics and error logs.
  - Adjusts prioritization rules and notification templates. [file:12]

## 4. Functional Requirements

### 4.1 Onboarding & Configuration

- The system shall allow students to sign up with a phone number linked to WhatsApp. [file:12]
- The system shall allow students to select preferred language (Hindi/English for MVP). [file:12]
- The system shall allow students to configure reminder frequency (e.g., daily digest, only near due dates). [file:12]

### 4.2 Input Ingestion

- The system shall allow students to upload syllabus PDFs through a web or mobile interface. [file:12]
- The system shall extract text from PDFs and forward it to the LLM extraction module. [file:12]
- The system shall allow users to connect a Google Calendar and pull upcoming events. [file:12]

### 4.3 Task Extraction & Structuring

- The system shall detect academic tasks such as assignments, labs, quizzes, projects, and exams from PDF and calendar content. [file:12]
- For each detected task, the system shall store:
  - Course name, task type, title/short description.
  - Due date and time.
  - Source (PDF or calendar). [file:12]

### 4.4 Effort & Urgency Prioritization

- The system shall estimate an effort score (e.g., Low, Medium, High) for each task based on description cues. [file:12]
- The system shall compute an urgency score based on time remaining until due date. [file:12]
- The system shall compute a combined priority score and order tasks accordingly for the user. [file:12]
- The system shall update priorities when new tasks are added or due dates change. [file:12]

### 4.5 Notifications & WhatsApp Integration

- The system shall send WhatsApp reminders for upcoming tasks to the student’s registered number. [file:12]
- The system shall support template-based notification messages in Hindi and English. [file:12]
- The system shall send a daily to‑do list with the top N tasks based on priority. [file:12]
- The system shall send at least one reminder within 24 hours of a task’s due time, unless the user opts out. [file:12]

### 4.6 AI-Generated Study Tips

- The system shall generate a short (1–2 line) study tip for each task using the LLM. [file:12]
- The system shall include the study tip in at least one reminder per task. [file:12]

### 4.7 Offline/Low-Bandwidth Handling

- The system shall be resilient to intermittent connectivity by:
  - Queuing notifications and sending when connectivity resumes.
  - Minimizing payload sizes in WhatsApp messages. [file:12]

## 5. Non-Functional Requirements

- **Performance**
  - From PDF upload to task list generation should complete within 60–120 seconds for typical syllabi. [file:12]
- **Scalability**
  - The system should support thousands of users with serverless auto-scaling on AWS. [file:12]
- **Reliability**
  - Notification delivery success rate should be at least 95% for valid WhatsApp numbers. [file:12]
- **Cost**
  - Initial deployment should fit within or close to AWS Free Tier usage, using serverless and on-demand LLM calls. [file:12]
- **Security & Privacy**
  - Store only necessary academic data and phone numbers.
  - Do not share user data with third parties beyond required APIs (e.g., WhatsApp, Google Calendar). [file:12]

## 6. Future Enhancements

- Support more Indian languages beyond Hindi and English. [file:12]
- Add simple student-facing dashboard to visualize all tasks, completion status, and workload heatmaps. [file:12]
- Integrate with LMS platforms (e.g., Google Classroom, Moodle) for automatic task ingestion. [file:12]
- Add parental/guardian notification options for specific tasks or risk levels. [file:12]
