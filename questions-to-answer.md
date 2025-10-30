# Questions our system should answer

### 1) Volkswagen “Dieselgate” - how did coverage and tone evolve, and how did it relate to VW’s stock moves?

* **External tools:** Stock prices (e.g., Alpha Vantage/Yahoo Finance), date normalization (HeidelTime/SUTime), FinBERT sentiment.
* **Pipeline sketch:**
  NER+Entity Linking (VW AG) -> de-dup & cluster by week -> FinBERT sentiment per cluster -> changepoint detection on sentiment & price (ruptures) -> timeline summary with key milestones.

### 2) Samsung Galaxy Note 7 recall - did brand sentiment shift, and which themes (safety, batteries, airlines) dominated when?

* **External tools:** FinBERT or aspect-based sentiment; product-*variant* entity linking; airline bans list (optional).
* **Pipeline sketch:**
  Product NER + coref ->  topic modeling (BERTopic) by month -> aspect sentiment (battery/safety/brand) -> timeline w/ peaks -> cross-outlet comparison (tech vs general press).

### 3) Bitcoin cycles 2013-2017 - how did media tone & topics correlate with BTC price spikes/crashes?

* **External tools:** BTC price series; FinBERT/VADER; BERTopic; changepoint detection.
* **Pipeline sketch:**
  Filter articles with “Bitcoin/cryptocurrency” -> monthly topic proportions + sentiment -> correlate with price level/returns -> generate “what changed before/after” summaries.

### 4) Brexit 2016 - what arguments for/against dominated *before* the vote vs. *after*?

* **External tools:** Stance/argument mining model; date normalization; (optional) UK equity index/GBPUSD for context.
* **Pipeline sketch:**
  Query span (Jan-Jul 2016, Jul-Dec 2016) -> stance classifier (pro/contra) -> argument unit extraction (claim+evidence) -> contrastive summary (before vs after) + optional market overlay.

### 5) Oil price crash (2014-2016) - impact on airlines vs. oil majors in coverage & markets

* **External tools:** Brent/WTI price series (EIA/FRED), equity prices for representative firms, sector mapping.
* **Pipeline sketch:**
  Articles mentioning oil price + sector NER -> group by sector -> sentiment/time series per sector -> diff-in-diff style comparison vs oil price path -> synthesis.

### 6) “AI/Deep Learning” rise (2012-2017) - topic evolution and which firms were most associated

* **External tools:** BERTopic; entity linking (company <-> research lab); (optional) funding datasets.
* **Pipeline sketch:**
  Identify DL/AI lexicon -> yearly topic distributions -> co-mention network (company<->topic) -> “emergent themes over time” narrative.

### 7) How has the public perception of Apple evolved between the launch of the first iPhone and the Apple Watch? (2007–2015)
* **External tools:**:
- Use of LLM's to decompose the question into sub-questions. 
- Sentiment analysis models (ProsusAI/finBERT) are based on BERTA for sentiment analysis over time.
- Topic Extraction: BERTopic for dominant discussion clusters.
- Summarization: LLM's for generating concise summaries. GPT or similar models.
- Finance API for stock prices or Google Trends API for public interest trends.
* **Human Approach:** A human would probably:
  1. Read articles from 2007-2015 about Apple, iPhone, Steve Jobs, Apple Watch and Tim Cook.
  2. Note tone Shifts like Excitment around Key Launches like iPhone and Apple Watch but also criticism around issues like Antennagate and Labor Practices.
* **Pipeline Sketch:** Question -> Decompose (Time + Aspect) -> Retrieve -> Analyze (Sentiment via FinBERT) -> Aggregate (Mean Score per period) -> Contrast (Tone Difference + Topic Shift) -> Summarize (LLM Synthesis) -> Evaluate (Hit-Rate + Trend Alignment)

### 8) How did media sentiment towards Barack Obama change from his 2008 campaign to the end of his first term in 2012?
* **External tools:** MiniLM Retriever, FinBERT for sentiment analysis, BERTopic for topic modeling and GPT-4 for summarization.
* **Pipeline Sketch:**
  1. Question
  2. Decompose into time segments (2008 campaign, 2009-2012 first term)
  3. Retrieve articles mentioning Barack Obama in each segment
  4. FinBERT + BERTopic Analysis per Segment to get sentiment scores and dominant topics
  5. Aggregate sentiment scores and topic distributions per segment
  6. Contrast sentiment and topics between segments
  7. Summarize findings using GPT-4

### 9) How did the media coverage of the 2008-2009 financial crisis evolve into discussion of accountability by 2012 or later ?
* **External Tools:** BM25 + MiniLM as for Retrieval, BERTopic for topic modeling, NLI Stance Model for accountability detection, GPT-4 for summarization.
* **Pipeline Sketch:**
  1. Question
  2. Decompose into time segments (2008-2009 crisis, 2010-2012 accountability)
  3. Retrieve articles mentioning financial crisis and accountability in each segment
  4. BERTopic Analysis per Segment to get dominant topics
  5. Stance Detection on accountability-related articles
  6. Aggregate topic distributions and accountability stances per segment
  7. Contrast topics and stances between segments
  8. Summarize findings using GPT-4

### 10) Climate Change Coverage Shift - How did media coverage evolve from early skepticism to global-acting framing (Paris 2015) ?
* **External tools:** MiniLM Retriever, VADER + FinBERT for sentiment analysis, BERTopic for topic modeling, GPT-4 for summarization.
* **Pipeline Sketch:** 
  1. Question
  2. Decompose into time segments (pre-2010 skepticism, post-2015 global action)
  3. Retrieve articles mentioning climate change in each segment
  4. VADER + FinBERT Analysis per Segment to get sentiment scores
  5. BERTopic Analysis per Segment to get dominant topics
  6. Aggregate sentiment scores and topic distributions per segment
  7. Contrast sentiment and topics between segments
  8. Summarize findings using GPT-4

### 11) Electric Vehicle Acceptance / Adaptation - How did attitudes towards EVs shift from skepticism (First Roadster in 2008) to mainstream adoptation (Model S and beyond) ?
* **External tools:** MiniLM Retriever, FinBERT for sentiment analysis, BERTopic for topic modeling, GPT-4 for summarization. Google Trends APi for public interest trends.
* **Pipeline Sketch:** 
  1. Question
  2. Decompose into time segments (2008-2012 early skepticism, 2013-2020 mainstream adoption)
  3. Retrieve articles mentioning electric vehicles in each segment
  4. FinBERT + BERTopic Analysis per Segment to get sentiment scores and dominant topics
  5. Aggregate sentiment scores and topic distributions per segment
  6. Contrast sentiment and topics between segments
  7. Summarize findings using GPT-4




## template to add more questions later

* **Sample question:** *What changed in X between Y1 and Y2, and how did Z react?*
* **External tools:** *(list data feeds/models you’ll consult)*
* **Signals to compute:** Entities, time windows, sentiment/topics, events, relations.
* **Retrieval plan:** Query -> filter -> cluster (by week/month) -> deduplicate -> select exemplars.
* **Synthesis plan:** Summarize per segment -> contrast segments -> optionally correlate with external data -> produce timeline + short answer.
* **Evaluation idea:** Compare against known milestones; measure retrieval hit-rate; judge summary quality.
