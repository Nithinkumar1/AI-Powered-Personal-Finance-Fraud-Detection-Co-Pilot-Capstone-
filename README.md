# AI-Powered Personal Finance Copilot

A local AI banking assistant and personal finance copilot built with **FastAPI**, **React**, **SQLite**, **LangGraph**, **MCP tools**, and **Ollama/Llama 3.2**. The application provides a chat-based banking interface where users can ask about balances, transfer funds, create accounts, and query banking policies in a simulated secure banking environment.

The project report also describes the broader finance-copilot goal: fraud detection, anomaly detection, and conversational financial insights using supervised ML, unsupervised anomaly detection, and an intent-based NLP layer.

---

## Features

- AI-powered banking chat assistant
- React frontend with a clean chat UI
- FastAPI backend API for chat requests
- LangGraph ReAct agent with conversation memory
- Ollama-powered local LLM using `llama3.2`
- MCP-based banking tool server
- SQLite database for local account and transaction storage
- Secure PIN/password validation using SHA-256 hashing
- Balance lookup
- Account creation
- Internal funds transfer
- Basic fraud rule that blocks transfers above `$2,000`
- Banking policy query support
- Local-first architecture for privacy

---

## Project Architecture

```text
AI Finance/
├── agent_api.py                  # FastAPI API used by the React frontend
├── run_agent.py                  # Terminal-based AI banking agent
├── core_bank_server/
│   └── app/
│       ├── main.py               # SQLite database setup and health endpoint
│       └── mcp_server.py         # MCP tools for banking operations
├── data/
│   ├── demo_users.csv            # Demo users/accounts
│   ├── demo_transactions.csv     # Demo transaction data
│   ├── bank_policies/            # Banking policy text files
│   └── scripts/
│       ├── init_db.py            # Initializes SQLite database from CSV files
│       ├── clean_transactions.py # Data cleaning/init helper
│       └── build_vector_db.py    # Builds ChromaDB vector store for policy docs
├── local_db/
│   └── chroma_db_storage/        # Local vector database storage
└── bank-frontend/
    ├── package.json              # React dependencies and scripts
    └── src/
        ├── App.js                # Chat UI and backend API call
        └── App.css               # Frontend styling
```

---

## Tech Stack

### Backend

- Python
- FastAPI
- SQLite
- LangGraph
- LangChain
- MCP / FastMCP
- Ollama
- ChromaDB
- Pandas

### Frontend

- React
- JavaScript
- CSS
- Create React App

### AI / NLP

- Llama 3.2 through Ollama
- LangGraph ReAct agent
- Tool-based AI workflow
- Conversational memory using `MemorySaver`

---

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/your-repository-name.git
cd your-repository-name
```

---

## Backend Setup

### 2. Create and Activate a Python Virtual Environment

```bash
python3 -m venv venv
source venv/bin/activate
```

On Windows:

```bash
python -m venv venv
venv\Scripts\activate
```

### 3. Install Python Dependencies

```bash
pip install fastapi uvicorn pandas chromadb langchain langchain-core langchain-ollama langgraph fastmcp mcp langchain-text-splitters
```

### 4. Install and Start Ollama

Install Ollama from the official website, then pull the Llama 3.2 model:

```bash
ollama pull llama3.2
```

Start Ollama if it is not already running:

```bash
ollama serve
```

---

## Database Setup

The app can create and seed the SQLite database automatically when the backend starts. You can also initialize it manually:

```bash
python data/scripts/init_db.py
```

To build the local policy vector database:

```bash
python data/scripts/build_vector_db.py
```

---

## Running the Backend

From the project root folder, run:

```bash
uvicorn agent_api:app --reload --port 8001
```

The backend will run at:

```text
http://127.0.0.1:8001
```

The React frontend sends chat requests to:

```text
http://127.0.0.1:8001/chat
```

---

## Frontend Setup

Open a new terminal and go to the React app folder:

```bash
cd bank-frontend
```

Install frontend dependencies:

```bash
npm install
```

Start the React development server:

```bash
npm start
```

The frontend will open at:

```text
http://localhost:3000
```

---

## Running the Terminal Agent

You can also run the banking assistant directly in the terminal:

```bash
python run_agent.py
```

Type `exit` or `quit` to stop the terminal chat.

---

## API Endpoint

### `POST /chat`

Sends a user message to the AI banking agent.

#### Request Body

```json
{
  "message": "What is my account balance?",
  "session_id": "react_user_1"
}
```

#### Response

```json
{
  "response": "The current balance for account 1000000002 is $45000.00."
}
```

---

## MCP Banking Tools

The MCP server exposes these tools to the AI agent:

| Tool | Description |
|---|---|
| `get_account_balance` | Retrieves account balance after account number and PIN validation |
| `transfer_funds` | Transfers funds between accounts and logs transactions |
| `get_recent_transactions` | Shows recent transactions for an authenticated account |
| `create_new_account` | Creates a new simulated banking account |
| `query_bank_policy` | Searches or returns banking policy information |

---

## Example Questions

You can ask the assistant:

```text
What is my account balance?
```

```text
Transfer $50 from account 1000000002 to 1000000001.
```

```text
Create a new account for John with $500 initial balance.
```

```text
What is the transfer limit policy?
```

```text
Show my recent transactions.
```

For balance, transfer, and transaction requests, the assistant requires an account number and PIN.

---

## Fraud Detection Logic

The current implementation includes a simple rule-based fraud layer:

- Any transfer above `$2,000` is blocked.
- The transaction is logged as suspicious.
- The agent returns an `ERROR_FRAUD_FLAG` message.

The project report describes an expanded fraud detection design using:

- Logistic Regression
- Random Forest
- Gradient Boosting
- SMOTE for class imbalance
- Z-score anomaly detection
- Isolation Forest
- Intent-based NLP querying

---

## Project Report Highlights

The FSE project report describes a full-stack AI-powered finance and fraud detection copilot that integrates heterogeneous banking datasets, applies supervised and unsupervised ML, and delivers conversational financial insights through a FastAPI backend and React frontend.

Reported ML results include:

| Model | Precision | Recall | F1 | ROC-AUC |
|---|---:|---:|---:|---:|
| Logistic Regression | 0.81 | 0.74 | 0.77 | 0.88 |
| Random Forest | 0.93 | 0.89 | 0.91 | 0.97 |
| Gradient Boosting | 0.95 | 0.92 | 0.93 | 0.98 |

The report also describes a local, privacy-preserving architecture where React handles the UI, FastAPI handles ETL/ML/NLP routing, and SQLite stores structured transaction data.

---

## Privacy and Security Notes

- This is a simulated banking project for academic use.
- Account PINs are stored as SHA-256 hashes.
- Data processing is designed to run locally.
- No real banking credentials should be used.
- Do not deploy this project as a production banking system without proper authentication, authorization, encryption, audit logging, and compliance review.

---

## Recommended `.gitignore`

Before pushing to GitHub, avoid committing unnecessary local files:

```gitignore
# Python
venv/
__pycache__/
*.pyc

# Node
node_modules/
build/

# macOS
.DS_Store
__MACOSX/

# Local databases
local_db/*.sqlite
local_db/chroma_db_storage/

# Environment files
.env
```

---

## Future Enhancements

- Add a dashboard for spending trends and category breakdowns
- Add CSV upload and schema mapping in the frontend
- Implement ML-based fraud scoring endpoint
- Implement anomaly detection endpoint
- Add `/metrics`, `/predict`, `/anomalies`, and `/query` APIs
- Add SHAP explanations for flagged transactions
- Add user authentication and role-based access control
- Add encrypted local storage
- Add transaction history view in the frontend
- Add test cases for backend and frontend
- Dockerize the full-stack application

---

## Authors

- Nikhilesh Goud Bairampally
- Nithin Kumar Surineni
- Pavan Teja Tallapalli
- Sahithi Katoori
- Uday Muncip

Arizona State University  
FSE 570 - Data Science & Analytics Capstone

---

## License

This project is intended for academic and educational use. Add your preferred license before public release.
