# Adverse Childhood Experiences and Cardiovascular Disease Risk Analysis

## Project Description

This repository contains a comprehensive analysis examining the relationship between Adverse Childhood Experiences (ACEs) and cardiovascular disease (CVD) risk using the 2024 Behavioral Risk Factor Surveillance System (BRFSS) data. The project employs complex survey design methodology to investigate whether childhood adversity is associated with increased odds of cardiovascular disease in adulthood, while controlling for sociodemographic factors, health behaviors, and comorbidities.

The analysis demonstrates a dose-response relationship between ACE exposure and CVD prevalence, with particular attention to proper handling of survey weights, stratification, and clustering inherent in the BRFSS data structure.

## Files Included

### Data Files
- `USCODE24_LLCP_082125.HTML`: BRFSS 2024 codebook documentation
- `ComplexSamplingWeightsandPreparingModuleDataforAnalysis2024508.pdf`: CDC guide for complex survey analysis

### Code Files
- `ADAProject.Rmd`: Complete R Markdown analysis script containing:
  - Data import and cleaning procedures
  - Variable recoding and categorization
  - Complex survey design specification
  - Descriptive statistics generation
  - Multivariable logistic regression models
  - Data visualization creation

### Analysis Outputs

#### Tables
- `Table1_Descriptive_Statistics1.docx`: Descriptive characteristics of the study sample stratified by ACE exposure category, including weighted percentages and confidence intervals
- `Table2_Regression_Results1.html`: Complete regression results from three models:
  - Model 1: Unadjusted association between ACEs and CVD
  - Model 2: Adjusted for sociodemographic factors (age, sex, race/ethnicity, education, income)
  - Model 3: Fully adjusted model including health behaviors and comorbidities

#### Data Tables
- `Regression_Results_ACE_CVD1.csv`: Extracted odds ratios, confidence intervals, and p-values from all three regression models in CSV format for additional analysis or presentation

#### Figures
- `Figure1_CVD_Prevalence_by_ACE.png`: Bar chart displaying weighted prevalence of cardiovascular disease across ACE categories (None, Low, Medium, High) with 95% confidence intervals
- `Figure2_Forest_Plot_ORs.png`: Forest plot visualizing adjusted odds ratios and 95% confidence intervals from the fully adjusted model (Model 3), demonstrating the dose-response relationship

### Supporting Documents
- `Copy_of_ADA_Concept_Proposal_Revision_3.pdf`: Original research proposal outlining study aims, hypotheses, and analytical approach
- `README.md`: This file

## What the Code Does

### 1. Data Preparation
- Loads 2024 BRFSS data from XPT format
- Filters to 11 states that administered the ACE module (Arizona, Florida, Georgia, Hawaii, Iowa, Maine, Nevada, North Dakota, Ohio, Oklahoma, Virginia)
- Verifies presence of required variables for ACE assessment and CVD outcomes
- Documents initial sample size and state-level distributions

### 2. Variable Creation and Recoding

#### ACE Variables
Creates 11 binary indicators for adverse childhood experiences:
- Parental depression (ACEDEPRS)
- Parental substance use: alcohol (ACEDRINK), drugs (ACEDRUGS)
- Parental incarceration (ACEPRISN)
- Parental divorce/separation (ACEDIVRC)
- Domestic violence exposure (ACEPUNCH, ACEHURT1)
- Emotional abuse (ACESWEAR, ACETTHEM)
- Sexual abuse (ACETOUCH, ACEHVSEX)

Calculates total ACE score (0-11) and categorizes into four groups:
- None (0 ACEs)
- Low (1 ACE)
- Medium (2-3 ACEs)
- High (≥4 ACEs)

#### CVD Outcome
Creates composite cardiovascular disease variable from three components:
- Myocardial infarction (CVDINFR4)
- Coronary heart disease (CVDCRHD4)
- Stroke (CVDSTRK3)

Coded as 1 if respondent reports any of these conditions, 0 if none reported

#### Covariates
- **Demographics**: Age group, sex, race/ethnicity
- **Socioeconomic status**: Education level, household income
- **Health behaviors**: Smoking status, heavy drinking, physical activity, BMI category
- **Comorbidities**: General health status, diabetes diagnosis

### 3. Sample Restrictions
Applies sequential exclusion criteria:
1. Requires valid ACE module responses
2. Requires valid CVD outcome data
3. Requires complete data on all covariates (complete case analysis)

Documents sample size at each step to ensure transparency

### 4. Complex Survey Design Setup
Properly specifies BRFSS complex survey design:
- **Primary sampling units (PSU)**: `_PSU` variable
- **Strata**: `_STSTR` variable
- **Survey weights**: `_LLCPWT` variable
- Uses `srvyr` package for tidy survey analysis workflow

### 5. Descriptive Analysis
Generates weighted descriptive statistics:
- Overall sample characteristics
- Characteristics stratified by ACE category
- CVD prevalence by ACE exposure level
- Tests for differences across ACE categories using appropriate survey-adjusted tests

### 6. Regression Analysis
Fits three sequential logistic regression models using survey design:

**Model 1 (Unadjusted)**
- Outcome: CVD (yes/no)
- Predictor: ACE category

**Model 2 (Sociodemographic Adjustment)**
- Adds: age, sex, race/ethnicity, education, income

**Model 3 (Fully Adjusted)**
- Adds: smoking, heavy drinking, physical activity, BMI, general health, diabetes

Tests for linear trend across ACE categories using ordinal coding

### 7. Visualization
Creates publication-ready figures:
- Bar chart with error bars showing CVD prevalence by ACE group
- Forest plot displaying adjusted odds ratios with confidence intervals
- Saves high-resolution PNG files (300 DPI) suitable for publication

### 8. Results Export
Exports results in multiple formats:
- Word document (Table 1) for easy editing
- HTML table (Table 2) for web viewing
- CSV file for data sharing and further analysis
- PNG figures for presentations and manuscripts

## Key Findings

### Sample Characteristics
- **Final analytic sample**: Weighted N = 14,016,546 adults from 11 U.S. states
- **ACE distribution**: 31.5% reported no ACEs, 68.5% reported at least one ACE
  - None (0 ACEs): 31.5%
  - Low (1 ACE): 22.4%
  - Medium (2-3 ACEs): 23.1%
  - High (≥4 ACEs): 23.1%
- **CVD prevalence**: Overall weighted prevalence of 10.3%

### Main Results

1. **CVD Prevalence by ACE Exposure**: CVD prevalence varies by ACE category
   - None (0 ACEs): 10.2%
   - Low (1 ACE): 9.8%
   - Medium (2-3 ACEs): 10.2%
   - High (≥4 ACEs): 10.8%

2. **Adjusted Associations** (Model 3 - Fully Adjusted):
   - Low ACEs (1): OR = 1.07 (95% CI: 0.83-1.37), p = 0.600
   - Medium ACEs (2-3): OR = 1.17 (95% CI: 0.92-1.50), p = 0.197
   - High ACEs (≥4): OR = 1.47 (95% CI: 1.13-1.91), p = 0.004
   
3. **Progressive Relationship Across Models**:
   - Model 1 (Unadjusted): High ACEs OR = 1.06 (0.85-1.32), p = 0.610
   - Model 2 (Demographics + SES): High ACEs OR = 1.75 (1.37-2.23), p < 0.001
   - Model 3 (Fully Adjusted): High ACEs OR = 1.47 (1.13-1.91), p = 0.004
   
4. **Population Impact**: Adults with high ACE exposure (≥4 ACEs) have 47% higher odds of CVD compared to those with no ACE exposure, even after adjusting for sociodemographic factors, health behaviors, and comorbidities. The medium ACE category shows a 17% increased odds (though not statistically significant), suggesting a dose-response pattern

## Methods Summary

### Study Design
Cross-sectional analysis using 2024 BRFSS data from states participating in the ACE module

### Statistical Approach
- Complex survey design methodology accounting for BRFSS sampling structure
- Weighted descriptive statistics with 95% confidence intervals
- Survey-weighted logistic regression for adjusted analyses
- Sequential model building to examine confounding
- Trend testing for dose-response assessment

### Software
- **R version**: 4.x or higher
- **Key packages**:
  - `tidyverse`: Data manipulation and visualization
  - `haven`: Reading SAS/SPSS/Stata files
  - `survey`: Complex survey analysis
  - `srvyr`: Tidy interface to survey package
  - `gtsummary`: Formatted regression tables
  - `flextable`: Formatted descriptive tables
  - `ggplot2`: Data visualization

## How to Run the Code

### Prerequisites
1. Install R (version 4.0 or higher recommended)
2. Install RStudio (optional but recommended)
3. Install required R packages (see code chunk 1 in `ADAProject.Rmd`)

### Steps
1. **Download the BRFSS 2024 data**:
   - Visit the CDC BRFSS website
   - Download the `LLCP2024.XPT` file
   - Place in your working directory

2. **Set up your environment**:
   ```r
   # Check working directory
   getwd()
   
   # Set to location of your data file
   setwd("path/to/your/data")
   ```

3. **Open the analysis file**:
   - Open `ADAProject.Rmd` in RStudio

4. **Update file paths**:
   - Modify file paths in the code to match your directory structure
   - Lines to update are marked with `setwd()` or file paths containing `/Users/sambattaglia/Downloads/`

5. **Run the analysis**:
   - Run chunks sequentially OR
   - Click "Knit" to generate full HTML output
   - Code is organized into logical sections with clear headers

6. **View outputs**:
   - Tables will be saved to your specified output directory
   - Figures will be saved as PNG files
   - Check console for sample sizes and model results

### Troubleshooting
- **Missing variables**: Verify you're using 2024 BRFSS data and have the correct XPT file
- **Survey design errors**: Ensure variables `_PSU`, `_STSTR`, and `_LLCPWT` are present
- **Memory issues**: BRFSS data is large; ensure adequate RAM (8GB+ recommended)
- **Package errors**: Run `install.packages()` for any missing dependencies

## Reproducibility Notes

### Random Seed
Not applicable - analysis uses complete survey data without random sampling

### Data Availability
- BRFSS data is publicly available from the CDC
- No individual-level data are included in this repository due to file size
- Data can be downloaded from: https://www.cdc.gov/brfss/annual_data/annual_2024.html

### Version Information
- Analysis conducted in 2025
- BRFSS 2024 data released August 2025
- Code developed using R 4.x

### Ethical Considerations
- BRFSS data is de-identified and publicly available
- No IRB approval required for secondary data analysis of public datasets
- Analysis follows CDC guidelines for complex survey data analysis

## Study Limitations

1. **Cross-sectional design**: Cannot establish temporal sequence or causation
2. **Self-reported data**: Subject to recall bias, especially for childhood experiences
3. **Selection bias**: Limited to 11 states with ACE module; may not generalize to all U.S. adults
4. **Healthy survivor bias**: Adults with severe childhood adversity may not survive to participate
5. **Unmeasured confounding**: Cannot control for all potential confounders (e.g., genetics, adult trauma)
6. **Response bias**: Survey response rates vary by state and demographic characteristics

## Future Directions

1. Examine specific CVD subtypes separately (MI, CHD, stroke)
2. Test for effect modification by sex, race/ethnicity, or age
3. Explore mediating pathways (e.g., mental health, inflammation)
4. Compare findings across multiple BRFSS years to assess temporal trends
5. Conduct sensitivity analyses excluding specific ACE types to identify most impactful exposures

## References

1. Behavioral Risk Factor Surveillance System (BRFSS). Centers for Disease Control and Prevention. https://www.cdc.gov/brfss/

2. Felitti VJ, Anda RF, Nordenberg D, et al. Relationship of childhood abuse and household dysfunction to many of the leading causes of death in adults: The Adverse Childhood Experiences (ACE) Study. Am J Prev Med. 1998;14(4):245-258.

3. Su S, Jimenez MP, Roberts CT, Loucks EB. The role of adverse childhood experiences in cardiovascular disease risk: a review with emphasis on plausible mechanisms. Curr Cardiol Rep. 2015;17(10):88.

4. CDC. Complex Sampling Weights and Preparing 2024 BRFSS Module Data for Analysis. August 2025.

## Author

- **Name**: Sam Battaglia
- **Course**: Advanced Data Analysis (ADA)
- **Institution**: [Your Institution Name]
- **Date**: November 2025
- **Contact**: [Your Email] (optional)

## Acknowledgments

- CDC BRFSS program for data collection and public data access
- Survey respondents who shared sensitive personal information
- Course instructors and teaching assistants for guidance on complex survey methods

## License

This analysis code is provided for educational purposes. BRFSS data usage must comply with CDC data use restrictions. Please cite this repository if you use or adapt this code for your own research.

---

**Repository Structure:**
```
.
├── README.md                                          # This file
├── ADAProject.Rmd                                     # Main analysis code
├── USCODE24_LLCP_082125.HTML                         # BRFSS codebook
├── ComplexSamplingWeightsandPreparingModuleData...   # CDC methods guide
├── Copy_of_ADA_Concept_Proposal_Revision_3.pdf       # Study proposal
├── Table1_Descriptive_Statistics1.docx               # Descriptive table output
├── Table2_Regression_Results1.html                   # Regression table output
├── Regression_Results_ACE_CVD1.csv                   # Regression results CSV
├── Figure1_CVD_Prevalence_by_ACE.png                 # Prevalence bar chart
└── Figure2_Forest_Plot_ORs.png                       # Forest plot of ORs
```

**Last Updated**: December 2025
