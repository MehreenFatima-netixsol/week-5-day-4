# CrewAI Multi-Agent Collaboration — Sequential vs Hierarchical vs Single-Agent

This project demonstrates how a team of specialized AI agents can collaborate on a shared business objective using **CrewAI**. The task simulates a real-world marketing workflow for a **“Budget Laptops for Students”** campaign, where different agents analyze internal products, research competitor pricing, and produce a stakeholder-ready marketing brief.

The implementation includes both **sequential** and **hierarchical** CrewAI workflows and compares them against a **single-agent LangGraph** solution from a previous assignment.

---

## Project Objective

The goal was to evaluate whether a **multi-agent crew** provides better organization, reliability, and output quality than a single generalist agent for a multi-step business task.

The workflow answers a practical business question:

> Which laptop should be recommended for a student-focused marketing campaign based on internal pricing, stock availability, and competitor pricing?

---

## Agent Architecture

### 1. Data Analyst
**Responsibility:** Analyze the internal product catalog and identify the two strongest laptop candidates.

- Uses product lookup tools
- Computes the exact price gap between shortlisted models
- Produces a structured output for downstream agents

### 2. Market Researcher
**Responsibility:** Compare competitor pricing for the shortlisted laptops.

- Uses competitor pricing tools
- Determines which model has the stronger market advantage
- Provides sourced retailer comparisons

### 3. Marketing Strategist
**Responsibility:** Write the final stakeholder-ready marketing brief.

- Uses only verified upstream data
- Recommends exactly one model
- Produces a concise executive summary under 200 words

---

## Workflow Implementations

### Sequential CrewAI

```text
Data Analyst
      ↓
Market Researcher
      ↓
Marketing Strategist
```

The workflow is predefined, and each task receives the output of the previous task through `context=[...]`. This approach is simple, predictable, and easy to debug.

### Hierarchical CrewAI

```text
Campaign Manager
        ↓
Data Analyst
        ↓
Market Researcher
        ↓
Marketing Strategist
        ↓
Campaign Manager Review
```

The Campaign Manager dynamically delegates work, reviews intermediate outputs, and assembles the final recommendation. This approach is more flexible and scalable for complex projects.

---

## Comparison: Sequential vs Hierarchical vs Single-Agent

| Aspect | Single-Agent | Sequential CrewAI | Hierarchical CrewAI |
|--------|---------------|-------------------|---------------------|
| Token Usage | Lowest | Moderate | Highest |
| Approx. Cost | Lowest | Moderate | Highest |
| Latency | Fastest | Moderate | Slowest |
| Role Separation | None | High | Very High |
| Coordination | None | Context Passing | Manager Delegation |
| Best Use Case | Simple tasks | Fixed multi-step workflows | Complex dynamic workflows |

### Key Takeaways

- **Single-Agent (LangGraph):** Cheapest and fastest, but less transparent.
- **Sequential CrewAI:** Best balance of quality, traceability, and cost.
- **Hierarchical CrewAI:** Most flexible, but introduces additional coordination overhead.

---

## Evaluation Criteria

The final marketing brief was evaluated using three criteria:

### 1. Factual Grounding
All prices, stock values, and competitor numbers must come directly from tool outputs.

### 2. Completeness
The brief must include:

- One recommended laptop
- Internal pricing and stock information
- Competitor pricing with retailer names
- A clear recommendation rationale

### 3. Tone & Audience Fit
The brief must remain under 200 words, use professional business language, and be understandable to non-technical stakeholders.

---

## Installation

```bash
pip install crewai litellm python-dotenv
```

---

## Running the Sequential Crew

```python
sequential_result = await sequential_crew.kickoff_async()
print(sequential_result)
```

## Running the Hierarchical Crew

```python
hierarchical_result = await hierarchical_crew.kickoff_async()
print(hierarchical_result)
```

---

## Conclusion

For this assignment, the **Sequential CrewAI workflow** proved to be the most effective approach. The business problem naturally decomposed into three independent stages—product analysis, competitor research, and marketing strategy—which aligned perfectly with specialized agents. Compared with the single-agent LangGraph solution, the sequential crew improved traceability and factual consistency while keeping implementation complexity manageable. The hierarchical workflow demonstrated advanced delegation capabilities, but its additional planning overhead did not provide significant quality improvements for this relatively simple, predefined pipeline.

---

## Technologies Used

- **CrewAI** — Multi-agent orchestration
- **LiteLLM** — Unified LLM interface
- **Python** — Workflow implementation
- **Jupyter Notebook** — Experimentation and execution
- **Custom pricing tools** — Internal and competitor product lookups

---

## Author

**Mehreen Fatima**  
