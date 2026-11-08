# Job Description Keyword Analysis

An exploratory NLP project investigating how job-description language can support resume tailoring. The project applies bag of words (BOW), n-grams, and TF-IDF to Indeed job descriptions, comparing the usefulness of phrases extracted from a single listing and a collection of listings.

## Research question

Can text analysis of Indeed job listing descriptions identify recurring and role-specific skills and phrases useful for resume tailoring?

## Project goals

- Compare unigrams and bigrams as sources of actionable resume language.
- Explore frequency-based extraction and TF-IDF weighting at different document levels.
- Inspect extracted terms in context and document where the approaches succeed or fall short.
- Establish a baseline for future text cleaning and a potential resume-tailoring application.

## Current status

**Initial exploratory experiments complete; further development paused while textbook study continues.**

Two experiments have been completed, with results and interpretations recorded alongside notebook outputs. Bigram frequency analysis produced promising candidate phrases. TF-IDF provided another view of the descriptions but did not consistently improve phrase usefulness. Repeated headers, job details, titles, and generic language remain a major source of noise.

This version is an exploratory analysis, not a deployed application. It does not include live collection or validated ATS screening outcomes.

## Data source

[Indeed USA data jobs — burcuonel/datajob_indeed_usa](https://huggingface.co/datasets/burcuonel/datajob_indeed_usa)

The source contains 1,200 listings with job titles, companies, locations, and description text. The analysis uses **`Descriptions`**, the longer description field, rather than **`Description`**, which contains short listing snippets. Data is loaded through the Hugging Face `datasets` package and converted to a pandas DataFrame.

The collection includes multiple data-related job families and seniority levels. It is a convenience dataset rather than a representative sample of the current job market. Collection dates and cleaning procedures are not well documented. Duplicate or near-duplicate postings may affect aggregate rankings.

## Experiments

### Experiment 1: One job description

**Approach:** Inspect the first description, extract unigram and bigram counts, and explore TF-IDF with sentences or text segments treated as documents. Compare the resulting terms with the original description.

**Findings:** Unigrams provided limited context. Bigrams yielded some more interpretable phrases, but neither frequency analysis nor sentence-level TF-IDF consistently isolated useful resume language. This established an initial baseline and clarified how the choice of document unit changes the interpretation of TF-IDF.

### Experiment 2: Multiple job descriptions

**Approach:** Treat each full description as a document. Rank unigrams and bigrams by aggregate BOW frequency, inspect one description's bigram TF-IDF scores using the full collection as the reference, and assess an overall ranking based on mean bigram TF-IDF across descriptions.

**Findings:**

- Unigrams largely surfaced broad description language.
- Bigram BOW analysis produced more actionable phrases and was the most promising approach in the experiments.
- Single-description TF-IDF showed similar strengths and constraints to bigram BOW, with additional irrelevant terms appearing among the results.
- Mean TF-IDF rankings remained heavily influenced by titles, headers, and broadly redundant descriptors.

Findings are based on qualitative inspection of the outputs. A formal measure of phrase usefulness has not yet been implemented.

## Potential application workflows

The experiments suggest three workflows for further development:

| Workflow | Intended use |
| --- | --- |
| Single-listing bigram frequency | Surface recurring phrases to consider when tailoring a resume to one job. |
| TF-IDF within a collection of similar jobs | Explore distinctive requirements and language in individual listings. |
| Aggregate bigram TF-IDF | Explore reusable phrases across a broader collection of descriptions. |

These are application directions to test after cleaning and evaluation. Suggested phrases would need to match the user's actual experience and qualifications.

## Limitations

- Boilerplate and scraping artifacts have not undergone targeted removal.
- Frequency and distinctiveness do not, by themselves, establish whether a phrase is useful for a resume.
- The dataset mixes roles and seniority levels, which affects collection-wide rankings.
- English stop-word removal can create n-grams whose words were not adjacent in the original text; promising phrases should be checked against source passages.
- No ATS software, recruiter screening criteria, or application outcomes were available for validation.

## Next steps

1. Apply targeted cleaning to redundant headers, wage details, and other irrelevant text while preserving meaningful requirements and phrase boundaries.
2. Compare cleaned outputs with the existing baseline using consistent criteria for useful phrases.
3. Evaluate results on descriptions not used to develop the cleaning rules.
4. Explore filtering by job family or seniority, and later clustering descriptions by their text features.
5. If extraction quality becomes useful and repeatable, investigate live collection and package the analysis into an application.

## Tools

Python, Jupyter notebooks, pandas, NumPy, scikit-learn, Matplotlib, and Hugging Face Datasets. Development uses the Conda environment `job-keywords` and Git for version control.

An environment specification is planned once project dependencies have settled. This version does not yet provide a pinned environment for exact reproduction.