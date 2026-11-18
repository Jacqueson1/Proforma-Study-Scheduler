
# Proforma Study Scheduler

A personalized study scheduling application that converts academic proforma documents into structured study plans based on academic requirements, available time, and individual preferences.

## Overview

Proforma Study Scheduler is a Python-based application designed to help university students manage their academic workload.

Instead of manually creating study timetables, students can upload their academic proforma PDFs and let the application extract subject information, weekly topics, Student Learning Time (SLT), and assessment requirements.

The application generates a study schedule based on the extracted academic information and the student's personal availability. Students can customize their semester settings, adjust assessment deadlines, manage study sessions, and export their schedules to calendar applications.

An integrated AI chatbot, powered by Ollama and Qwen 2.5:7B, allows students to interact with and modify their schedules using natural language.

The main objective is to provide a flexible study planning experience that adapts to students' academic responsibilities and personal routines without creating unnecessary scheduling pressure.

## Features

### 1. Academic Proforma Extraction
- Upload multiple academic proforma PDFs.
- Extract course codes, course names, and credit hours.
- Identify weekly topics and Student Learning Time (SLT).
- Extract assessment names and their respective weightage.
- Validate extracted information using Pydantic data models.
- Store extracted subject information as structured JSON files.

### 2. Automated Study Scheduling
- Generate study schedules based on academic proforma requirements.
- Allocate study sessions according to weekly SLT.
- Schedule assessment preparation sessions.
- Consider assessment deadlines and preparation periods.
- Distribute study sessions across available days.
- Prioritize tasks according to their deadlines and remaining workload.
- Allow study preparation ahead of academically demanding weeks.
- Identify study tasks that could not fit into the available schedule.

### 3. Personalized Scheduling
- Configure semester start dates and duration.
- Set maximum daily study hours.
- Customize study session and break durations.
- Configure daily availability and class schedules.
- Set preferred free evenings.
- Configure lighter study days.
- Block specific days when studying is not possible.
- Adjust assessment preparation hours and target weeks.

### 4. Schedule Management
- View generated study sessions.
- Filter schedules by semester week.
- Move study sessions to different dates and times.
- Pin sessions to prevent them from moving during replanning.
- Add personal events and schedule around them.
- Remove personal events.
- Replan schedules while preserving pinned sessions.
- Undo previous scheduling changes.

### 5. AI Study Assistant
- Interact with the study schedule using natural language.
- Retrieve scheduled study sessions.
- Request changes to existing study sessions.
- Add personal events.
- Pin or unpin study sessions.
- Block or unblock study days.
- Regenerate study schedules.
- Undo previous changes.

The AI assistant uses tool-based interactions to perform scheduling operations. Changes that modify the schedule require user confirmation before execution.

### 6. Calendar Export
- Export generated study schedules in `.ics` format.
- Import schedules into Google Calendar and other compatible calendar applications.
- Include study session titles, dates, and start and end times.
- Add calendar reminders 10 minutes before scheduled sessions.
- Support the Asia/Kuala_Lumpur timezone.

## Technologies Used

| Category | Technology |
|---|---|
| Programming Language | Python 3.12+ |
| User Interface | Streamlit |
| PDF Processing | pdfplumber |
| Data Validation | Pydantic |
| Scheduling | Custom Greedy Scheduling Algorithm |
| AI Integration | Ollama |
| Language Model | Qwen 2.5:7B |
| Calendar Export | iCalendar (.ics) |
| Data Storage | JSON |
| Dependency Management | uv |

**Additional dependency:** OR-Tools is declared in the project dependencies, although the current scheduling implementation uses a custom greedy algorithm.

## Project Structure

The main repository contains the `study-agent` Git submodule, which holds the application source code.

```text
Proforma-Study-Scheduler/
│
├── README.md
│
└── study-agent/                 # Application submodule
    │
    ├── config.json              # Semester and scheduling preferences
    ├── pins.json                # Pinned sessions and blocked days
    ├── pyproject.toml            # Project dependencies
    ├── uv.lock                   # Dependency lock file
    │
    ├── data/
    │   ├── DES3083.json         # Extracted subject information
    │   └── DES3103.json         # Extracted subject information
    │
    ├── proforma/
    │   ├── Proforma_SE_Process.pdf
    │   └── Proforma_V_V.pdf
    │
    ├── output/
    │   ├── schedule.json        # Generated study schedule
    │   ├── study_schedule.ics  # Calendar export
    │   └── undo.json            # Previous schedule states
    │
    └── src/
        ├── app.py               # Streamlit application
        ├── agent.py             # AI chatbot and tool execution
        ├── extract.py           # PDF extraction and validation
        ├── scheduler.py         # Study scheduling algorithm
        ├── store.py             # Schedule management and persistence
        ├── export_ics.py        # Calendar generation
        │
        └── study_agent/
            └── __init__.py
```

## Getting Started

### Prerequisites

Install the following before running the application:

- Python 3.12 or newer
- [uv](https://docs.astral.sh/uv/)
- [Ollama](https://ollama.com/)
- Git

### 1. Clone the Repository

Clone the repository together with its submodule:

```bash
git clone --recurse-submodules https://github.com/Jacqueson1/Proforma-Study-Scheduler.git

cd Proforma-Study-Scheduler/study-agent
```

If you have already cloned the repository without its submodule:

```bash
git submodule update --init --recursive
```

### 2. Install Dependencies

Install the project dependencies using uv:

```bash
uv sync
```

### 3. Set Up the AI Model

Make sure Ollama is installed and running.

Download the Qwen 2.5:7B model:

```bash
ollama pull qwen2.5:7b
```

The application uses this model for its AI study assistant.

### 4. Run the Application

Start the Streamlit application:

```bash
uv run streamlit run src/app.py
```

Streamlit will provide a local URL, usually:

```text
http://localhost:8501
```

Open the URL in your browser to access the application.

## How It Works

The application follows a series of processing stages to generate and manage personalized study schedules.

### Stage 1: Proforma Upload

Students upload their academic proforma PDFs through the Streamlit interface.

### Stage 2: Information Extraction

The PDF extraction module uses pdfplumber to retrieve relevant academic information, including:

- Course information
- Weekly topics
- Student Learning Time
- Assessment requirements

Pydantic models structure the extracted information before it is saved as JSON.

### Stage 3: Scheduling Configuration

The application reads the student's semester settings, weekly availability, academic commitments, and assessment preferences from the configuration files.

### Stage 4: Schedule Generation

The scheduling engine converts academic workload into individual study blocks.

It considers:
- Weekly SLT requirements.
- Assessment preparation requirements.
- Available study periods.
- Daily study limits.
- Existing pinned sessions.
- Blocked days.
- Task deadlines.

The current implementation uses a greedy scheduling approach that prioritizes tasks according to urgency and deadline constraints.

### Stage 5: Schedule Customization

Students can review their generated schedules and make adjustments through the Streamlit interface.

The schedule management module handles changes, pinned sessions, personal events, replanning, and undo operations.

### Stage 6: AI Assistance

The AI chatbot uses Ollama to interpret natural-language requests and select appropriate scheduling tools.

Schedule modifications are previewed and require student approval before being applied.

### Stage 7: Calendar Export

The generated schedule can be exported into an `.ics` calendar file for use with compatible calendar applications.

## Configuration

The main configuration file is `config.json`.

It contains the following settings:

| Configuration | Description |
|---|---|
| Semester | Start date and semester duration |
| Weekly Schedule | Classes, meals, and other commitments |
| Study Window | Preferred daily study hours |
| Study Duration | Study block and break duration |
| Daily Limit | Maximum study hours per day |
| Free Evening | Preferred evening without study sessions |
| Light Day | Day with reduced study workload |
| Assessments | Assessment deadlines and preparation requirements |
| Timezone | Calendar timezone |

Users can modify these settings through the application's Settings interface.

## AI Assistant

The AI assistant uses a local language model through Ollama.

**Default model:** `qwen2.5:7b`

The assistant interacts with the scheduling system through predefined tools rather than directly modifying scheduling files.

Available operations include:

- `get_schedule`
- `move_block`
- `add_event`
- `delete_event`
- `pin_block`
- `unpin_block`
- `clear_pins`
- `block_day`
- `unblock_day`
- `replan`
- `undo`

The assistant also supports confirmation workflows for schedule modifications.

## Data Storage

The application uses JSON files for local data persistence.

| File | Purpose |
|---|---|
| `config.json` | Semester settings and preferences |
| `data/*.json` | Extracted academic proforma information |
| `output/schedule.json` | Current generated schedule |
| `output/undo.json` | Previous schedule states |
| `pins.json` | Pinned sessions and blocked days |

No external database server is required.

## License

No license file is currently provided in this repository.
```

---

## 3. Important observations

A few things I noticed while inspecting the implementation:

| Area | Observation |
|---|---|
| Scheduling | Currently uses a greedy algorithm, not OR-Tools. |
| AI | Qwen 2.5:7B communicates with scheduling functions through tool calls. |
| Storage | JSON-based persistence, no SQL database. |
| PDF extraction | Uses a specific academic proforma table structure. |
| Calendar | Generates `.ics` files with a 10-minute reminder. |
| Deployment | Streamlit currently runs as a local web application. |

**One repository detail:** `study-agent` is a Git submodule with its own Git repository. The README above is intended for the parent `Proforma-Study-Scheduler` repository, so cloning with `--recurse-submodules` is important.

I have only read the repository. **No files, commits, branches, or repository settings have been modified.**

<MemoryCite />
