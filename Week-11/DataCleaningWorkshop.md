# Week 11 — Cleaning and Enriching Your Dataset in OpenRefine

## CV-Worthy Skills Gained This Week
- **Wikidata** Literacy: Understanding how to navigate and interpret structured linked data.
- **Data Cleaning (Beginner Level):** Identifying and correcting inconsistencies, duplicates, and formatting issues in tabular data.  
- **OpenRefine Competency:** Using one of the most widely recognized tools in digital scholarship for data wrangling.  
- **Data Structuring:** Reformatting raw CSV into structured, analysis-ready datasets.  
- **Data Enrichment:** Reconciling entities with Wikidata (QIDs, standardized names) and manually filling missing values.  
- **Critical Data Awareness:** Understanding the difference between “raw” vs. “clean” data and why it matters for research.  

---
# 1. What is Wikidata?

Before we touch any buttons, we need to know what **Wikidata** is and why we’re using it.
[Wikidata](https://www.wikidata.org/wiki/Wikidata:Main_Page) is a structured database created by the Wikimedia Foundation (the same organization that runs Wikipedia). While Wikipedia articles are written for humans to read, **Wikidata** stores facts in a way that computers can understand and **query**. Each “thing” in **Wikidata** is called an item. For example:

- [William Shakespeare](https://www.wikidata.org/wiki/Q692) himself is stored as Q692. That means his “QID” (unique identifier) is Q692.

- His play Hamlet has its own QID (Q41567).

- A French **translation** of Hamlet also has its own QID.

Instead of long names, **Wikidata** uses these Q-numbers to make sure each entity is unique and unambiguous.

Now, facts in **Wikidata** are stored as triples:

- A **Subject** (e.g., Hamlet in French),

- A **Property** (e.g., “**translation** of”),

- An **Object** (e.g., Hamlet).

This is like a mini-sentence: “Hamlet in French” is a “**translation** of” “Hamlet.”

## Why is this powerful?
Because with these triples, we can build chains of knowledge: Shakespeare wrote Hamlet → Hamlet was translated into French → that French edition was published in Paris in 1870 → coordinates for Paris are (48.8566, 2.3522).

And suddenly, we have **author** + **work** + **translation** + date + place — the ingredients for our time-and-space visualization.

Read more about linked data [here](https://www.wikidata.org/wiki/Help:About_data).

## Hands-on One: Exploring Wikidata

Goal: Learn how to browse **Wikidata**, find items (QIDs), and properties (PIDs).



### Step 1. Go to Wikidata

👉 Visit: [https://www.**wikidata**.org/](https://www.**wikidata**.org/)



###  Step 2. Find Shakespeare

In the search bar (top right), type William Shakespeare.

Click the result.

Look at the top left: you’ll see Q692 — this is Shakespeare’s QID.

💡 Discussion: What else can you see on this page? (Date of birth, place of birth, notable works, etc.)


###  Step 3. Explore a property

Scroll down Shakespeare’s page and look at one fact, for example:

“Place of birth” → Stratford-upon-Avon

Click “place of birth.” You’ll go to the **property** page P19.

💡 Note: Properties always start with P (P19, P50, P800). Items always start with Q (Q692, Q30).


###  Step 4. Follow a link

Click on Stratford-upon-Avon (the place).
Notice: it also has a QID (Q31031).

💡 Point out:

- Items link to other items, like a web of knowledge.

That’s how we can travel: Shakespeare → place of birth → Stratford → coordinates.


###  Step 5. Student Task

Pick a famous person you know (a writer, singer, politician, scientist). Search for them in **Wikidata**. Write down their QID and one interesting **property** you find about them (e.g., place of birth, notable **work**, occupation).

➡️ Example answers:

Albert Einstein → Q937, **property** P27 (country of citizenship) = Switzerland.

Beyoncé → Q36180, **property** P106 (occupation) = singer, actor.

# 2. What is Data Cleaning and Structuring?

**Data Cleaning** = making messy data usable.  
Example: You have “Paris,” “paris,” “París.” Cleaning means making them all “Paris.”  

Also includes removing duplicates, fixing missing values, standardizing formats (e.g., all dates in `YYYY-MM-DD`).

**Data Structuring** = organizing data into a consistent shape so tools can use it.  
Example: putting authors in one column, works in another, dates in another.

A well-structured dataset is like a tidy closet: you can actually find your clothes!

Why we care: raw Wikidata exports are powerful but messy. Cleaning and structuring makes them map-ready.

---

## What is OpenRefine?

OpenRefine is a free, open-source tool (formerly Google Refine) for cleaning messy data.

Think of it like Excel on steroids — but instead of just storing data, it helps you:
- Spot errors (typos, duplicates).
- Standardize inconsistent values.
- Reconcile your data with external databases (like Wikidata).

🌐 **Website:** [https://openrefine.org/](https://openrefine.org/)

We use it here because our raw CSV from Wikidata might have missing dates, inconsistent languages, or duplicate places. OpenRefine helps us tidy this before building maps.

---

## Download & Install OpenRefine

1. Go to [https://openrefine.org/download](https://openrefine.org/download)  
2. Choose your operating system (Windows, macOS, Linux).  
3. Install and launch OpenRefine:
   - Run `openrefine.exe` (Windows) or `OpenRefine.app` (Mac).  
   - It opens in your browser at `http://127.0.0.1:3333`.

> Note: It looks like a website, but it’s running locally on your computer.

---

## Creating Your First Project

Before working with our big Shakespeare dataset, let’s warm up on a very small, safe dataset: a simple CSV file with messy author names.  

Download the sample dataset file from BrightSpace-->Week Eleven

### Step 1 — Import the Sample CSV
1. Open OpenRefine → **Create Project → Choose Files** → select `authors_sample.csv`.  
2. Click **Next → Create Project**.  
3. You’ll now see a grid of data similar to Excel.

**Interface Guide**
- Rows = one record  
- Columns = properties (e.g., author, workLabel, relation, etc.)  
- Facets = filters on the left  
- Cluster & Edit = merge near-duplicates  
- Undo/Redo = go back anytime  

---

### Step 2 — Use a Facet to See What’s Inside

**Faceting** = grouping values to see patterns.

##Hands-on One
- Click dropdown on column `relation` → **Facet → Text facet**.  
- On the left, you’ll see counts for each value.  
- Which relation is most common?

Repeat on the `author` column to see spelling variations.

---

### Step 3 — Cluster to Merge Similar Entries

**Clustering** automatically finds values that are “probably the same thing.”

**Hands-on Practice**
- On the `author` column → **Edit cells → Cluster and edit**.  
- Try the default *key collision* method.  
- Merge similar entries like:
  - `William shakespeare`, `Shakespear, W.`, `Shakspeare` → `William Shakespeare`
  - `Tolstoi, Leo`, `Lev Tolstoy`, `tolstoy` → `Leo Tolstoy`

---

### Step 4 — Transforming Data

**Goal:** Standardize date formats (e.g., all `YYYY-MM-DD`).

**Hands-on Practice**
- On the `date` column → **Edit cells → Common transforms → To date**.  
- OpenRefine converts recognized values; blanks remain if invalid.

---

### Step 5 — Export the Cleaned Dataset

- Click **Export → Comma-Separated Values (CSV)**.  
- Save as `authors_sample_clean.csv`.
- Upload the cleaned dataset (indivisually) in your class practice folder on GtiHub

✅ Compare with your original: notice fewer duplicates, standardized names, and richer info.

---

## Why This Matters

By the end:
- Dates are standardized.
- Places are deduplicated.
- Missing coordinates are filled.

---

## What’s Next: Taking OpenRefine Skills Further

- Learn **GREL (General Refine Expression Language)** for scripted transformations.  
- Merge data automatically via reconciliation APIs.  

---

## Assignment for Week 12

**Title:** Cleaning Your Final Project Dataset  

- Import your final project dataset (the ones you uploaded on GitHub last week) into OpenRefine.  
- Clean at least two columns (if it is not clean).  
- Export as `Group-1-cleandataset.csv`.
- Upload it to your Group folder (one submission per group)

✍️ **Additional Step:** Fill missing data manually (from Wikipedia, WorldCat, or library catalogs).  
Keep notes of sources.

---

## OpenRefine Cheat Sheet 📝

| Step | Action |
|------|---------|
| **Import** | Create Project → Choose File → Upload CSV → Create |
| **Facet** | Column dropdown → Facet → Text Facet |
| **Cluster** | Column dropdown → Edit cells → Cluster and edit |
| **Transform** | Column dropdown → Edit cells → Common transforms → To date |
| **Export** | Top right → Export → CSV |
