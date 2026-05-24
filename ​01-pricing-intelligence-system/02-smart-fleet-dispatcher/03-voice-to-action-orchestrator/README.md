# Project 3: Voice-to-Action Enterprise Task Orchestrator

## 📌 Project Overview
This project targets corporate administrative friction by automating meeting transcription and task deployment. It acts as an autonomous project manager that listens to meeting audio uploads, runs semantic extraction to find action items, and populates team sprint backlogs instantly with zero human effort.

## ⚙️ How It Works (Workflow Logic)
1. **Trigger:** A Google Drive node monitors a shared directory for new `.mp3` or `.m4a` corporate recordings.
2. **Speech-to-Text:** An HTTP Request Node pipes the binary audio data directly into the OpenAI Whisper / Groq API for rapid text transcription.
3. **Semantic Processing:** The Claude AI API node performs deep semantic chunking to match raw audio spoken tasks into corporate operational frameworks.
4. **Data Looping:** An Item Lists node converts Claude's output JSON array into separate data streams.
5. **Project Board Update:** A Notion Node generates individual task cards with assigned names and calculated deadlines.
6. **Targeted Ping:** Individual team members receive automated text reminders detailing their exact assignments via Slack/WhatsApp.

## 🧠 Claude AI System Prompt
```text
Context: You are an Elite Executive Project Manager AI. Analyze the following raw meeting transcript, extract explicit commitments and tasks mentioned, and format them into a structured task array.

Raw Transcript Text:
"{{ $json.transcription_text }}"

Extraction Directives:
1. Identify the specific task description (Action Item).
2. Identify the owner responsible for the task.
3. Extract any mentioned deadlines. If no deadline is stated, default to "3 days from today".
4. Map internal spoken names to official corporate handles (e.g., If transcript says "Fariq", map owner to "Abang Muhammad Fariq").

Output Format (Strict JSON Array):
[
  {
    "task_title": "string",
    "assigned_owner": "string",
    "due_date": "YYYY-MM-DD",
    "priority_level": "High" | "Medium" | "Low"
  }
]
