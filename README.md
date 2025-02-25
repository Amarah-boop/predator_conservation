# predator_conservation
This repository intends to store a reproducible exploratory data analysis exercise with data from an open access scientific paper.

Khorozyan, I., 2022. Importance of non-journal literature in providing evidence for predator conservation. Perspectives in Ecology and Conservation, 346-351.

Description
This project investigates how various study designs and journal sources affect relative risk (RR) values by analyzing data on predator conservation interventions.  Numerous predator species, protected assets, intervention kinds, and research papers are all included in the dataset.

Installation
To run this analysis, you need R and RStudio installed. Then, install the required packages: install.packages(c("openxlsx", "dplyr", "tidyr", "ggplot2"))

Data
Source: NonJournalRawData.xlsx
Format: Excel file
Key Columns:
  Paper: Study reference
  Journal: Journal (1) vs. non-journal (0)
  Study_Design: Research methodology
  Predator_Species: Studied predator(s)
  Protected_Assets: Target assets for protection
  Relative_Risk_RR: Effectiveness measure of intervention

Data Processing Steps
Loading Data: Read the dataset from an Excel file.
Exploratory Data Analysis (EDA):
  View top rows (head())
  Check column names (colnames())
Data Cleaning & Transformation:
  Rename columns for clarity
  Separate multiple values into different rows (separate_rows())
  Convert categorical columns to factors (mutate()).
Data Summarization:
  Group data by Study_Design and Journal
  Compute summary statistics for Relative_Risk_RR (median, min, max, count).
Visualization:
  Plot Study_Design vs. Relative Risk (RR) using ggplot2.
  Use scatter points and error bars to show data distribution.

Usage
To run the analysis, execute the following script in R:
# Load required libraries
  library(openxlsx)
  library(dplyr)
  library(tidyr)
  library(ggplot2)
# Read data
  raw_data <- read.xlsx("C:/Users/Student/Documents/GitHub Deliverable/predator_conservation/NonJournalRawData.xlsx")
# Rename columns
  tidy_data <- raw_data %>%
    rename(
      Journal = `Journal.(1)/non-journal(0)`,
      Study_Design = `Study.design`,
      Predator_Species = `Predator.species`,
      Protected_Assets = `Protected.assets`,
      Intervention_Category = `Category.of.intervention`,
      Intervention_Type = `Type.of.intervention`,
      Country = `Country`,
      Damage_Metric = `Damage.metric`,
      Relative_Risk_RR = `Relative.risk.RR`
    )
# Clean and process data
  tidy_data <- tidy_data %>%
    separate_rows(Predator_Species, sep = ", ") %>%
    separate_rows(Protected_Assets, sep = ", ") %>%
    mutate(
      Journal = as.factor(Journal),
      Study_Design = as.factor(Study_Design),
      Predator_Species = as.factor(Predator_Species),
      Protected_Assets = as.factor(Protected_Assets),
      Relative_Risk_RR = as.numeric(Relative_Risk_RR)
    )
# Summarize data
  summary_data <- tidy_data %>%
    group_by(Study_Design, Journal) %>%
    summarise(
      median_RR = median(Relative_Risk_RR, na.rm = TRUE),
      min_RR = min(Relative_Risk_RR, na.rm = TRUE),
      max_RR = max(Relative_Risk_RR, na.rm = TRUE),
      count = n(),
      .groups = "drop"
    )
# Visualization
  ggplot(summary_data, aes(x = Study_Design, y = median_RR, color = factor(Journal))) +
    geom_point(position = position_dodge(width = 0.5), size = 4) +
    geom_errorbar(aes(ymin = min_RR, ymax = max_RR),
                position = position_dodge(width = 0.5),
                width = 0.2, size = 1) +
    geom_text(aes(label = count, y = max_RR),
            position = position_dodge(width = 0.5),
            vjust = -0.5, size = 3, color = "black") +
    scale_color_manual(values = c("red", "blue"),
                    labels = c("Non-journals", "Journals")) +
    labs(title = "Study Design vs. Relative Risk (RR)",
       x = "Study Design", y = "Relative Risk (RR)",
       color = "Journal Type") +
    theme_minimal() +
    theme(axis.text.x = element_text(angle = 45, hjust = 1))
    
Results
The analysis sheds light on the ways in which various study designs affect relative risk levels.
Based on the effectiveness of the intervention, the visualization aids in comparing journal with non-journal studies.

Contribution
Contributions are welcome. Feel free to copy the repository and submit pull requests.

License
The project is licensed under the MIT license.

Contact
Feel free to reach out if you have any questions.
    
[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/Amarah-boop/predator_conservation/HEAD)
