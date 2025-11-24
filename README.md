# 🚀 Multi-Agent Account Plan System

A modular, multi-agent architecture using **Gemini + ADK** designed for structured content generation through a **Root-Writer-Critic** chain of reasoning.

---

## 📌 **Overview**

This project implements a multi-agent workflow that:

1. **Root-Agent**

   * Understands the user query.
   * Routes the request to the correct subsystem.
   * Prevents hallucinated content.
   * Responsible for removing irrelevant(outside domain) user queries.
   * Detects requests to *create/update/delete* account-plan content and forwards to the Writer.

2. **Writer-Agent**

   * Generates or edits **only the required sections**.
   * Uses the Critic feedback before returning the final content.
   * Avoids modifying unrelated sections.

3. **Critic-Agent**

   * Evaluates the writer draft.
   * Returns **top 3 issues** + **top 3 improvements**.
   * No rewriting; only guidance.

This creates a **single-iteration writer-critic loop** for high-quality content generation.

---

# 🧠 **Architecture Diagram**

```
User → Root-Agent
          │
          ├─► Handles normal questions (information/guidance/irrelevant queries)
          │
          └─► Writer-Agent (when user wants to create/edit content)
                      │
                      ▼
                 Critic-Agent
                      │
                      ▼
               Writer applies feedback
                      │
                      ▼
                   Final Output
```

---

# 🛠 **Setup Instructions**

### **1. Install dependencies**

```bash
pip install google-generativeai google-adk python-dotenv
```

### **2. Set environment variables**

Create `.env`:

```
GOOGLE_API_KEY=your-key-here
```

### **3. Run the script**

Running inside a Jupyter notebook

Make sure you imported the runner and initialized the agents, then:

```python
await run_root(["Create a company overview for Google.", session_name="test-session"])
```

---

# ⚙️ **Design Decisions**

### ✔ **Stateless by Choice**

* No persistent section storage.
* Instead, the *root* defers all structural editing to the writer.
* This keeps behavior predictable, avoids ghost-state, and reduces complexity.

### ✔ **Writer Controls Section Editing**

The root **never edits content**, it only routes.


### ✔ **JSON-Only Protocol Between Agents**

This ensures:

* No prompt leakage
* No accidental content generation
* Reliable routing logic

### ✔ **Strong Guardrails**

Each agent has strict rules to prevent:

* Hallucination
* Unauthorized editing
* Multi-agent runaway loops

---

# 🔍 **Key Features**

* Multi-agent composition
* Writer-Critic refinement
* Section-targeted updates
* Automatic routing logic
* JSON-strict inter-agent communication
