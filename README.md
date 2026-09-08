# Telegram Voice Meeting Assistant

An AI-powered meeting assistant built with **n8n** that receives Telegram voice messages, transcribes them using Groq Whisper, analyzes the conversation with an AI Agent, and returns a structured meeting summary with decisions and actionable tasks.

## 🎯 Project Overview

Meetings often contain valuable information that is difficult to organize after the conversation ends.

This project automates the process of turning a **Telegram voice message into structured meeting intelligence**.

Instead of manually listening to a recording and writing meeting notes, the workflow:

1. Receives the voice message through Telegram.
2. Downloads the audio file.
3. Processes the audio format for API compatibility.
4. Transcribes the audio into text.
5. Analyzes the transcript using an AI Agent.
6. Extracts key discussion points, decisions, and action items.
7. Returns the structured result to Telegram.

## 🏗️ Architecture

**Telegram → Audio Processing → Groq Whisper → AI Agent → Structured Output → Telegram**

### Workflow

```text
Telegram Trigger
      ↓
Telegram Get File
      ↓
JavaScript Code
(Audio Format Handling)
      ↓
HTTP Request
(Groq Whisper)
      ↓
AI Agent
(LangChain + Groq)
      ↓
Structured Output Parser
      ↓
Telegram Output
```

## 🔧 Components

### 1. Telegram Trigger

Listens for incoming voice messages from a Telegram chat.

### 2. Telegram Get File

Downloads the raw Telegram audio file, which is commonly received in `.oga` format.

### 3. JavaScript Code Node

Processes the binary file parameters and adjusts the file metadata/extension to make it compatible with the transcription API.

This step was added to handle audio format compatibility issues encountered during development.

### 4. Groq Whisper API

The workflow sends the processed audio to the Groq API and uses:

**`whisper-large-v3`**

to convert speech into text.

### 5. AI Agent

The transcribed text is passed to an AI Agent built with the n8n LangChain node.

The Agent analyzes the meeting content and extracts:

* Key discussion points
* Decisions
* Action items
* Assignees
* Deadlines
* Meeting summary

### 6. Structured Output Parser

Ensures that the AI response follows a predefined JSON structure instead of returning unpredictable free-form text.

### 7. Telegram Output

The structured result is transformed into a clean, human-readable Telegram message and sent back to the user.

## 🧩 Key Challenges Solved

### Telegram `.oga` compatibility

Telegram voice messages may arrive using the `.oga` format, while the transcription API may expect a different supported audio format.

The workflow includes a custom JavaScript processing step to handle the binary metadata and file extension.

### n8n Binary Data Handling

During development, differences in binary property names such as `data` and `data0` caused processing issues.

The workflow was adjusted to correctly handle the binary data passed between nodes.

### Structured Telegram Output

Raw JavaScript objects can appear as:

```text
[object Object]
```

when they are not formatted correctly.

The workflow converts the structured AI response into readable Telegram Markdown instead of sending the raw object.

## 📋 Example Output

A processed meeting can be returned in a structure similar to:

```text
📌 Meeting Summary

📝 Summary:
Discussion about the next project phase and delivery priorities.

🎯 Decisions:
• Finalize the project scope
• Prepare the implementation plan

✅ Action Items:
• Prepare project plan — Assigned to: Ali
• Review requirements — Assigned to: Sara

⏰ Deadlines:
• Project plan — Next Monday
```

## 🛠️ Technologies

* **n8n**
* **Telegram Bot API**
* **Groq Cloud API**
* **Whisper Large V3**
* **LangChain / n8n AI Agent**
* **JavaScript**
* **Structured Output Parser**
* **Docker**

## 📌 Current Status

**Version:** 1.0

* ✅ Telegram voice message input
* ✅ Telegram audio file retrieval
* ✅ Audio binary processing
* ✅ Groq Whisper transcription
* ✅ AI-powered meeting analysis
* ✅ Structured JSON output
* ✅ Human-readable Telegram response
* ✅ Tested locally

### Planned Improvements

* ⏳ Persistent meeting history
* ⏳ Database integration
* ⏳ Speaker identification
* ⏳ Automatic task management
* ⏳ Calendar integration
* ⏳ Production deployment

## 🚀 How to Use

1. Download `telegram-voice-meeting-assistant.json` from this repository.
2. Import the workflow into your n8n instance.
3. Configure your own Telegram Bot credentials.
4. Configure your own Groq API credentials.
5. Activate the workflow.
6. Send a voice message to the connected Telegram bot.
7. The workflow transcribes and analyzes the message.
8. The structured result is returned to Telegram.

### Important

**Never commit API keys, Telegram bot tokens, passwords, or other private credentials to a public repository.**

Configure credentials inside your own n8n environment.

## 🌍 Localization Note

The current workflow can be adapted to different languages by changing the AI Agent instructions and output formatting.

The core architecture—voice ingestion, transcription, structured AI analysis, and messaging—is language-agnostic.

## 💡 Lessons Learned

* **Binary data handling matters.** Integrations involving files can fail when binary property names or metadata are not handled consistently between nodes.
* **API compatibility is part of automation engineering.** A workflow may be logically correct but still fail because an external API expects a specific file format or metadata.
* **Structured output improves reliability.** Using a defined JSON schema makes AI-generated results easier to process and present.
* **Formatting is part of the user experience.** Returning structured data is not enough; the final response should be formatted for the platform where the user receives it.
* **Debugging requires testing the complete pipeline.** Individual nodes may work correctly while the complete workflow still fails because of data-format mismatches between steps.

## 📄 Version History

### v1.0

Initial version of the Telegram Voice Meeting Assistant.

The workflow receives Telegram voice messages, transcribes them with Groq Whisper, analyzes the content with an AI Agent, and returns structured meeting intelligence to Telegram.
