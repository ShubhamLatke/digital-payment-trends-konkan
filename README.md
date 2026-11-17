Digital Payment Trends in Semi-Urban and Rural Markets

A survey-based data analysis of 101 respondents in the Konkan region of Maharashtra (September 2026)

B.Sc. Data Science Field Project, Semester III (2026-2027) Department of Data Science, D.B.J. College (Autonomous), Chiplun, affiliated to the University of Mumbai

Author: Shubham Latke | LinkedIn | GitHub

Objective

Headline statistics on digital payments are usually national or urban in focus. This project collects ground-level evidence on how residents and shopkeepers in semi-urban and rural Konkan markets actually pay, and what holds them back. It aims to:

Profile respondents and assess digital readiness (smartphone and bank account ownership).
Measure awareness of payment methods and how people first learned about them.
Compare payment method, frequency of use and digital share of transactions between semi-urban and rural respondents.
Examine how adoption varies with age, income, occupation, gender and education.
Identify the main barriers to adoption and perceptions of safety.
Gauge future intent and merchant acceptance, and recommend practical actions.
Methodology
Item	Detail
Design	Cross-sectional quantitative survey with a convenience sample
Sample	101 respondents (57 semi-urban, 44 rural) from 32 village and town labels; Chiplun is the largest cluster (30)
Instrument	Online questionnaire, 23 questions in 7 blocks (demographics, digital readiness, awareness, usage, barriers, suggestions, merchant module)
Collection period	4 to 30 September 2026
Field validation	In-person visits in Malghar, Guhagar and Chiplun in the first week of September 2026
Tools	Microsoft Excel, Python (pandas, SciPy, Matplotlib)
How the data was analysed
Cleaning. The raw export (101 responses, 26 named fields plus 2 empty columns) was reduced to a 101-row, 19-column analysis table. Answer labels were standardised (for example "Rural (Village)" became "Rural"), multi-select answers were kept as semicolon-separated lists, and missing values were left blank instead of being imputed.
Derived measure. A respondent is digital-leaning if their primary payment method is anything other than cash alone (UPI, mobile wallet, or cash plus digital). Overall, 59 of 101 (58.4%) are digital-leaning.
Descriptive statistics and cross-tabulations. Percentages are calculated within the group being compared. Multi-select questions (awareness, purposes, barriers) add up to more than 100%.
Significance tests. Chi-square test of independence and Fisher's exact test, with p < 0.05 treated as significant.
Validation. All summary counts were recomputed from the clean table and compared with the workbook summary.
Key findings
Readiness is not the problem. 97.0% own a smartphone and 98.0% have a bank account. UPI is known to 92.1%.
A clear urban-rural divide exists. UPI is the primary method for 52.6% of semi-urban respondents but only 13.6% of rural respondents, while cash is primary for 54.5% of rural respondents (chi-square(3) = 16.94, p < 0.001, Cramer's V = 0.41).
Digital payments supplement cash rather than replace it. 51.5% use them daily, but only 14.9% say more than half of their transactions are digital.
Adoption rises with income and falls with age. Digital-leaning share goes from 25.0% (below Rs. 10,000 a month) to 83.3% (above Rs. 50,000), and from 69.0% (age 18 to 25) to 33.3% (above 50).
Barriers differ by location. Poor network connectivity is the top barrier overall (31.7%). Fear of fraud is higher in semi-urban areas (17.5% vs 6.8%), while preference for cash (20.5% vs 5.3%) and lack of know-how (11.4% vs 1.8%) are concentrated in villages.
Willingness and supply are high. 91.1% want to use digital payments more, and 93.5% of the 77 respondents who answered the merchant question accept UPI or cards at their shop.

Conclusion: the gap in these markets is one of connectivity, confidence and skills rather than awareness or access. The report proposes six recommendations for government, banks, fintech firms, telecom operators and local merchants.

Repository structure
digital-payment-trends-rural-semiurban/
├── README.md
├── report/
│   └── Digital_Payment_Trends_Report.pdf
├── data/
│   └── Digital_Payment_Trends_Anonymised.xlsx
├── notebooks/
│   └── analysis.ipynb          (optional: your pandas / SciPy code)
├── figures/                    (optional: exported charts)
├── requirements.txt
└── .gitignore
How to view the PDF report
On GitHub: open report/Digital_Payment_Trends_Report.pdf and click it. GitHub shows an inline preview. If the preview does not load, click Download raw file (the download icon at the top right of the file view).
Locally: clone the repository or download the file, then open it in any PDF reader.
bash
git clone https://github.com/ShubhamLatke/digital-payment-trends-rural-semiurban.git
cd digital-payment-trends-rural-semiurban/report

The report is organised as: Introduction, Methodology, Respondent Profile, Awareness, Payment Behaviour, Drivers of Adoption, Barriers and Trust, Future Intent, Key Insights, Recommendations, Limitations, Conclusion, and appendices (data dictionary and data-quality notes).

How to analyse the Excel dataset

The workbook has four sheets:

Sheet	Contents
Row Data	Raw survey export (101 responses, one column per question)
Clean Data	Cleaned analysis table (101 rows, 19 columns), use this one for analysis
Summary	Summary tables built from the clean data
Charts	Charts used in the report

Option 1: Excel. Open the workbook, go to Clean Data, and use Insert > PivotTable (for example Location Type in rows, Primary Payment Method in columns, count of rows as values) to reproduce the cross-tabulations.

Option 2: Python.

bash
pip install -r requirements.txt
python
import pandas as pd
from scipy import stats

df = pd.read_excel("data/Digital_Payment_Trends_Anonymised.xlsx", sheet_name="Clean Data")

# Location vs primary payment method
ct = pd.crosstab(df["Location Type"], df["Primary Payment Method"])
chi2, p, dof, _ = stats.chi2_contingency(ct)
print(ct, f"\nchi2({dof}) = {chi2:.2f}, p = {p:.4f}")

# Digital-leaning = primary method is not cash alone
df["Digital Leaning"] = df["Primary Payment Method"] != "Cash"
tab = pd.crosstab(df["Location Type"], df["Digital Leaning"])
odds, p = stats.fisher_exact(tab.values)
print(f"Fisher's exact: odds ratio = {odds:.2f}, p = {p:.3f}")

# Multi-select barriers are semicolon-separated: split before counting
barriers = df["Barrier Reason"].dropna().str.split(";").explode().str.strip()
print(barriers.value_counts())

Suggested requirements.txt:

pandas
scipy
matplotlib
openpyxl
jupyter
Data notes and privacy
Anonymised data only. The original survey workbook contained respondent names and shop names. Those columns (Full Name, Shop Name) must be removed from any version published here, because respondent names are not reported in the study.
Known workbook error. The original Summary sheet shows 205.7% for "% of shopkeepers accepting" because 72 is divided by 35 (shopkeeper count). The correct figure is 72 of 77 merchant-question answers (93.5%).
Not analysed. The Shop Digital Share column was recorded in inconsistent free-text formats (for example "40vs60", "50-50") and was excluded.
Missing income. Four respondents chose "Prefer not to say" and one left income blank; they are excluded from income analysis.
Place names contain spelling variants (for example Kamathe / Kamthe), so town-level analysis was not attempted. The analysis uses Location Type.
Limitations
Non-random convenience sample of 101 respondents: indicative, not population-level estimates.
The sample is 81.2% male, 41.6% aged 18 to 25, and concentrated in Chiplun. Several subgroups have fewer than 10 respondents.
Self-reported behaviour may carry recall and social-desirability bias.
"Digital-leaning" is a proxy based on primary method, not on transaction value or volume.
Findings are associations only; age, income, education and location overlap.
Future scope
Larger, randomly selected sample across more districts, with better representation of women and older residents.
Logistic regression to separate the effects of location, income, age and education.
Redesigned merchant module with structured numeric fields.
Systematic text analysis of open-ended comments.
Repeat survey after interventions such as literacy camps.
Acknowledgement

Guided by Prof. Rupali Yogesh Agavekar, Department of Data Science, D.B.J. College (Autonomous), Chiplun. Thanks to all survey respondents in Malghar, Guhagar, Chiplun and surrounding areas.

License

Code in this repository is released under the MIT License. The report and dataset are shared for educational purposes; please credit the author if you reuse them.
