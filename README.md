# DeadLine AI – Smart Academic Deadline Assistant

DeadLine AI is an intelligent assistant that helps students manage academic workloads by automatically extracting deadlines from syllabus PDFs and calendars, prioritizing them by effort and urgency, and sending smart reminders in local languages. [file:12]

## Problem

Students juggle multiple courses, assignments, labs, and exams, often across different platforms and formats. [file:12] Manually tracking every deadline leads to stress, missed submissions, and last-minute cramming. [file:12]

## Solution Overview

DeadLine AI eliminates manual tracking by:

- Scanning syllabus PDFs and Google Calendars to detect tasks and deadlines. [file:12]
- Classifying tasks by **effort** (high/medium/low) and **urgency** (immediate/soon/later). [file:12]
- Generating a dynamic, personalized roadmap instead of a static list of dates. [file:12]
- Sending multilingual WhatsApp reminders with quick study tips so students and their support systems stay aligned. [file:12]

## Key Features

- **Automated PDF/Calendar ingestion**: Upload a syllabus or connect a calendar to auto-detect assignments, labs, quizzes, and exams. [file:12]
- **Effort + urgency prioritization**: Tasks are categorized (e.g., “High Effort Lab”, “Quick Quiz”) with smart ordering. [file:12]
- **Multilingual WhatsApp reminders**: Notifications in English and Hindi, with friendly task summaries. [file:12]
- **AI-generated study tips**: Short, contextual tips to help students start a task quickly. [file:12]
- **Offline/low-bandwidth friendly**: Designed for Indian scenarios where connectivity is spotty. [file:12]

## High-Level Architecture

- Input sources: Syllabus PDFs, Google Calendar events. [file:12]
- LLM extraction: Uses an LLM (e.g., AWS Bedrock / Anthropic Claude) to extract tasks, due dates, and effort hints. [file:12]
- Sorting & prioritization: Custom algorithm to compute effort and urgency scores per task. [file:12]
- Notification layer: WhatsApp Business API sends localized reminders and to‑do lists. [file:12]
- Storage: Amazon DynamoDB stores user profiles, tasks, and reminder schedules. [file:12]

For a detailed technical breakdown, see `design.md`. For user stories and acceptance criteria, see `requirements.md`.
