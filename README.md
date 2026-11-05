# Global AI Job Market & Salary Trends (2024–2025)

Comparative exploratory analysis of reported AI salaries, experience requirements, work arrangements, and skill mentions across two datasets.

## Project Overview

This project uses Python to examine how reported AI job salaries vary across roles, experience levels, industries, and work arrangements. It also compares skill mentions, employer locations, and monthly record counts to describe the composition of each sample.

Understanding these relationships can help frame career research and compensation discussions. The findings describe the supplied data, whose provenance and representativeness are unverified; they should not be treated as global hiring estimates or validated salary benchmarks.

The analysis is contained in [scripts.ipynb](scripts.ipynb). Both datasets cover posting dates from **January 1, 2024, through April 30, 2025**, rather than two complete calendar years. They are analyzed separately, with dataset labels preserved in comparative plots and tables.

## Objectives

- Compare salary distributions and identify roles with the highest median salaries.
- Examine salaries by experience level and the association between years of experience and salary.
- Compare onsite, hybrid, and remote salaries, including comparisons within experience levels.
- Explore industry-by-experience salary patterns.
- Identify frequently mentioned skills and the most represented employer locations.
- Compare monthly record volumes and summarize data quality, identifier overlap, and salary outliers.

## Dataset Information

| Attribute | Dataset A | Dataset B |
| --- | --- | --- |
| File | `ai_job_dataset.csv` | `ai_job_dataset1.csv` |
| Records | 15,000 | 15,000 |
| Original columns | 19 | 20 |
| Posting period | 2024-01-01 to 2025-04-30 | 2024-01-01 to 2025-04-30 |
| Role and employer coverage | 20 job titles, 20 company locations, 15 industries | 20 job titles, 20 company locations, 15 industries |
| Important shared features | `job_id`, `job_title`, `salary_usd`, `salary_currency`, `experience_level`, `years_experience`, `employment_type`, `company_location`, `employee_residence`, `company_size`, `required_skills`, `remote_ratio`, `education_required`, `industry`, posting and deadline dates, description length, benefits score, company name | Same shared features |
| Distinguishing features | 3 salary currencies; 20 employee-residence labels | Additional `salary_local` column; 8 salary currencies; 50 employee-residence labels |
| Main limitations | Unverified origin, reused cross-dataset identifiers, uncertain sampling and salary definitions | Same limitations; local-currency conversion methodology is also undocumented |

### Identifier overlap and source status

Each file has 15,000 unique `job_id` values, and **all 15,000 IDs occur in both files**. However, no shared ID has the same salary in both datasets, and none matches simultaneously on job title, salary, and posting date. The notebook uses a one-to-one merge to diagnose this overlap, then analyzes the files as separate samples. It does not join their analytical attributes or claim 30,000 distinct, independently verified job postings.

No original Kaggle URL or reliable dataset-source metadata is present in the supplied project files. Accordingly, no source URL or claim of authentic real-world postings is supplied here. Synthetic generation is possible, but is not established by the available evidence.

All salary comparisons use the supplied `salary_usd` field. The notebook labels it annual salary; the repository does not independently establish its annualization or currency-conversion methodology.

## Tools and Technologies

| Tool | Use in this project |
| --- | --- |
| Python | Data preparation and analysis |
| pandas | CSV loading, validation, grouping, pivot tables, date handling, and skill expansion |
| NumPy | Numerical utilities, median estimation, and salary-band boundaries |
| Matplotlib | Figure construction, axes, labels, and layout |
| seaborn | Distribution, bar, box, heatmap, line, and scatter plots |
| pathlib | Dataset path resolution |
| IPython / Jupyter Notebook | Interactive execution and formatted tables |

## Project Workflow

1. **Load and explore:** Read both CSVs; inspect shapes, sample rows, column types, numerical and categorical summaries, and selected categorical distributions.
2. **Assess data quality:** Check exact duplicates, duplicate IDs within each sample, and cross-dataset ID overlap. Compare selected attributes for shared IDs before deciding how to handle the samples.
3. **Validate missing values and records:** Convert date fields with coercion and selected numeric fields with `to_numeric`. Count missing cells and flag nonpositive or missing salaries, unparseable dates, deadlines before posting dates, negative experience, and remote ratios outside `0`, `50`, and `100`.
4. **Clean and preprocess:** Work on copies, exclude invalid records, and remove exact duplicates. The saved quality table reports **zero missing cells, exact duplicates, duplicate IDs, and invalid rows in each dataset**, retaining all 15,000 rows per sample. No imputation is performed.
5. **Engineer features:** Create `posting_month`, `remote_type`, `experience_label`, `salary_band`, and `application_window_days`. Map experience codes `EN`, `MI`, `SE`, and `EX` to Entry, Mid, Senior, and Executive; map remote ratios to Onsite, Hybrid, and Remote. Salary bands use pandas' default right-closed intervals at $60,000, $100,000, $150,000, and $200,000, so boundary values belong to the lower interval despite shorthand labels such as `<60k`.
6. **Explore and compare:** Calculate group counts, means, medians, experience-level ratios, and industry/experience and experience/work-arrangement pivot tables. Split comma-separated skills, trim whitespace, and count expanded mentions.
7. **Assess relationships and outliers:** Calculate Pearson correlation between years of experience and salary. Flag salary observations outside 1.5 times the interquartile range (IQR); retain them in the analysis. This is descriptive analysis, without hypothesis tests or predictive modeling.
8. **Visualize:** Produce eight figures, retaining dataset distinctions throughout. The experience boxplot hides outlier markers and limits the displayed salary range; this changes presentation, not the underlying analytical sample.
9. **Summarize:** Generate an executive summary for each dataset and a cross-sample median comparison. The notebook contains code cells with saved tables, figures, and printed summaries; it has no Markdown narrative cells.

## Exploratory Data Analysis and Visualizations

| Visualization | Variables and analytical question |
| --- | --- |
| **Salary distributions** | Overlaid density-scaled histograms of `salary_usd`, with median reference lines: how do salary distributions and their centers differ between samples? |
| **Highest-median roles** | Horizontal bars for the top 10 `job_title` groups by median salary, requiring at least 50 records per group: which roles rank highest within each sample? |
| **Salary by experience level** | Boxplots of salary across Entry, Mid, Senior, and Executive levels, separated by dataset: how does compensation vary with seniority? |
| **Salary by work arrangement** | Median-salary bars for Onsite, Hybrid, and Remote categories: is any arrangement consistently associated with higher pay across samples? |
| **Industry and experience salary matrix** | Annotated heatmaps of median salaries for the eight most frequent industries in each sample, split by experience level: how do industry comparisons change across seniority groups? |
| **Most-mentioned skills** | Top 15 expanded skill counts per dataset: which skill labels occur most often in the supplied records? |
| **Monthly record counts** | Lines of record counts by posting month and dataset: how does sample volume vary over the covered period? |
| **Years of experience versus salary** | Scatter plots using up to 2,500 records per sample with `random_state=42`; displayed Pearson correlations use each full dataset: what association exists between experience in years and salary? |

Supporting tables include salary summaries by work arrangement, experience-level median salaries, and work-arrangement medians stratified by experience. Employer-location coverage is examined through categorical summaries and the generated executive summary, rather than a dedicated geographic chart. Salary bands and application-window length are engineered but do not have dedicated visual analyses.

## Key Findings and Insights

Values below reproduce saved notebook results and calculations from the supplied CSVs. Salaries are in USD and rounded to whole dollars as in the notebook; ratios and percentage differences use unrounded values.

1. **Dataset B has a higher salary center.** Median salary is **$99,705 in A** and **$107,262 in B**; means are **$115,349** and **$121,992**. B's unrounded median exceeds A's by $7,556 when rounded, or **7.6%**. This compares two samples covering the same period and does not establish salary growth.
2. **Experience levels show substantial salary differences.** Entry and Executive medians are **$60,374 and $177,512 in A**, versus **$66,462 and $197,354 in B**. Executive-to-entry median ratios are **2.94×** and **2.97×**. These are descriptive group differences, without adjustment for other characteristics.
3. **Data Engineer ranks first by role median in both samples.** Its median is **$104,447 in A** and **$112,505 in B**, under the notebook's minimum-50-record rule. The rankings are not adjusted for experience or location.
4. **Work arrangement has no consistent median-salary leader across samples.** A's medians are Onsite **$98,817**, Hybrid **$99,295**, and Remote **$100,614**. B's are Onsite **$106,506**, Hybrid **$108,528**, and Remote **$107,070**. These results do not establish a causal remote-work premium.
5. **Python is the most-mentioned skill in both samples.** It appears in **4,450 records in A** and **4,499 in B**, followed by SQL at **3,407** and **3,511**. Each dataset contains **24 unique skill labels**. These counts measure sample mentions, not verified market-wide demand or skill-specific salary premiums.
6. **The most represented company locations differ.** Germany leads A with **814 records**; Switzerland leads B with **819**. These counts describe employer-location composition, not national AI market size.
7. **The peak posting months differ.** April 2024 has the highest count in A (**985 records**), while March 2024 leads B (**1,021**). Sample peaks do not by themselves establish seasonality or changes in overall hiring demand.
8. **High-salary observations are retained.** The IQR rule flags **483 records (3.2%) in A** and **409 (2.7%) in B**. Keeping these observations avoids automatically treating high reported compensation as erroneous; medians provide a less extreme-value-sensitive comparison.

## Executive Summary

The two samples show similar broad patterns: salary medians increase across experience levels, Python leads recorded skill mentions, and Data Engineer has the highest role median under the notebook's sample-size threshold. Dataset B reports higher mean and median salaries, while the arrangement with the highest median differs between samples.

Experience-stratified comparisons provide useful context for compensation research, and skill counts indicate subjects worth exploring further. However, unverified provenance, reused identifiers, and unknown sampling prevent these findings from supporting population estimates, causal pay claims, or claims of salary growth. The project's strongest contribution is a transparent, reproducible comparison of the supplied samples.

## How to Run the Project

Install Python 3 with `venv` support and Git. The repository does not include a dependency lockfile or a tested version matrix.

1. Clone the repository and enter its root:

   ```bash
   git clone https://github.com/rehan-mehmood-tech/Global-AI-Job-Market-Salary-Trends-2024-2025-Data-Analysis.git
   cd Global-AI-Job-Market-Salary-Trends-2024-2025-Data-Analysis
   ```

2. Create a virtual environment:

   ```bash
   python -m venv .venv
   ```

   Activate it in Windows PowerShell:

   ```powershell
   .\.venv\Scripts\Activate.ps1
   ```

   Or on macOS/Linux:

   ```bash
   source .venv/bin/activate
   ```

3. Install the notebook libraries and Jupyter:

   ```bash
   python -m pip install pandas numpy matplotlib "seaborn>=0.12" notebook ipykernel
   ```

   The seaborn lower bound supports the notebook's `errorbar=None` argument. Other dependency versions are not pinned by the project.

4. Keep `scripts.ipynb`, `ai_job_dataset.csv`, and `ai_job_dataset1.csv` together in the repository root, then launch Jupyter from that directory:

   ```bash
   python -m notebook scripts.ipynb
   ```

5. Select the Python kernel associated with the environment, then restart the kernel and run all cells from top to bottom. Later cells depend on objects created earlier. Figures and tables display inline; the notebook does not export charts or cleaned datasets.

The loading cell first checks the current working directory for Dataset A and falls back to `/mnt/data` if it is absent. Keeping both CSVs in the launch directory avoids relying on that environment-specific fallback. Saved outputs include a pandas warning about categorical string selection; dependency changes may affect warnings or rendering. The first cell has saved output despite a null execution count, so a fresh full run is the appropriate reproducibility check.

## Project Structure

```text
.
├── .gitattributes
├── README.md
├── ai_job_dataset.csv
├── ai_job_dataset1.csv
└── scripts.ipynb
```

Git internals are omitted. Charts and executive summaries are embedded in the notebook; there are no separate chart exports, dependency files, or supporting report directories in the supplied project.

## Limitations

- **Unverified provenance:** Collection methods, source authenticity, sampling frame, and original licensing metadata are not documented. Possible synthetic data should be considered without asserting it as fact.
- **Overlapping identifiers:** Shared IDs refer to differing records across files. Dataset-qualified labels preserve sample identity, but distinct real-world posting counts cannot be established.
- **Limited representativeness:** Coverage of selected locations, roles, and industries does not establish representative global AI employment data. Repeated company labels do not verify individual employers.
- **Incomplete time coverage:** The data ends in April 2025. Monthly counts are sample counts, with no external hiring baseline or formal seasonal analysis.
- **Salary comparability:** Annualization, exchange-rate dates, and conversion methods are unverified. Analysis uses supplied USD values without purchasing-power or cost-of-living adjustment, and comparisons do not normalize salaries by employment type.
- **Descriptive associations:** Role, industry, experience, and work-arrangement differences are not causal effects. There is no multivariable adjustment, inferential testing, or forecasting.
- **Bounded quality checks:** Passing implemented checks does not validate source authenticity or every possible field constraint. Complete data and zero exact duplicates do not prove accuracy.
- **Reproducibility environment:** Dependencies are unpinned, and notebook execution metadata is inconsistent for the first cell. Saved outputs support inspection but do not replace a fresh run in the chosen environment.

## Conclusion

This project demonstrates a reproducible exploratory workflow: inspect source data, validate records, investigate overlapping identifiers, engineer features, and compare distributions and grouped summaries. Its findings provide a useful starting point for investigating AI compensation and skill patterns, with explicit limits on what these two unverified samples can establish.
