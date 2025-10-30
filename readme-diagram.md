## Understanding the Pipeline

### 1. User
- The user asks a complex temporal question.
- Something in the terms of "How did the public opinion on Apple change from 2007-2015?"
- This is the input query that starts the system.

### 2. Planner
- The planner is the brain and "thinks" "reasons" before acting. It figures out how to answer the question.
- It understands the questions type -> temporal sentiment/trend analysis.
- Decomposes into different timelines (e.g., 2007-2010, 2011-2015).
- Generates retrieval prompts -> What to search for in each period ("Apple iPhone public opinion)

Something like this could return from that 

```json
{ "periods": ["2007–2010","2013–2015"],
  "aspect_queries": { ... },
  "topK": 200 }
``` 

### 3. Retriever
- The retriever finds with the help of FAISS the most semantically similiar texts to this query.
- But also uses BM25 to get keyword matches.
- Returns relevant documents for each time period.
- Filter the documents by year
- General combines semantic + keyword search.
- Return top k documents per period.

### 4. Docs
- Document ID's come from the corpus - All CNN Files for example.
- Fetches the full text of the documents from the corpus.

### 5. Analyzer / Analytics Model
- FinBERT -> Calculates sentiment scores (positive/negative/neutral) for each document.
- BERTopic -> Clusters articles into themes and topics per time period.

### 6. Aggregator
- Combines everthing, computes sentiment per period (0.65 -> 0.42 etc).
- Lists top topics

### 7. External Signals 
- Fetches stock price data for Apple from a Finance API.
- Fetches Google Trends data for Apple related searches.
- Aligns these external signals with the time periods.