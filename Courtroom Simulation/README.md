# Courtroom Simulator

A multi-agent courtroom simulation powered by Large Language Models (LLMs) via the Groq API. The system simulates realistic trials with autonomous agents-Judge, Defense Lawyer, Defendant, Prosecution Lawyer, Plaintiff (for civil cases), and more-using real-world case data. Each run randomly selects a case from your dataset and proceeds through all major trial phases, with dynamic agent management and a flexible, evolving trial structure.

![Courtroom-ghibli-style](https://github.com/user-attachments/assets/a2fbb86a-10f6-40c9-93f0-bd773664e784)

---

## Features

- **Groq LLM Integration:** Fast, scalable, state-of-the-art language model API. Model: `llama-3.3-70b-versatile`.
- **All Minimum Required Agents:** Judge, Defense Lawyer, Defendant, Prosecution Lawyer, Plaintiff (for civil cases), and more.
- **Dynamic Agent Management:** Add/remove witnesses, experts, jurors, and other roles at any phase-automatically, based on trial context.
- **Flexible, Evolving Trial Structure:** Agents can request phase changes (e.g., recalling witnesses, moving to verdict) for a realistic, non-linear trial.
- **Batch Case Support:** Randomly selects a case from `data.csv` for each simulation run.
- **Realistic, Role-Specific Prompts:** Each agent acts with serious, context-aware legal reasoning.
- **Extensible:** Easily add new roles, phases, or modify trial flow.
- **Shared History for Agents:**  
  All agents operate with a shared conversation history, ensuring every response is informed by the full context of the trial. This enables the Judge and others to make decisions with complete awareness of prior arguments, testimonies, and rulings-mirroring real courtroom dynamics.

---

## Setup

1. **Install Dependencies:**
pip install groq pandas

2. **Prepare Your Data:**
- Place a `data.csv` file in the project directory.
- The first row should be a header (e.g., `case`).
- Each subsequent row should be a case description (text).

3. **Set Your Groq API Key:**
- **Option 1 (for quick testing):**  
  Edit the script and replace the `api_key` string with your Groq API key.
- **Option 2 (recommended for production):**  
  Set your API key as an environment variable:  
  ```
  export GROQ_API_KEY="your-groq-api-key"
  ```

---

## Usage

- On each run, a random case from `data.csv` is selected.
- The simulation proceeds through all trial phases, with each agent responding in character.
- The trial structure can evolve dynamically based on agent (especially Judge) instructions.
- **Shared History:** Every agent’s response is based on the entire conversation so far, leading to more informed, realistic, and consistent trial proceedings.

---

## Architecture

### Core Components

- **CourtroomAgent:**  
Represents a legal persona (Judge, Defense, etc.). Maintains its own prompt and interacts with the shared conversation history. Handles all LLM interactions for that role.
- **Courtroom:**  
Orchestrates the trial, manages agents, runs phases, and handles dynamic agent creation/removal.
- **Flexible Trial Controller:**  
A loop that determines the next phase based on agent responses, enabling realistic, non-linear trial flow.

### Shared History

Agents communicate via a **shared message list** (the "scratchpad" or memory pool), which contains all prior exchanges in the trial. This approach enables:
- Richer, more context-aware legal reasoning by all agents.
- The Judge’s rulings and agent arguments to be grounded in the evolving narrative of the case.
- Easy extensibility for future features like memory pruning or retrieval-augmented context windows.

---

### Agents

- **Judge:** Presides, rules, and delivers verdicts.
- **Defense Lawyer:** Advocates for the defendant.
- **Defendant:** The accused party.
- **Prosecution Lawyer:** Argues for the state (criminal) or against the defendant (civil).
- **Plaintiff:** The party seeking relief (civil cases only).
- **Bailiff:** Maintains order.
- **Stenographer:** Summarizes proceedings.
- **ExpertConsultant:** Provides technical/legal expertise.
- **Jurors:** Deliver individual verdicts.
- **Witnesses/Experts:** Dynamically created as needed, based on trial context.

---

### Trial Phases

1. **Opening Statements:** Each side presents their case overview.
2. **Witness Interrogation & Argumentation:** Examination and cross-examination of witnesses, expert testimony, and argumentation.
3. **Closing Statements:** Final summaries by all sides.
4. **Judge’s Ruling:** Judge delivers the verdict.
5. **Jury Deliberation (Optional):** Each juror states their verdict.

---

### Dynamic Agent and Phase Management

- **Automatic Agent Creation:**  
If an agent (e.g., Judge or lawyer) requests a new witness or expert in their response, the system parses the response and creates the agent automatically for the next phase.
- **Flexible Phase Progression:**  
After each phase, agent responses are analyzed for cues (e.g., “recall witness”, “move to verdict”). The controller loop then jumps to the requested phase, repeats phases, or proceeds as appropriate.

---

## Example Output

==== Phase 1: Opening Statements ====

PROSECUTION RESPONSE:
Prosecution’s formal opening statement...

DEFENSE RESPONSE:
Defense’s formal opening statement...

DEFENDANT RESPONSE:
Defendant’s introduction...

PLAINTIFF RESPONSE:
If civil case, plaintiff’s opening statement...

------------------------------------------------------------

==== Phase 2: Witness Interrogation & Argumentation ====

...


---

## Customization

- **Add new roles:** Use `create_agent()` to add any role at any time.
- **Change trial flow:** The flexible controller loop allows for custom branching, looping, or skipping of phases.
- **Modify agent prompts:** Change system prompts to experiment with different agent behaviors.

---

## References

- [AgentCourt: Simulating Court with Adversarial Evolvable Lawyer Agents](https://arxiv.org/html/2408.08089v1)
- [LangGraph Multi-Agent Concepts](https://langchain-ai.github.io/langgraph/concepts/multi_agent/)
- [Memory Sharing for LLM-based Agents](https://arxiv.org/html/2404.09982v1)
- [Groq Llama-3.3-70b-versatile Documentation](https://console.groq.com/docs/model/llama-3.3-70b-versatile)

---
