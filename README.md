# Assignment 5 – Multi-Agent Customer Service System (A2A + MCP)

This repository contains an end-to-end implementation of a multi-agent customer service system using Google’s Agent-to-Agent (A2A) protocol together with a Model Context Protocol (MCP) data access layer.  
All components — MCP tools, three A2A agents, orchestration logic, and the full test suite — are implemented inside the notebook:

    A5.ipynb

This design keeps the project self-contained and easy to run while still demonstrating the complete architecture required by the assignment.

------------------------------------------------------------
1. Project Overview
------------------------------------------------------------

The system models a realistic customer-service workflow where multiple specialized agents collaborate:

• **Router Agent**  
  Interprets incoming user requests, identifies intent domains, and decides which downstream agents should be invoked.

• **Customer Data Agent**  
  Connects exclusively to the MCP server to retrieve or modify customer records. Handles structured data queries such as profile lookup, customer lists, ticket history, and field updates.

• **Support Agent**  
  Focuses on support-related tasks, including billing issues, refund detection, upgrade requests, ticket creation, and ticket queries.

• **MCP Server (SQLite-backed)**  
  Provides standardized JSON-RPC–style tools for database operations:
    - get_customer  
    - list_customers  
    - update_customer  
    - create_ticket  
    - get_customer_history  

The three A2A agents run as independent HTTP services inside the notebook and communicate through JSON-RPC A2A messaging.

------------------------------------------------------------
2. System Architecture
------------------------------------------------------------

The notebook implements the following components:

1) **Database Layer**  
   A lightweight SQLite database is created on startup.  
   Tables include:
   - customers(id, name, email, phone, status, created_at, updated_at)  
   - tickets(id, customer_id, issue, status, priority, created_at)

2) **MCP Tool Interface**  
   The notebook defines a minimal MCP-like server exposing 5 required tools.  
   All customer and ticket operations performed by agents must use these tools; no agent accesses the database directly.

3) **Three A2A Agents (Router / Data / Support)**  
   Each agent runs on its own port as a Starlette + A2aAgentExecutor service.  
   Every agent produces structured JSON outputs only.

4) **Orchestration Logic**  
   The Router Agent analyzes input text and handles:
   - intent detection  
   - routing rules  
   - combined flows (customer context + support action)  
   - sequential and parallel coordination  
   - aggregation of JSON responses from multiple agents

5) **Integrated Test Suite**  
   The notebook runs several scenarios to verify:
   - task assignment  
   - escalation and negotiation  
   - multi-step reasoning  
   - complex cross-agent workflows

------------------------------------------------------------
3. Installation & Environment Setup
------------------------------------------------------------

Clone this repository:

    git clone <repo-url>
    cd <repo-folder>

Create and activate a Python virtual environment:

    python -m venv venv
    source venv/bin/activate         # macOS/Linux
    venv\Scripts\activate            # Windows

Install required dependencies:

    pip install -r requirements.txt

For API-based models (Gemini), set your environment key:

    export GOOGLE_API_KEY="<your-key>"
    # or use Colab's userdata storage

No additional Python files or folders are required — the notebook contains the entire implementation.

------------------------------------------------------------
4. Running the System
------------------------------------------------------------

Open:

    A5.ipynb

Then execute all cells from top to bottom.

The notebook will automatically:

• initialize the SQLite database  
• start the MCP tool server  
• launch all three A2A agent servers (on ports 9300, 9301, 9400)  
• print live logs from each agent  
• run all end-to-end test scenarios  

All services execute inside background threads to allow the notebook to issue A2A requests during testing.

------------------------------------------------------------
5. Test Scenarios Implemented
------------------------------------------------------------

The notebook demonstrates multiple coordination patterns required in the assignment:

### 1) Simple Query  
“Get customer information for ID 5”  
→ Routed to Customer Data Agent → MCP lookup

### 2) Coordinated Request  
“I'm customer 12345 and need help upgrading my account”  
→ Customer Data Agent (context) + Support Agent (upgrade handling)

### 3) Multi-Agent Negotiation  
“Show me all active customers who have open tickets”  
→ Data Agent retrieves active customers → Support Agent filters ticket state

### 4) Escalation / Urgency Detection  
“I've been charged twice, please refund immediately!”  
→ Support Agent detects billing + high priority

### 5) Multi-Intent Command  
“Update my email to X and show my ticket history”  
→ Router splits request → update via Data Agent + history retrieval

All results are returned as structured JSON through the Router Agent.

------------------------------------------------------------
6. Repository Contents
------------------------------------------------------------

.gitignore               # Python and notebook ignore rules  
requirements.txt         # Dependencies for MCP + A2A + SQLite  
A5.ipynb                 # Full implementation (agents, MCP, tests)  
A5.html                  # Static HTML export of the notebook (optional)  

The entire project is intentionally consolidated in a single notebook to simplify evaluation and reduce environment complexity.

------------------------------------------------------------
7. Troubleshooting
------------------------------------------------------------

**MCP server unreachable**  
- Restart the notebook kernel  
- Ensure port 8000 is free  

**Agents not responding**  
- Verify the agent startup logs  
- Confirm ports 9300/9301/9400 are not blocked  

**Database issues**  
- Delete the generated `mcp.db` file and rerun setup cells  

**Slow execution**  
- A2A calls involve multiple HTTP round-trips; allow 1–3 seconds per test  
- Re-run the test suite if ports were not fully initialized

------------------------------------------------------------
8. Assignment Requirements Checklist
------------------------------------------------------------

✔ Three-agent architecture implemented (Router / Customer Data / Support)  
✔ MCP integration with all five required tools  
✔ A2A communication using JSON-RPC  
✔ Task allocation scenario  
✔ Negotiation / escalation scenario  
✔ Multi-step coordination scenario  
✔ SQLite database with correct schema  
✔ End-to-end demonstration notebook  
✔ Structured JSON outputs only  
✔ Logging of agent-to-agent interactions  
✔ Conclusion and documentation included  

------------------------------------------------------------
9. Conclusion
------------------------------------------------------------

This project demonstrates how multi-agent systems can coordinate through standardized protocols such as A2A and MCP.  
Building the Router, Customer Data Agent, and Support Agent required explicit intent detection, structured message passing, and carefully ordered tool calls. The combined workflow shows how independent services can collaborate to complete complex tasks reliably.

The implementation also highlights practical engineering challenges — such as port management, tool error handling, and multi-step reasoning — and illustrates how to design modular agents that exchange information through well-defined interfaces.

------------------------------------------------------------
10. License
------------------------------------------------------------

This repository is for academic use as part of the Applied Generative AI Agents and Multimodal Intelligence course.
