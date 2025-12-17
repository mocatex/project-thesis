
## A. Core questions for 2016–2021

### A1) Trump election & presidency – media tone vs market risk

**Question:**
How did media sentiment towards Donald Trump evolve from the 2016 campaign through his presidency (2016–2020), and how did it relate to US equity market volatility around key events (e.g., election night, trade war escalations, COVID crash)?

**External tools:**

* BM25 + MiniLM retriever for “Trump”, “President Trump”, “US election 2016”, “trade war”, “impeachment”, etc.
* FinBERT for sentiment.
* S&P 500 index + VIX time series via finance API.
* Changepoint detection (ruptures).

**Pipeline sketch:**

1. Query by person + roles + events (“Donald Trump”, “Trump administration”, “US president”).
2. Normalize dates; group by week.
3. FinBERT sentiment per article -> weekly average.
4. Changepoint detection on (a) sentiment, (b) VIX / S&P drawdowns.
5. Align sentiment breaks to major events (election, travel ban, trade war phases, impeachment, COVID shock).
6. LLM: timeline summary with “before/after event” comparisons and a short paragraph linking sentiment to volatility (but clearly separating correlation vs causation).

---

### A2) Facebook / Cambridge Analytica – from growth story to “techlash”

**Question:**
How did media coverage of Facebook shift from growth/innovation framing to privacy/regulation framing around the Cambridge Analytica scandal (2016–2019), and how did this relate to FB’s stock drawdowns?

**External tools:**

* Retriever: “Facebook”, “Meta”, “social network”, “Cambridge Analytica”.
* Entity linking (Facebook Inc. / Meta Platforms).
* BERTopic for topic themes (growth, ads, privacy, regulation, misinformation).
* FinBERT for sentiment.
* Stock prices for FB/Others (e.g., SNAP, TWTR) via finance API.

**Pipeline sketch:**

1. Retrieve Facebook-related articles 2016–2019.
2. Apply NER+EL to filter articles talking about the *company*, not just “facebook post”.
3. Monthly topic modeling (BERTopic) → label topics manually/LLM (“growth”, “privacy”, “regulation”, “misinformation”).
4. FinBERT sentiment per article → monthly mean.
5. Overlay topics + sentiment with FB stock returns and key dates (CA revelations, US/EU hearings).
6. LLM: “pre-CA vs post-CA” narrative: topic share changes, sentiment drift, how markets reacted.

---

### A3) COVID-19 – from outbreak to vaccines

**Question:**
How did media tone and topics around COVID-19 evolve from early outbreak (late 2019 / early 2020) to vaccine rollout in 2021, and how did health vs economy vs civil-liberty frames compete?

**External tools:**

* Retriever: “coronavirus”, “COVID-19”, “SARS-CoV-2”, “lockdown”, “vaccine”.
* BERTopic for topics (healthcare, lockdown, economy, schools, vaccines, protests).
* Sentiment: VADER + FinBERT for document tone.
* External data: case counts / stringency index (e.g., Oxford Index) if you want.

**Pipeline sketch:**

1. Filter COVID articles 2019–2021.
2. Segment time into phases (outbreak, first lockdowns, second wave, vaccines).
3. Topic modeling per phase → topic distributions (health, econ, politics, protest).
4. Sentiment per article → average per topic & phase.
5. Optionally correlate topic intensity with external stringency / case metrics.
6. LLM: phase-by-phase narrative (“panic & uncertainty”, “lockdown fatigue”, “vaccine hope”, etc.).

---

### A4) Greta Thunberg & youth climate activism

**Question:**
How did media coverage of Greta Thunberg and youth climate activism evolve from 2018–2021, and how did the framing (personalization vs policy vs skepticism) vary across regions/outlet types?

**External tools:**

* Retriever: “Greta Thunberg”, “school strike for climate”, “Fridays for Future”.
* Entity linking (person, movement).
* Outlet metadata (region, type if available, or heuristic using domain).
* BERTopic for themes (personal story, protests, UN speeches, policy, backlash).
* FinBERT / VADER for sentiment (per region & outlet type).

**Pipeline sketch:**

1. Retrieve Thunberg/youth climate articles.
2. Label each article by region/outlet type (e.g., EU vs US; broadsheet vs tabloid if inferable).
3. Topic modeling → topics like “UN speech”, “EU policy”, “criticism/skepticism”.
4. Sentiment per article and per topic × region.
5. Time-series: topic share + sentiment by quarter.
6. LLM: narrative on how coverage shifted (from curiosity → movement → polarization) and cross-region comparisons.

---

### A5) Crypto boom & bust 2016–2021

**Question:**
How did media tone and themes around Bitcoin and cryptocurrencies change across the 2017 bull run, the 2018 crash, the 2020–2021 bull market, and how did that relate to BTC price cycles?

**External tools:**

* Retriever: “Bitcoin”, “cryptocurrency”, “Ethereum”, “crypto exchange”.
* BTC (and possibly ETH) price series via crypto API.
* BERTopic for themes (tech, speculation, regulation, crime, institutional adoption, DeFi, NFTs).
* FinBERT/VADER for sentiment.

**Pipeline sketch:**

1. Filter crypto articles 2016–2021.
2. Define macro phases from price series (bull, crash, consolidation, next bull) via changepoint detection.
3. Topic modeling within each phase → distribution of themes.
4. Sentiment per phase, per topic (e.g., “regulation” vs “innovation”).
5. Correlate topic/sentiment shifts with price turning points.
6. LLM: “what changed before vs after” each major cycle.

---

### A6) Tesla & EV mainstreaming

**Question:**
How did media sentiment towards Tesla and electric vehicles evolve from 2016–2021, especially around Autopilot accidents, Model 3 production issues, and inclusion in the S&P 500?

**External tools:**

* Retriever: “Tesla”, “electric vehicle”, “EV”, “Autopilot”.
* Entity linking (Tesla Inc., Elon Musk).
* FinBERT for sentiment.
* BERTopic for themes (technology, safety, production, competition, regulation).
* Stock prices (TSLA, maybe legacy automakers).

**Pipeline sketch:**

1. Retrieve Tesla/EV articles 2016–2021.
2. Event tagging: Autopilot crashes, “production hell”, S&P 500 inclusion, big rallies/crashes.
3. Weekly sentiment series; mark event windows.
4. Topic modeling to see which themes spike around each event.
5. Overlay sentiment + topics with TSLA stock price moves.
6. LLM: timeline story of “hype vs safety concerns vs financial market acceptance”.

---

### A7) US–China trade war & Huawei / 5G

**Question:**
How did Western media coverage of Huawei and the US–China trade war evolve between 2018 and 2021, and how did security vs economic vs tech-innovation frames compete?

**External tools:**

* Retriever: “Huawei”, “5G ban”, “US–China trade war”, “tariffs”.
* Entity linking (Huawei Technologies, US/China).
* Outlet region detection (US vs EU vs others).
* BERTopic for themes (security, espionage, trade, technology, diplomacy).
* FinBERT sentiment.

**Pipeline sketch:**

1. Retrieve Huawei/trade war articles 2018–2021.
2. Group by region of outlet.
3. Topic modeling per region + year.
4. Sentiment per topic + region over time.
5. LLM: contrast narratives (e.g., US security focus vs EU balancing act) and evolution as bans, sanctions, and 5G rollouts progress.

---

## B. “Hard” multi-hop / tool-composition questions

Here the *point* is that CorpusAgent **must first resolve an indirection** using a tool (Wiki/Wikidata/Google) and then query the news corpus with the resolved entity + time window.

### H1) Multi-hop spouse → person → corpus

**Question:**
How did media coverage of the spouse of the Microsoft co-founder who runs a major philanthropic foundation change around the 2021 divorce announcement?

(= Melinda French Gates, using Bill Gates → spouse.)

**Tools:**

* Web/Wikidata lookup: “spouse of Microsoft co-founder Bill Gates” → Melinda French Gates.
* Date lookup: “Gates divorce announcement date”.
* Retriever: articles mentioning Melinda Gates (not just Bill Gates) between 2016–2021.
* FinBERT + BERTopic.

**Pipeline sketch:**

1. **Tool step 1 (KB):** Resolve description → canonical entity (Melinda French Gates).
2. **Tool step 2 (KB):** Get divorce announcement event date (2021).
3. Retrieve articles on Melinda Gates 2016–2021.
4. Time segments: pre-announcement vs post-announcement.
5. Sentiment + topic modeling per segment.
6. LLM: contrast philanthropic-focus coverage vs personal/divorce-related coverage.

---

### H2) Parent company indirection (Instagram → Facebook/Meta)

**Question:**
How did media sentiment towards the company that owns Instagram change before vs after the Cambridge Analytica scandal?

**Tools:**

* Web/KB: “company that owns Instagram” → Facebook/Meta.
* Event date lookup for CA scandal.
* Retriever: Facebook/Meta articles (not just the product “instagram photo”).
* FinBERT + BERTopic.

**Pipeline sketch:**

1. Resolve “company that owns Instagram” via KB → {Facebook Inc., Meta Platforms}.
2. Get CA scandal window (e.g., March–April 2018).
3. Retrieve FB/Meta articles 2016–2019.
4. Cut into pre-CA vs post-CA.
5. Sentiment + topic modeling (growth vs privacy/regulation).
6. LLM: compare sentiment and topic distributions.

---

### H3) Role + other company (SpaceX → Tesla)

**Question:**
How did media coverage of the electric-car company led by the founder of SpaceX change after the 2018 SEC settlement over tweets?

(= Tesla, Elon Musk.)

**Tools:**

* KB: “founder of SpaceX” → Elon Musk → “electric-car company led by him” → Tesla.
* KB: date of SEC settlement.
* Retriever: Tesla-related articles.
* FinBERT + BERTopic; TSLA stock.

**Pipeline sketch:**

1. Resolve description to {Tesla, Elon Musk}.
2. Look up SEC settlement date/window.
3. Retrieve Tesla/Elon-Tesla articles 2016–2021.
4. Pre- vs post-settlement sentiment and topics (governance, Twitter, volatility).
5. Overlay with TSLA price.
6. LLM: describe if tone about governance/CEO behavior changed.

---

### H4) Movement figurehead (Thunberg, “How dare you” speech)

**Question:**
How did global media coverage of the youth climate activist who delivered the “How dare you” speech at the UN in 2019 change between 2018 and 2021?

(= Greta Thunberg.)

**Tools:**

* KB: resolve description → Greta Thunberg; get UN Climate Action Summit 2019 date.
* Retriever: Thunberg-related articles.
* Outlet country inference.
* BERTopic + FinBERT.

**Pipeline sketch:**

1. Resolve description and UN speech date.
2. Retrieve all Thunberg articles 2018–2021.
3. Time segments: pre-speech, immediate post-speech, long-run afterward.
4. Topic modeling (personal, activism, policy, backlash).
5. Sentiment by topic & region.
6. LLM: contrast pre-speech emergence vs post-speech global recognition and polarization.

---

### H5) Owner/brand mapping (WhatsApp → Meta)

**Question:**
How did media tone towards the company that owns WhatsApp change around the 2016–2017 encryption and data-sharing debates?

**Tools:**

* KB: “company that owns WhatsApp” → Facebook/Meta.
* Retriever: FB/Meta + WhatsApp articles 2016–2018.
* FinBERT + BERTopic.

**Pipeline sketch:**

1. Resolve company via KB.
2. Retrieve corpus where both {Facebook/Meta} and {WhatsApp / end-to-end encryption / data sharing} occur.
3. Time segments around key announcements (end-to-end encryption, data-sharing with FB).
4. Topics: privacy, security, regulation, user trust.
5. Sentiment per segment.
6. LLM: describe trust/privacy narrative shift.

---

## C. Negative-control / “no real correlation” debugging questions

Here you *want* CorpusAgent to (a) run the pipeline, (b) find no robust signal, and (c) explicitly say “no meaningful relationship”.

### N1) Bitcoin vs random consumer staples

**Question:**
Did Bitcoin price movements between 2016 and 2021 have any systematic impact on media sentiment towards Colgate-Palmolive?

**Expected reality:** basically no meaningful link.

**Tools:**

* Retriever: “Colgate-Palmolive” or ticker, plus “Bitcoin price” time series.
* FinBERT sentiment on Colgate-related articles.
* Correlation / Granger causality tests between BTC returns and Colgate sentiment/volume.

**Pipeline sketch:**

1. Retrieve Colgate articles 2016–2021.
2. Build monthly/weekly sentiment + volume series for Colgate.
3. Align with BTC returns/volatility.
4. Run simple correlations / Granger tests.
5. If effect sizes are tiny/unstable: LLM should say “no evidence of systematic impact; any apparent correlations are likely noise”.

---

### N2) Oscars vs bank stock sentiment

**Question:**
Did the number of Oscars won by films in a given year meaningfully affect media sentiment towards large US bank stocks (e.g., JPMorgan, Bank of America) in 2016–2021?

**Expected reality:** no causal link.

**Tools:**

* KB/API: get Oscars count per year.
* Retriever: bank-stock articles (JPM, BAC, etc.) per year.
* FinBERT for sentiment.
* Basic regression/correlation between Oscars count and bank sentiment.

**Pipeline sketch:**

1. Fetch yearly Oscars counts 2016–2021.
2. Retrieve bank-related articles; compute yearly sentiment per bank.
3. Correlate Oscars counts with bank sentiment / coverage volume.
4. LLM: explicitly check for significance; expected answer: “no robust relationship”.

---

### N3) Weather vs OPEC sentiment

**Question:**
Is there any meaningful relationship between daily weather in Zurich and media sentiment towards OPEC decisions in 2016–2021?

**Expected reality:** none.

**Tools:**

* Weather API (Zurich daily temperature / rainfall).
* Retriever: “OPEC”, “oil cartel”, “OPEC meeting” etc.
* FinBERT for sentiment on OPEC articles.
* Time-series correlation.

**Pipeline sketch:**

1. Retrieve OPEC articles; map to dates; compute daily/weekly sentiment.
2. Get corresponding Zurich weather time series.
3. Run correlations and changepoint analyses.
4. LLM: report “no meaningful pattern; any alignment is coincidental” if nothing strong appears.

---

### N4) eSports results vs central bank coverage

**Question:**
Did major eSports tournament results (e.g., League of Legends Worlds winners) cause measurable shifts in media sentiment about central bank policy (Fed/ECB) between 2016 and 2021?

**Expected reality:** no connection.

**Tools:**

* KB: list Worlds winners & dates.
* Retriever: central bank policy articles (Fed, ECB, “interest rate decision”, “monetary policy”).
* FinBERT for sentiment.
* Event-study setup around tournament finals.

**Pipeline sketch:**

1. Get tournament final dates.
2. Build sentiment timeline for central-bank articles.
3. Event-study around those dates (± X days).
4. LLM: expected to say “no consistent shift around eSports events”.

---

## How to use these

* **Core questions (A)** → main demos/experiments in the thesis.
* **Hard ones (B)** → evaluate tool-calling / decomposition (did the agent:
  a) resolve the description correctly,
  b) choose sensible time windows,
  c) then run the corpus pipeline?).
* **Negative controls (C)** → sanity checks against hallucination. For them, define the *correct* answer as:
  “We ran retrieval + stats and found no robust relationship; here’s a brief explanation.”
