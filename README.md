# 🧠 InvestIQ — Your Mutual Fund Copilot

**InvestIQ** is an intelligent AI-powered chatbot that serves as your one-stop assistant for all mutual fund-related queries. Whether you’re tracking NAV history, evaluating returns, reviewing holdings, or simply exploring fund performance — InvestIQ understands your question and delivers precise, context-aware responses.

Built with 💡 LangChain, ⚡ Gemini Flash (1.5 & 2.0), 🔍 web scraping, 🧠 custom LLM pipelines, and 🗃️ NeonDB — the system is highly modular, fast, and handles follow-up queries with intelligence and clarity.

---

## 🛠️ Tech Stack

- **Frontend**: React.js  
- **Backend**: FastAPI (Python)  
- **LLM**: Gemini 1.5 Flash & Gemini 2.0  
- **Embeddings & Retrieval**: LangChain + Vector Search  
- **Database**: NeonDB (PostgreSQL)  
- **Authentication**: Custom JWT-based flow  
- **Mutual Fund Data Source**: Kuvera API  
- **Additional Data**: Web Scraping (Holdings, Allocation)

---

## 🔍 Key Features

### 🔹 Natural Language Chatbot
- Ask queries like “What are the holdings of HDFC Small Cap?” or “Show me the 5-year return of JM Flexi Cap Fund.”
- Understands complex follow-up questions like “What about its NAV last year?” using smart context tracking.

### 🔹 Reasoning Step
- Extracts key information from the query using Gemini Flash (LLM):
  - Fund Name(s)
  - Intent (FUND, COMPARISON, etc.)
  - Category (NAV, RETURNS, HOLDINGS, etc.)
  - Date (if provided)
  - Follow-up (boolean)

### 🔹 Structuring Step
- Converts reasoning output into a clean structured JSON like:
  ```json
  {
    "fund_names": ["JM Flexi Cap Fund"],
    "intent": "FUND",
    "category": ["HOLDINGS"],
    "date": null,
    "follow_up": false
  }
