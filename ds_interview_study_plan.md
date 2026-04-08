# DS + ML Engineer Interview Study Plan
### Staff-Level | Series C+ & Late-Stage Targets | 9–10 Month Timeline

---

## Learning Approach: Theory First, Then Practice, Then Write It Up

Every topic follows the same three-phase cycle:

1. **Learn the theory** — Understand the concept in plain language first. What is it? Why does it exist? When would you use it vs. something else? Build intuition before touching any math or code.
2. **Practice it** — Apply what you learned. Start with guided exercises, then work up to interview-style problems and F1 project application.
3. **Write it up in a notebook (.ipynb)** — Create a Jupyter notebook for each topic that combines your own explanations (in markdown cells) with code examples and experiments. Write it as if you're teaching someone else. This forces clarity, gives you a personal reference library, and builds a portfolio.

Don't skip to practice early. If you can't explain a concept simply to a non-technical person, you don't know it well enough yet.

### Notebook Structure (use for every topic)
- **Section 1: What is it?** — Plain English explanation in your own words
- **Section 2: Why does it matter?** — When you'd use it, when you wouldn't
- **Section 3: How it works** — Code walkthrough with a simple example (toy data or F1 data)
- **Section 4: Experiment** — Change something, observe what happens, explain why
- **Section 5: Interview answer** — Write out your 60-second verbal explanation of this topic

---

## Weekly Schedule (~7 hrs/week)

| Day | Block | Focus | Time |
|-----|-------|-------|------|
| **Tuesday** | Evening | Theory session (current topic) | 1.5 hrs |
| **Thursday** | Evening | SQL + Coding Drills | 1.5 hrs |
| **Saturday** | Morning | Practice session (current topic) + Stats theory | 3 hrs |
| **Sunday** | Flex | Apply learnings to F1 project | 1 hr |

> Tuesday = learn the concept. Saturday = practice it. Thursday = keep SQL sharp. Sunday = make it real in your F1 project.

---

## Area 1: ML Fundamentals (~25% of time)

**Goal:** Build clear, explainable understanding of core ML concepts — the kind where you could teach each one to someone new.

### Learning Sequence (theory → practice for each topic)

**Topic 1: Decision Trees (Weeks 1–2)**
- Theory: What is a decision tree? How does it decide where to split? What makes a split "good"? Gini impurity vs. information gain in plain English. Why do single trees overfit?
- Practice: Walk through a split decision by hand with a tiny dataset. Then look at a single tree from your F1 model and interpret it.

**Topic 2: Random Forests (Weeks 3–4)**
- Theory: What problem do random forests solve that single trees can't? What is bagging? Why does averaging many trees reduce overfitting? What's the role of randomness?
- Practice: Compare a single decision tree vs. a random forest on your F1 data. Look at feature importances from both.

**Topic 3: Gradient Boosting & XGBoost (Weeks 5–8)**
- Theory week 1: The core idea — each tree corrects the previous one's mistakes. What are residuals? Why "gradient"? How is this different from random forests?
- Theory week 2: The knobs that matter — learning rate, max depth, number of trees, subsampling. What each one controls and why you'd turn it up or down.
- Practice week 1: Take your F1 model. Change one hyperparameter at a time, observe what happens. Write a sentence explaining each result.
- Practice week 2: Overfitting exercise — intentionally overfit your F1 model (deep trees, high learning rate, no regularization), then fix it step by step.

**Topic 4: Regularization (Weeks 9–10)**
- Theory: What is regularization in plain English? Why do models overfit? L1 vs. L2 — what each one does and when you'd pick one. How regularization shows up in XGBoost (reg_alpha, reg_lambda).
- Practice: Experiment with L1/L2 on your F1 model. Compare feature importances with and without strong regularization. Can you explain what changed and why?

**Topic 5: Model Performance Interpretation (Weeks 11–14)**

This is where most people plateau — they know what R² is, but can't explain what it means when two metrics disagree. Staff-level candidates need to diagnose model behavior, not just report numbers.

- Theory week 1: What each metric actually measures.
    - R² (coefficient of determination) — "what fraction of the variance does my model explain?" Not how accurate it is. A model can have low R² and still be useful if the domain is inherently noisy.
    - Pearson correlation — "does my model's output move in the right direction with the target?" Captures linear relationship strength but ignores scale. A model that's always off by 10 can still have perfect Pearson.
    - MAE / RMSE — "how far off are my predictions on average?" RMSE punishes big errors more. When RMSE is much larger than MAE, you have outlier predictions.
    - Precision / recall / F1 — for classification. Precision = "when I say yes, am I right?" Recall = "do I catch all the real yeses?" The tradeoff depends on the cost of false positives vs. false negatives.
    - AUC-ROC vs. AUC-PR — ROC can be misleading with imbalanced classes. PR curves are more honest when positives are rare.
    - Log loss — "how confident and correct are my probability estimates?" Punishes confident wrong predictions harshly.
    - Calibration — "when my model says 70% probability, does it happen 70% of the time?"

- Theory week 2: What it means when metrics disagree — this is the real skill.
    - Low R² but high Pearson: Your model gets the *ranking* right but misses the *scale*. Common in noisy domains. The model captures the trend but the target has lots of variance it can't explain. Often fine for "which driver will do better" but not "by exactly how many seconds."
    - High R² but poor calibration: The model explains variance well overall but its probability estimates are off. Dangerous for decision-making — "the model says 90% chance" might really be 60%.
    - Good accuracy but bad recall: The model is right most of the time by defaulting to the majority class. Useless if the minority class is what you care about (e.g., rare failures, churning users).
    - Low RMSE but high MAE close to RMSE: The model is consistently slightly wrong rather than occasionally very wrong. That's actually a good sign.
    - High RMSE relative to MAE: A few predictions are way off. Investigate outliers — is there a subgroup the model fails on?
    - Good training metrics, bad test metrics: Overfitting. But also check — is the test set from a different distribution? Time-based splits can reveal this.

- Practice week 1: Run your F1 model and compute R², Pearson, MAE, and RMSE simultaneously. Do they tell the same story? Where do they disagree, and what does that tell you about your model's behavior?
- Practice week 2: Intentionally build a "bad" model (e.g., predict with only one feature) and a "good" model. Compare all metrics side by side. Write a paragraph explaining to a stakeholder why you'd pick one over the other — using the metrics as evidence, not just "this number is higher."

**Topic 6: Neural Network Foundations (Weeks 15–16)**
- Theory: What is a neural network at its simplest? Layers, weights, activation functions — in plain English. What is backpropagation intuitively? (The network checks its answer, then works backwards to figure out which weights to adjust.) Why deep learning works for some problems (images, text) but not others (tabular data with 50 features).
- Practice: No coding required here — the goal is conceptual. Be able to draw a simple network on a whiteboard and explain the forward pass. Know when you'd recommend a neural network vs. XGBoost and why.

**Topic 7: Transformers & LLMs (Weeks 17–20)**

You don't need to build a transformer from scratch for DS roles. You need to understand how they work, what they're good at, and how to use them as a tool in your DS workflow.

- Theory week 1: The building blocks.
    - What problem did transformers solve? (Sequential processing was slow; attention lets the model look at everything at once.)
    - Attention in plain English: "For each word, decide how much to pay attention to every other word." That's it. Self-attention is the model doing this for its own input.
    - Encoder vs. decoder: Encoder reads and understands (BERT). Decoder generates (GPT). Some models do both.
    - Why transformers dominate NLP: they handle long-range dependencies, they parallelize well, they scale.

- Theory week 2: LLMs and how they're used.
    - What makes an LLM "large"? Parameters, training data, compute.
    - Pre-training vs. fine-tuning vs. prompting — three ways to use LLMs, from most to least effort.
    - Embeddings: LLMs can turn text into numbers that capture meaning. Useful for search, clustering, classification without building a model from scratch.
    - RAG (retrieval-augmented generation): combine search with LLMs. Why this matters for real-world applications.
    - Limitations: hallucination, context windows, cost, latency. When NOT to use an LLM.

- Practice week 1: Use an LLM API (OpenAI or Anthropic) to do something useful with your F1 data — e.g., generate natural language race summaries from structured data, or classify driver radio messages by sentiment.
- Practice week 2: Create text embeddings from race commentary or driver descriptions. Cluster them. Does the clustering make sense? This is a practical DS application of transformers.

### Resources
- **StatQuest YouTube** — watch the videos for each topic *before* your Tuesday theory session; they set up the intuition perfectly
- **Hands-On ML (Géron), Chapters 1–7** — read the relevant chapter after StatQuest to deepen understanding
- **Chip Huyen's ML Interviews Book** (free online) — use the questions for each topic as a self-test *after* practice week
- **3Blue1Brown "Neural Networks" series** — best visual explanation of neural nets and backprop
- **Jay Alammar's "The Illustrated Transformer"** — the gold standard for understanding transformers visually, no math required
- **Andrej Karpathy's "Intro to LLMs" YouTube talk** — one-hour overview that covers everything a DS needs to know
- **Chip Huyen's "Building LLM Applications" blog posts** — practical, DS-oriented perspective on using LLMs
- **Your F1 project** — every topic gets applied here during Sunday flex time

---

## Area 2: Statistics & Experimentation (~20% of time)

**Goal:** Understand the "why" behind statistical methods before memorizing any formulas.

### Learning Sequence

**Topic 1: Probability & Distributions (Weeks 1–3)**
- Theory: What is a distribution and why do we care? Normal, binomial, Poisson — when does each show up in real life? Central limit theorem in plain English.
- Practice: Look at your F1 data. What distributions do race finish times follow? Pit stop durations? Plot them and describe what you see.

**Topic 2: Hypothesis Testing (Weeks 4–6)**
- Theory: What is a hypothesis test really doing? What's a p-value in plain English (not "the probability the null is true")? Type I vs. Type II errors — real-world consequences of each. What is statistical power?
- Practice: Frame a question from your F1 data as a hypothesis test. "Do Mercedes drivers finish higher on average than Ferrari drivers?" Run it, interpret the result, explain what the p-value does and doesn't tell you.

**Topic 3: A/B Testing (Weeks 7–9)**
- Theory: Why do companies A/B test instead of just looking at before/after data? How sample size is determined. What "minimum detectable effect" means. Common pitfalls: peeking, novelty effects, Simpson's paradox.
- Practice: Design an A/B test for a hypothetical F1 scenario (e.g., testing a new tire compound). Calculate sample size. Identify what could go wrong.

**Topic 4: Causal Inference (Weeks 10–14)**
- Theory week 1: Why correlation isn't causation — with real examples. What confounders are. What a DAG is and why it helps.
- Theory week 2: Methods overview in plain language — difference-in-differences, propensity scores, instrumental variables. When you'd use each.
- Practice weeks: Apply one method to your F1 data. "Does starting on soft tires cause better outcomes, or is it confounded by grid position?" Build the analysis step by step.

### Resources
- **Seeing Theory** (brown.edu) — interactive visualizations, perfect for building intuition before formulas
- **Causal Inference for the Brave and True** (free online) — read the explanation sections first, code sections second
- **Trustworthy Online Controlled Experiments (Kohavi)** — chapters 1–6, read after you've covered A/B testing theory
- **Evan Miller's blog** — short, clear posts on sample size and testing

---

## Area 3: Product / Business Case (~30% of time)

**Goal:** Learn to think like a DS who drives business decisions, not just one who builds models.

### Learning Sequence

**Topic 1: Metrics Thinking (Weeks 1–4)**
- Theory: What is a North Star metric? What are guardrail metrics? Why do companies pick the metrics they pick? Read 2–3 company blog posts showing how real DS teams chose their metrics.
- Practice: Pick a product you use daily. Define its North Star metric, 2 guardrails, and explain why. Then do the same for a hypothetical F1 team's analytics dashboard.

**Topic 2: Diagnosing Metric Changes (Weeks 5–8)**
- Theory: Frameworks for "metric X dropped 5%." Decomposition approaches. How to separate real changes from noise, seasonality, and data issues.
- Practice: Work through 1 case per week on Exponent or DataLemur. Use the framework: clarify → decompose → hypothesize → recommend.

**Topic 3: Experiment Design for Products (Weeks 9–12)**
- Theory: How to go from "we want to test this feature" to a full experiment plan. Choosing randomization units. Dealing with network effects. When NOT to A/B test.
- Practice: Design experiments for 2 product scenarios (from Exponent). Present your designs out loud in under 5 minutes.

**Topic 4: Stakeholder Communication (Weeks 13–16)**
- Theory: How to structure a recommendation. How to present bad news from data. How to push back on "the data says what I want it to say" requests.
- Practice: Record yourself presenting an analysis from your F1 project as if to a VP. Watch it back. Refine.

### Resources
- **Exponent** (tryexponent.com) — subscribe for 2–3 months when you reach the practice phases
- **DataLemur** — free product case questions
- **Company blogs** (Airbnb, Netflix, Spotify, Uber) — read during theory phases to see how real teams think
- **Lewis Lin's Decode and Conquer** — metric and framework thinking

---

## Area 4: SQL + Python Coding (~25% of time)

**Goal:** Maintain and sharpen. This is the one area where practice-heavy is fine since you already have the theory.

### Learning Sequence

**Weeks 1–4: SQL Foundations Refresh**
- Theory (light): Review window functions, CTEs, self-joins — watch one Mode Analytics tutorial per week
- Practice: 2 DataLemur SQL problems per Thursday session, easy → medium difficulty

**Weeks 5–8: SQL at Interview Speed**
- Practice: 2 medium DataLemur or StrataScratch problems per session, timed at 15 minutes each

**Weeks 9–12: Python/Pandas**
- Theory (light): Vectorized operations vs. apply, merge types, reshaping patterns
- Practice: 1 Python problem + 1 SQL problem per Thursday session

### Resources
- **DataLemur SQL questions** — sorted by company and difficulty
- **StrataScratch** — real interview questions from target companies
- **Mode Analytics SQL tutorial** — window functions refresher
- **LeetCode Database section** — medium difficulty

---

## Area 5: ML Engineering (Months 7–10)

**Goal:** Build the engineering skills that let you take a model from notebook to production. This section starts after the DS foundation is solid — you're extending your range, not starting over.

> During months 7–10, the weekly schedule shifts: Tuesday and Saturday sessions rotate between MLE topics below. Thursday stays as coding practice but shifts toward DSA problems. Sunday flex continues with F1 project, now focused on engineering improvements.

### Topic 1: Software Engineering Foundations (Weeks 1–4)

**Goal:** Write code that other engineers trust. DS code that "works in a notebook" isn't enough — you need code that's testable, readable, and maintainable.

- Theory week 1: Clean code principles in plain English. What makes code "clean"? Functions that do one thing. Meaningful names. DRY (don't repeat yourself). Why this matters — your model code will be reviewed by software engineers, and messy code kills trust before anyone looks at your model.
- Theory week 2: Design patterns for ML code. Repository pattern for data access. Strategy pattern for swapping models. Configuration management — why hardcoded paths and magic numbers are dangerous. Version control workflows (branching, PRs, code review).
- Practice week 1: Take your F1 project and refactor it. Break the monolithic notebook into modules: data loading, feature engineering, training, evaluation. Add docstrings. Make functions that take arguments instead of relying on global variables.
- Practice week 2: Add tests to your F1 project. Unit test one function (e.g., a feature engineering step). Write an integration test that checks the pipeline runs end-to-end. Use pytest. The goal isn't 100% coverage — it's understanding why testing matters and being able to talk about it.

**Resources:**
- **Clean Code (Robert Martin)** — read chapters 1–6, skip the Java-specific parts; the principles are universal
- **Refactoring Guru** (refactoring.guru) — design patterns explained with visual examples
- **pytest documentation** — simple and practical

### Topic 2: ML System Design (Weeks 5–10)

**Goal:** Be able to whiteboard an end-to-end ML system for any product problem. This is the highest-signal MLE interview topic.

- Theory week 1: The anatomy of an ML system. Every system has the same pieces: data ingestion → feature engineering → training → evaluation → serving → monitoring. Walk through each one. What can go wrong at each stage?
- Theory week 2: Feature stores — what they are and why they exist. Online vs. offline features. Why "training-serving skew" is the silent killer of ML systems. Batch vs. real-time inference — when you'd use each and the tradeoffs.
- Theory week 3: Model serving patterns. REST API vs. batch prediction vs. streaming. Latency vs. throughput tradeoffs. A/B testing in production — how do you safely roll out a new model? Shadow mode, canary deployments, gradual rollouts.
- Practice week 1: Design the system for your F1 race prediction model as if it were a real product. Draw the architecture: where does data come from? How are features computed? How does the model get trained and retrained? How do predictions get served? What gets monitored?
- Practice week 2: Practice 2 system design questions from "Designing Machine Learning Systems" (Chip Huyen). Present each one out loud in 15 minutes — draw it on paper, explain the tradeoffs.
- Practice week 3: Pick a real product (e.g., Uber surge pricing, Netflix recommendations, Spotify Discover Weekly) and reverse-engineer the ML system. What data do they need? What features? What model? How would they serve it?

**Resources:**
- **Designing Machine Learning Systems (Chip Huyen)** — the definitive book for this topic; read cover to cover during these weeks
- **Stanford CS 329S lecture notes** — ML system design, freely available
- **Eugene Yan's blog** — practical posts on ML system design patterns from industry
- **Company engineering blogs** — Uber, Airbnb, DoorDash, LinkedIn all publish detailed ML system write-ups

### Topic 3: MLOps (Weeks 11–14)

**Goal:** Understand the infrastructure that keeps ML systems running. You don't need to be a DevOps expert, but you need to speak the language and use the basic tools.

- Theory week 1: Containers in plain English. What is Docker and why does it exist? (It packages your code + dependencies so it runs the same everywhere.) Images vs. containers. Dockerfiles. Why this solves "works on my machine" forever.
- Theory week 2: Model lifecycle management. Experiment tracking (MLflow, Weights & Biases) — why you need it and what to track. Model registries. CI/CD for ML — what changes vs. traditional software CI/CD? Data versioning (DVC).
- Practice week 1: Dockerize your F1 project. Write a Dockerfile that builds an image, installs dependencies, and runs your training pipeline. Run it. Push it to Docker Hub. This is a tangible portfolio piece.
- Practice week 2: Add experiment tracking to your F1 project using MLflow or W&B. Log hyperparameters, metrics, and model artifacts for 3 different model configurations. Compare them in the tracking UI.

- Theory week 3: Model monitoring in production. What is data drift? Concept drift? How do you detect when a model is degrading? Alerting strategies. Retraining triggers — when do you retrain, and how do you automate it?
- Practice week 3: Design a monitoring plan for your F1 model. What metrics would you track post-deployment? What thresholds would trigger a retrain? Write it up as a one-page doc.

**Resources:**
- **Docker official "Getting Started" guide** — all you need for the basics
- **MLflow documentation** — experiment tracking quickstart
- **Made With ML (madewithml.com)** — free MLOps course that covers the full lifecycle
- **Evidently AI blog** — practical posts on monitoring and drift detection

### Topic 4: Data Structures & Algorithms (Weeks 11–16, overlapping with MLOps on Thursdays)

**Goal:** Get comfortable with Leetcode medium-level problems. MLE coding rounds are harder than DS rounds — you need to know basic DSA, not just SQL and pandas.

- Theory weeks 1–2: Core data structures in plain English. Arrays, hash maps, stacks, queues, trees, graphs. For each one: what is it, when would you use it, what are the time complexity tradeoffs? You don't need to memorize Big-O tables — understand *why* a hash map lookup is fast and a list search is slow.
- Theory weeks 3–4: Core algorithms. Sorting (just know that it exists and costs O(n log n)). Binary search — when and why. BFS/DFS for trees and graphs. Two pointers and sliding window. Dynamic programming concept (break a big problem into overlapping subproblems) — just the idea, not hard DP problems.
- Practice (ongoing, Thursday sessions): 3 Leetcode problems per week, starting easy and moving to medium by week 4. Focus on arrays, hash maps, strings, and trees — these cover 80% of MLE interview coding. Time yourself at 25 minutes per problem. If you can't solve it in 25 minutes, read the solution, understand it, and redo it the next week.

**Resources:**
- **NeetCode 150** (neetcode.io) — curated list, sorted by pattern; do the "easy" tier first for each pattern, then medium
- **Grokking the Coding Interview (Educative)** — organized by pattern (sliding window, two pointers, etc.) which is more useful than random Leetcode grinding
- **Tech Interview Handbook** (techinterviewhandbook.org) — free, concise DSA cheat sheets and study plan

---

## Monthly Milestones

| Month | Milestone |
|-------|-----------|
| 1 | **Theory foundation.** Understand decision trees and random forests conceptually. Can explain gradient boosting to a non-technical person. Probability/distributions theory done. Metrics thinking started. SQL routine established. |
| 2 | **XGBoost deep dive.** Theory and practice complete for gradient boosting. Know the key hyperparameters and what they do. Hypothesis testing theory done. First product cases attempted. |
| 3 | **Regularization + performance interpretation.** Can explain L1/L2 clearly. Can diagnose when metrics disagree (e.g., low R² with high Pearson) and explain what it means. A/B testing theory done. |
| 4 | **Neural nets + transformers theory.** Can explain attention and how LLMs work at a conceptual level. Causal inference started. Product cases timed. SQL at interview speed. |
| 5 | **LLMs in practice + DS integration.** Applied transformers/embeddings to F1 project. Causal inference applied. Full DS mock interview loop. Identify weak spots. |
| 6 | **DS ready. MLE begins.** DS weak spots addressed. Start software engineering foundations. Refactor F1 project into clean, modular code with tests. |
| 7 | **ML system design.** Can whiteboard an end-to-end ML system. Understand feature stores, serving patterns, and training-serving skew. Start Leetcode practice. |
| 8 | **MLOps in practice.** F1 project Dockerized with experiment tracking. Understand model monitoring and drift detection. Leetcode mediums comfortable. |
| 9 | **Full hybrid ready.** Can do a complete MLE mock interview loop (system design + coding + ML theory). Design a monitoring plan. Portfolio (F1 project) shows both DS depth and engineering quality. |
| 10 | **Apply with confidence.** Both DS and MLE weak spots addressed. Start applying to top-choice companies. Can interview for DS, MLE, or hybrid roles depending on the opportunity. |

---

## Rules for Yourself

1. **Theory before practice, always.** If you can't explain it simply, you don't understand it yet. Don't jump to code.
2. **Never study more than 8 hrs/week.** Burnout kills consistency. Consistency beats volume.
3. **Speak answers out loud.** Staff-level interviews are about communication as much as correctness.
4. **Use the F1 project as your lab.** Every ML and stats concept gets tested on real data you care about.
5. **Track what you study.** Simple spreadsheet: date, topic, theory or practice, confidence (1–5). Review monthly.
6. **One concept at a time.** Don't move to the next topic until you can explain the current one from scratch.
7. **The F1 project is your portfolio piece.** By month 10 it should show clean code, tests, Docker, experiment tracking, and thoughtful ML — not just a notebook that runs.
