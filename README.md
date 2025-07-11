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

# 🧠 Context-Aware Follow-Up Engine (CAFÉ)

### 🚀 Goal  
Enhance the InvestIQ AI intelligence by integrating conversation history when required.

---

### 📝 Description  
Bringing **Context-Aware Follow-Up Engine** to our chat system.  
The engine smartly determines whether the user's current query depends on previous interactions.  

- If the query **is context-dependent (i.e., a follow-up)**, the LLM uses relevant chat history to generate a more accurate and coherent response.
- If the query **is standalone**, the LLM processes it independently without pulling prior context.

This selective use of history leads to:
- Improved relevance in responses
- Lower token usage
- More scalable and efficient chat handling

---
### 🧠 Context-Aware Follow-Up Engine (CAFÉ)  

The **CAFÉ** pipeline works as follows:

1. **Intent & Follow-Up Detection**  
   - Detects whether the current user query is a follow-up or a standalone question.

2. **Context Retrieval (if needed)**  
   - If follow-up, retrieves relevant parts of the previous conversation using the user's chat history.

3. **Query Reconstruction**  
   - Resolves ambiguous references in the question (e.g., “it”, “that fund”) by replacing them with actual fund names or entities from the context.

4. **Prompt Construction & LLM Call**  
   - Constructs the final prompt — with or without context — and queries the LLM for response generation.

---

### ✅ Benefits
- Reduces unnecessary use of tokens
- Improves LLM accuracy and user experience
- Lays the foundation for intelligent multi-turn dialogue
```
User Question
     |
     v
[ 🧠 Reasoning Step ]
→ Extracts:
   - Fund Name(s)
   - User Intent
   - Question Category
   - Date (if any)
   - Follow-up (True/False)
     |
     v
[ 🧱 Structuring Step ]
→ Converts Reasoning Output into a structured JSON format
     |
     |
     |── If Follow-Up:
     |     ├── Extract fund names from previous messages
     |     ├── Reconstruct the current query by resolving ambiguous terms (e.g., “it”)
     |     ├── No RAG needed — LLM gets precise standalone input
     |     └── Proceed with the newly formed query
     |
     ├── Get Similar Fund(s) using Embeddings
     |     └── Uses vector similarity (no LLM)
     |
     ├── Get Fund Details via API
     |
     ├── Extract relevant dates from structured data
     ├── Call 3rd-party API to get NAV on each date
     └── Calculate returns & form a structured context
     |
     ├── If Category == "HOLDINGS":
     |     ├── Google fund name + “portfolio holdings”
     |     ├── Scrape HTML from trusted finance sites
     |     ├── Parse asset allocation, top 10 holdings, etc.
     |
     v
[ ✨ Final Response Generation — LLM ]
→ Query the LLM using all gathered context and categories
→ Generate a user-friendly, well-structured response

```
# Custom Chat History Management (In case of follow up questions):- 
- Logged-In Users:
  - History is retrieved from NeonDB only when a query is a follow-up.
- Anonymous Users:
  - Context is passed via frontend state only (no DB calls).

# 🔁 Smart Fund Matching (Embeddings)
For every user-queried fund:
- Use vector similarity to match from 30,000+ mutual funds.
- Extract ISIN of the best-matched fund.
- Make precise API calls using the ISIN.

🧾 Returns & NAV Calculation
- Get full NAV history via Kuvera API using ISIN.
- Calculate point-to-point returns over 1–10 years.
- Format this into a human-readable performance summary.
  
⚠️ API Limitation
Kuvera API only provides NAV data for the last ~2 years. For older return calculations, premium APIs (e.g., Groww/Paytm) would be needed in the future.

# 📌 Example Use Cases
- "What are the holdings of JM Flexi Cap Fund?"
- "Show 3-year return for ICICI Prudential Flexicap Fund"
- "Compare Axis Bluechip with ICICI Pru Flexicap"
- "What is the NAV of that fund?" ← (Follow-up logic applies)

  | Resource             | Purpose                                  |
| -------------------- | ---------------------------------------- |
| **Gemini 1.5 & 2.0** | Reasoning, Structuring, Query Resolution |
| **LangChain**        | Embeddings, Retrieval, Prompt Chaining   |
| **Kuvera API**       | Fund NAV and return data (2 years only)  |
| **Web Scraping**     | Portfolio holdings and asset breakdown   |
| **NeonDB**           | User history, auth, chat context         |
| **React.js**         | Frontend client                          |
| **FastAPI**          | Backend server and LLM pipeline handler  |

