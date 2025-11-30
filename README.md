# Multi-Agent Customer Service System (A2A + MCP)

This repository contains a full implementation of a multi-agent customer service system that integrates:

- Router Agent  
- Customer Data Agent  
- Support Agent  
- MCP-style tool interface  
- SQLite-backed customer & ticket database  
- End-to-end A2A orchestration tests  

All components are implemented and executed inside the notebook:

    A5 (1).ipynb

This notebook includes:
- MCP server initialization  
- Database creation and seeding  
- Definitions of all three agents  
- A2A-compatible agent servers  
- An integrated test suite demonstrating multi-step coordination  

This satisfies the assignment’s requirements for MCP integration, A2A communication, and multi-agent workflow design.

------------------------------------------------------------
1. How the System Works
------------------------------------------------------------

1) **MCP Server (inside notebook)**  
Exposes JSON-RPC tool endpoints for:
- get_customer  
- list_customers  
- update_customer  
- create_ticket  
- get_customer_history  

These operate on `customers` and `tickets` tables stored in a local SQLite database.

2) **Customer Data Agent**  
Handles all structured data access through MCP tools.  
Extracts customer IDs, email updates, history requests, and validation rules.

3) **Support Agent**  
Handles billing issues, refunds, account upgrades, ticket creation, and ticket queries.  
Determines priority and escalation patterns.

4) **Router Agent**  
Receives natural-language queries, determines intent domains, and orchestrates calls to the two specialist agents.  
Produces final structured JSON output combining downstream agent results.

------------------------------------------------------------
2. Running the System
------------------------------------------------------------

Install dependencies:
    pip install -r requirements.txt

Launch the notebook:
    A5 (1).ipynb

The notebook will:
- Initialize the SQLite database
- Start MCP server logic
- Start all three A2A agent servers
- Run the full integration test suite automatically

No additional files or folders are required to run the complete system.

------------------------------------------------------------
3. End-to-End Demonstration
------------------------------------------------------------

The notebook executes at least five scenarios:

• Simple customer lookup  
• Coordinated upgrade handling  
• Combined data + support query  
• Refund escalation  
• Multi-intent (record update + ticket history)

For each scenario, the notebook prints:
- Router routing decisions  
- A2A messages exchanged  
- MCP tool calls  
- Final structured JSON response  

This provides direct evidence of correct A2A+MCP integration.

------------------------------------------------------------
4. Repository Contents
------------------------------------------------------------

.gitignore  
requirements.txt  
A5 (1).ipynb    ← full implementation (MCP + agents + tests)  
A5 (1).html     ← exported version of the notebook  

All executable logic lives inside the notebook to simplify submission, reproducibility, and evaluation.

------------------------------------------------------------
5. Troubleshooting
------------------------------------------------------------

• If MCP calls fail → restart the notebook kernel  
• If agent ports are busy → rerun the initialization cells  
• If database becomes inconsistent → rerun the DB setup cell  

------------------------------------------------------------
6. License
------------------------------------------------------------

This project is for academic course use and interview preparation.
