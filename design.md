# DeadLine AI – System Design Document

## 1. Architecture Overview

DeadLine AI is built as a cloud-native, serverless-first system consisting of:

- Ingestion Layer (PDF and calendar connectors)
- LLM Extraction & Enrichment Engine
- Prioritization & Scheduling Service
- Notification Service (WhatsApp)
- Data Storage Layer
- Admin & Analytics Interface [file:12]

The goal is a low-cost MVP on AWS that can scale with usage while staying simple to maintain. [file:12]

## 2. Components

### 2.1 Ingestion Layer

- **PDF Ingestion Service**
  - Accepts syllabus PDFs uploaded by students.
  - Uses an AWS Lambda function to extract raw text (e.g., via Textract or equivalent). [file:12]
  - Sends extracted text to the LLM Extraction Engine.

- **Calendar Connector**
  - Integrates with Google Calendar APIs (OAuth-based).
  - Pulls upcoming events and descriptions for selected calendars.
  - Normalizes events into a common “Task Candidate” format. [file:12]

### 2.2 LLM Extraction & Enrichment Engine

- Built using AWS Bedrock / Anthropic Claude to parse unstructured syllabus and event text. [file:12]
- Responsibilities:
  - Identify tasks (assignments, labs, quizzes, exams, projects). [file:12]
  - Extract key fields: course name, task type, title, due date/time, weightage if available. [file:12]
  - Infer **effort level** from wording (e.g., “lab report”, “project”, “MCQ quiz”). [file:12]
- Output format (per task):
  - `task_id`, `user_id`, `course`, `task_type`, `title`, `due_datetime`, `effort_score`, `raw_source`. [file:12]

### 2.3 Prioritization & Scheduling Service

- Uses a sorting algorithm that combines:
  - **Urgency**: Time remaining until due date. [file:12]
  - **Effort**: Estimated work required (e.g., high effort if it’s a project or lab; low for a short quiz). [file:12]
- Produces:
  - A prioritized task list (e.g., “Do Lab Report (High Effort, due in 2 days)” before “Quiz (Low Effort, due tomorrow)” if total time needed is higher). [file:12]
  - Reminder schedules (T‑3 days, T‑1 day, T‑X hours) adapted per task type. [file:12]
- Implemented as a stateless Lambda function that reads tasks from DynamoDB, computes scores, and updates task records. [file:12]

### 2.4 Notification Service (WhatsApp Layer)

- Integrates with WhatsApp Business API to send:
  - Daily “smart to‑do” lists in the user’s chosen language (Hindi/English). [file:12]
  - Task-specific reminders with due time and short study tip. [file:12]
- Language handling:
  - User profile stores preferred language.
  - Template messages are localized; generated tips may be translated via LLM. [file:12]

### 2.5 AI-Generated Study Tips Module

- Given a task description and course context, the LLM generates:
  - A 1–2 line study tip or starting guideline (e.g., “Re-read Chapter 3 and outline key formulas before starting the lab”). [file:12]
- Integrated into the notification payload for WhatsApp. [file:12]

### 2.6 Data Storage Layer

- **Amazon DynamoDB**
  - Stores user profiles, tasks, prioritization metadata, and reminder schedules. [file:12]
  - Partitioned by `user_id` for fast access.
- Audit logs (e.g., notification history) can be stored in separate tables for analytics. [file:12]

### 2.7 Admin & Analytics Interface (Optional for MVP)

- Web dashboard (simple SPA) to:
  - View anonymized stats: tasks created, reminders sent, completion rates.
  - Monitor system health (error rates, failed notifications). [file:12]

## 3. System Flow

1. Student uploads a syllabus PDF or connects Google Calendar. [file:12]
2. Ingestion Layer processes the input and extracts raw text/events. [file:12]
3. LLM Extraction Engine converts raw content into structured tasks with effort scores. [file:12]
4. Prioritization Service assigns urgency scores and orders tasks. [file:12]
5. Tasks and schedules are saved to DynamoDB. [file:12]
6. Notification Service sends multilingual WhatsApp reminders and daily prioritized to‑do lists. [file:12]
7. Admin/analytics tools aggregate usage data for improvement. [file:12]

## 4. Scalability & Cost Considerations

- Heavy processing (LLM calls, parsing) runs on AWS Lambda with pay-per-use model. [file:12]
- DynamoDB provides auto-scaling storage and throughput for task data. [file:12]
- WhatsApp API costs are controlled by:
  - Batching notifications where possible.
  - Allowing users to opt in/out and configure frequency. [file:12]
- Initial deployment targets the AWS Free Tier for an early-stage pilot. [file:12]
