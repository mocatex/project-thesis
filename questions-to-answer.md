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

---

## template to add more questions later

* **Sample question:** *What changed in X between Y1 and Y2, and how did Z react?*
* **External tools:** *(list data feeds/models you’ll consult)*
* **Signals to compute:** Entities, time windows, sentiment/topics, events, relations.
* **Retrieval plan:** Query -> filter -> cluster (by week/month) -> deduplicate -> select exemplars.
* **Synthesis plan:** Summarize per segment -> contrast segments -> optionally correlate with external data -> produce timeline + short answer.
* **Evaluation idea:** Compare against known milestones; measure retrieval hit-rate; judge summary quality.
