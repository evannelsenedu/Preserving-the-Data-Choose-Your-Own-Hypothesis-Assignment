# Preserving the Data: Choose Your Own Hypothesis

## 1. Research Question
For my final project, I am analyzing competition results from the Minnesota State Fair's Canned and Preserved Foods division. The primary goal of this research is to investigate community engagement across the state of Minnesota, which aligns with the university's mission to advance the common good. Specifically, I want to know if geographic location or prior competition experience gives entrants a measurable edge in winning.

## 2. Hypothesis
This analysis is driven by two main hypotheses:

**Hypothesis 1 (Geographic Impact)**
* **Null Hypothesis:** Geographic location (Twin Cities Metro vs. Greater Minnesota) has no significant impact on the mean number of awards won by participants.
* **Alternative Hypothesis:** Geographic location significantly impacts the mean number of awards won by participants.

**Hypothesis 2 (The "Veteran" Advantage)**
* **Null Hypothesis:** Repeat winners do not hold a statistically measurable advantage over first-time winners.
* **Alternative Hypothesis:** Repeat winners hold a statistically measurable advantage over first-time winners.

## 3. Data Description
Getting this data into a usable format was a massive headache. I initially tried to manually extract the data from the official competition PDFs, but standard extractors failed due to formatting inconsistencies. To get around this, I used NotebookLM and Gemini to accurately extract and structure all of the archival data from 2019 to 2025. 

* **Source:** Official Minnesota State Fair Canned & Preserved Foods PDFs and the historical ArcaSearch database.
* **Unit of Analysis:** Each observation represents a single placing entry in the competition.
* **Observations & Variables:** 1,664 observations. Key variables include Contestant Name, City, Class Number, Class Name, Year, Placement, Subcategory, and Entries in Class.
* **Transformations:** I created a new `Region` column to categorize cities into "Twin Cities Metro" (Minneapolis and Saint Paul) and "Greater Minnesota" (everything else). I also filtered the data to focus on top placements (1st, 2nd, and 3rd (sometimes more, sometimes less)). There are two notable gaps in the data: 2020 (canceled due to COVID-19) and 2024 (records unavailable online).

## 4. Methods
To analyze the data, I used the following statistical methods:
* **Permutation Testing:** I used a permutation test to compare the mean number of awards between urban (Twin Cities Metro) and rural (Greater Minnesota) participants. The test statistic was the difference in means between the two regions. I simulated the null hypothesis by randomly shuffling the region labels 10,000 times to see how often a difference as extreme as the observed one occurred by chance.
* **Bootstrapping:** I used bootstrapping to build 95% confidence intervals around the proportion of repeat winners. 
* **CLT Exception:** The Central Limit Theorem does not perfectly apply to the proportion of repeat winners because the distribution of repeat wins is heavily skewed by a few extreme outliers (e.g., one contestant taking 55 placements), and the sample violates strict independence since multiple entries can come from the exact same person in a given year.

## 5. Results
Here are the main findings from the analysis:
* **Descriptive Stats:** Out of 1,664 total entries, 1,371 came from Greater Minnesota, while 293 came from the Twin Cities Metro. The average placement was nearly identical (2.50 for Greater MN vs. 2.55 for Twin Cities). Jams were the most popular subcategory overall.
* **Geographic Impact:** The permutation test resulted in a high p-value, failing to reject the null hypothesis. While Greater Minnesota brings in a significantly higher *volume* of total entries, the *mean* number of awards per participant does not statistically differ between the two regions.
* **Veteran Advantage:** The bootstrap confidence interval confirmed a significant advantage for repeat competitors. A core group of "veteran" contestants accounts for a disproportionately large percentage of the total awards.

## 6. Uncertainty Estimation
To quantify uncertainty, I relied on resampling techniques:
* **Resamples:** I utilized 10,000 resamples for both the permutation test and the bootstrap distribution.
* **Distributions:** The randomization distribution for the geographic mean difference was normally distributed and centered around zero. The bootstrap distribution for the repeat winner proportion was slightly right-skewed.
* **Interpretation:** The 95% bootstrap confidence interval indicates we can be 95% confident that the true population proportion of repeat winners falls within our calculated range, meaning seasoned competitors consistently hold a measurable edge.

## 7. Limitations
The biggest limitation of this dataset is survivorship bias: the data *only* includes people who actually placed. Because we have zero data on the rest of the entrants who didn't win an award, we cannot calculate true win rates or control for the total volume of submissions per region. Additionally, the missing years (2020 and 2024) and the manual categorization of "Twin Cities Metro" (strictly limiting it to Minneapolis and St. Paul) may slightly skew the regional representation.

## 8. References
* Minnesota State Fair Archival Records (ArcaSearch)
* Python libraries: Pandas, NumPy, Matplotlib, SciPy, Statsmodels
* NotebookLM / Gemini LLM for PDF data extraction