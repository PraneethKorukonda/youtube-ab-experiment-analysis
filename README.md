# YouTube Recommendation Algorithm: A/B Experiment Analysis
**Praneeth Korukonda**

---

## The Use Case

YouTube wants to show users more long-form educational content: tutorials, lectures,
explainers, instead of the short entertainment clips that dominate most feeds today.
The hypothesis is that users who engage with educational content will watch longer, come
back more often, and be more satisfied with the platform over time.

But there's a real tension here. Short-form entertainment creators (people who make
3-minute comedy sketches, reaction videos, quick lifestyle content) make up a huge part
of the YouTube creator ecosystem. If the algorithm tilts too hard toward educational
content, those creators lose impressions, lose ad revenue, and potentially leave the
platform. That's bad for everyone.

So the question isn't just "does this algorithm increase watch time?" It's "does it
increase watch time *without* harming the creator ecosystem?" That's a much harder
problem to answer, and it's exactly the kind of ambiguous product question that a
data scientist on the YouTube team would be expected to structure, test, and resolve.

---

## What I Did

I designed and ran a full end-to-end A/B experiment analysis on this problem:

**Experiment design:** defined the hypothesis, set the randomization unit (user-level),
chose the 30-day window, and specified success criteria before running any tests.

**KPI framework:** identified one primary metric (avg daily watch time per user),
three secondary metrics (session duration, videos per session, week-4 retention), and
two guardrail metrics (short-form and entertainment creator impression share). The
guardrails are what prevent the team from shipping something that looks good on the
primary metric but quietly harms a segment of creators.

**Statistical testing:** ran Welch's two-sample t-test on the primary metric, computed
bootstrap confidence intervals, measured effect size (Cohen's d), and applied CUPED
variance reduction using pre-experiment watch time as a covariate. CUPED is a standard
technique at Google and Netflix for making experiments more statistically sensitive without
increasing sample size.

**Segmentation:** broke down the treatment effect by device type, age group, and account
type to find where the algorithm works best and whether any user segment is being harmed.

**Novelty effect check:** measured the treatment lift week by week to distinguish a
genuine engagement change from a short-term curiosity spike that would fade after launch.

**Root cause decomposition:** split the watch time lift into two components: users
opening the app more often (frequency) vs. staying longer per session (duration). This
tells the engineering team where the gain is coming from and which lever to pull if
they want to amplify it.

**Recommendation:** didn't just report the numbers. Made a call: the primary metric is
positive and significant, but the short-form creator guardrail is violated by 20 percentage
points. Conditional approval: fix the content blend, re-run, then ship.

---

## The Dataset

The dataset is synthetically generated. I built it programmatically using a realistic
user behavior model (log-normal session watch times, per-user engagement heterogeneity,
a churn process that produces realistic 70-78% week-4 retention). Synthetic data is
standard practice when real production data isn't accessible, and it lets me control the
ground truth to validate the analysis is working correctly.

5,000 users, 30-day experiment window, ~55,000 sessions, ~360,000 video interactions.

---

## What's in This Folder

| File | What it is |
|---|---|
| `YouTube_Experiment_Analysis.ipynb` | Full analysis notebook: run it top to bottom to reproduce everything |
| `Executive_Summary.pdf` | Business write-up of the same findings, written for a PM or director |

---

## Stack

Python: pandas, numpy, scipy, matplotlib, seaborn

`pip install numpy pandas scipy matplotlib seaborn nbformat`
