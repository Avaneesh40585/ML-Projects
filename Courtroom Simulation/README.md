# Courtroom Simulation v2.5
It is a multi-agent courtroom simulation powered by Large Language Models (LLMs) via the Groq API. The system simulates realistic trials with autonomous agents—Judge, Defense Lawyer, Defendant, Prosecution Lawyer, Plaintiff (for civil cases), and more—using real-world case data. Each run randomly selects a case from your dataset and proceeds through all major trial phases, with dynamic agent management and a flexible, evolving trial structure.

## Features:
	•	Groq LLM Integration: Fast, scalable, state-of-the-art language model API.
	•	All Minimum Required Agents: Judge, Defense Lawyer, Defendant, Prosecution Lawyer, Plaintiff (for civil cases), and more.
	•	Dynamic Agent Management: Add/remove witnesses, experts, jurors, and other roles at any phase—automatically, based on trial context.
	•	Flexible, Evolving Trial Structure: Agents can request phase changes (e.g., recalling witnesses, moving to verdict) for a realistic, non-linear trial.
	•	Batch Case Support: Randomly selects a case from `data.csv` for each simulation run.
	•	Realistic, Role-Specific Prompts: Each agent acts with serious, context-aware legal reasoning.
	•	Extensible: Easily add new roles, phases, or modify trial flow.


## Project Structure:
<pre>
```
.
├── Iterations/                       # Folder containing different versions of notebooks (.ipynb) of the Simulator  
│   ├── v0.0/                         # Explanation about each version’s speciality is given at the start of it  
│   ├── v1.0/  
│   ├── v2.0/  
│   ├── v2.5/  
│   └── v3.0/  
├── data.csv                          # CSV file containing case descriptions  
├── README.md                         # This file  
```
</pre>




## Setup:
1. Install Dependencies
pip install groq pandas

2. Prepare Your Data
	•	Place a `data.csv` file in the project directory.
	•	The first row should be a header (e.g., `case`).
	•	Each subsequent row should be a case description (text).
3. Set Your Groq API Key
	•	Option 1 (for quick testing):
Edit the script and replace the `api_key` string with your Groq API key.
	•	Option 2 (recommended for production):
Set your API key as an environment variable:
export GROQ_API_KEY="your-groq-api-key"

## Usage:
	•	On each run, a random case from `data.csv` is selected.
	•	The simulation proceeds through all trial phases, with each agent responding in character.
	•	The trial structure can evolve dynamically based on agent (especially Judge) instructions.

## Architecture:
### Core Components:
	•	CourtroomAgent:
Represents a legal persona (Judge, Defense, etc.). Maintains its own prompt and conversation history. Handles all LLM interactions for that role.
	•	Courtroom:
Orchestrates the trial, manages agents, runs phases, and handles dynamic agent creation/removal.
	•	Flexible Trial Controller:
A loop that determines the next phase based on agent responses, allowing for realistic, non-linear trial flow.

### Agents:
	•	Judge: Presides, rules, and delivers verdicts.
	•	Defense Lawyer: Advocates for the defendant.
	•	Defendant: The accused party.
	•	Prosecution Lawyer: Argues for the state (criminal) or against the defendant (civil).
	•	Plaintiff: The party seeking relief (civil cases only).
	•	Bailiff: Maintains order.
    •	Stenographer: Summarizes proceedings.
	•	ExpertConsultant: Provides technical/legal expertise.
	•	Jurors: Deliver individual verdicts.
	•	Witnesses/Experts: Dynamically created as needed, based on trial context.

### Trial Phases:
	1.	Opening Statements: Each side presents their case overview.
	2.	Witness Interrogation & Argumentation: Examination and cross-examination of witnesses, expert testimony, and argumentation.
	3.	Closing Statements: Final summaries by all sides.
	4.	Judge’s Ruling: Judge delivers the verdict.
	5.	Jury Deliberation (Optional): Each juror states their verdict.

### Dynamic Agent and Phase Management:
	•	Automatic Agent Creation:
If an agent (e.g., Judge or lawyer) requests a new witness or expert in their response, the system parses the response and creates the agent automatically for the next phase.
	•	Flexible Phase Progression:
After each phase, agent responses are analyzed for cues (e.g., “recall witness”, “move to verdict”). The controller loop then jumps to the requested phase, repeats phases, or proceeds as appropriate.


### Example Output:
==== Phase 1: Opening Statements ====

PROSECUTION RESPONSE:
[Prosecution's formal opening statement...]

DEFENSE RESPONSE:
[Defense's formal opening statement...]

DEFENDANT RESPONSE:
[Defendant's introduction...]

PLAINTIFF RESPONSE:
[If civil case, plaintiff's opening statement...]

------------------------------------------------------------

==== Phase 2: Witness Interrogation & Argumentation ====

[...]

### Customization:
	•	Add new roles: Use `create_agent()` to add any role at any time.
	•	Change trial flow: The flexible controller loop allows for custom branching, looping, or skipping of phases.
	•	Modify agent prompts: Change system prompts to experiment with different agent behaviors.
Troubleshooting
	•	Dictionary output after responses?
Don’t print the return value of `run_phase`. Just call it directly.
	•	Not enough responses?
Make sure your prompts dictionary includes all present agents for each phase.
	•	API errors?
Ensure your Groq API key is correct and you have internet connectivity.
