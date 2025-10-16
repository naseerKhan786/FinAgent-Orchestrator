# FinAgent-Orchestrator
# 🧠 AA_AGENT_TEAM_SUPERV_01

## Overview
`AA_AGENT_TEAM_SUPERV_01` is a workflow-driven multi-agent system designed to assist users with **Payables Invoices** and **Receivables Transactions** queries.

The workflow uses a **supervisor agent** to understand the user’s intent and delegates tasks to the appropriate **worker agents** for data retrieval and summarization.

---

## 🏗️ Architecture

### Components
| Component | Type | Description |
|------------|------|-------------|
| **AA_AGENT_SUPERV_01** | Supervisor Agent | Determines user intent and delegates to the correct worker agent. |
| **AA_AGENT_AP_INV** | Worker Agent | Retrieves **Payables Invoice** data based on *Supplier Name*. |
| **AA_AR_AGENT_INV** | Worker Agent | Retrieves **Receivables Transaction** data based on *Customer Name*. |

### Supported Tasks
- Show invoices for a given **Supplier**
- Show transactions for a given **Customer**

### Examples
AA_AGENT_TEAM_SUPERV_01/
│
├── config/
│ └── workflow.json # Workflow definition
│
├── src/
│ ├── main.py # Entry point
│ │
│ ├── agents/
│ │ ├── supervisor_agent.py # Delegation logic
│ │ ├── ap_invoice_agent.py # Handles payables invoice data
│ │ └── ar_transaction_agent.py # Handles receivables transaction data
│ │
│ ├── tools/
│ │ ├── ap_invoices_tool.py # REST integration for Payables
│ │ └── ar_trx_tool.py # REST integration for Receivables
│ │
│ └── utils/
│ └── http_client.py # Shared HTTP functions
│
└── tests/
└── test_workflow.py # Validation tests


---

## 🔁 Workflow Logic

1. **User Request** → Supervisor Agent (`AA_AGENT_SUPERV_01`)
2. **Intent Detection**  
   - Mentions *Supplier* → delegate to `AA_AGENT_AP_INV`
   - Mentions *Customer* → delegate to `AA_AR_AGENT_INV`
3. **Worker Agent Execution**
   - Fetches data via REST API:
     - `/fscmRestApi/resources/11.13.18.05/invoices?q=Supplier='{SupplierName}'`
     - `/fscmRestApi/resources/11.13.18.05/receivablesInvoices?finder=invoiceSearch;BillToCustomerName="{CustomerName}"`
4. **Summarization & Response**
   - Generates a friendly, professional answer to the user.

---

## 🧩 Key Files

| File | Purpose |
|------|----------|
| `workflow.json` | Defines agents, mappings, and triggers |
| `supervisor_agent.py` | Core decision-making logic |
| `ap_invoice_agent.py` | Payables data retrieval |
| `ar_transaction_agent.py` | Receivables data retrieval |
| `http_client.py` | Common REST helper functions |

---

## 🚀 Running Locally

### 1. Clone Repository
```bash
git clone https://github.com/<your-org>/AA_AGENT_TEAM_SUPERV_01.git
cd AA_AGENT_TEAM_SUPERV_01

2. Install Dependencies
pip install -r requirements.txt

3. Run the App
python src/main.py


(Optional) If using FastAPI:

uvicorn src.main:app --reload

🧪 Testing
pytest tests/

⚠️ Error Handling

Workflow includes an EMAIL-based error handler.

Configure recipients in workflow.json → errorHandlers.

📚 Notes

The Supervisor only supports Supplier and Customer queries.

“Transaction” and “Invoice” are treated as synonyms.

Any other type of query will result in a polite explanatory message.

🧰 Tech Stack

Python 3.10+

REST API Integration

JSON Workflow Configuration

Modular Agent Architecture
