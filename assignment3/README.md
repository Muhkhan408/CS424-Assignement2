# Assignment 3: Interaction and evaluation

The report is [`report.md`](report.md), organized by Tasks 1 to 5.

```
report.md                       single report, Tasks 1-5
visualizations/
  static.html, static.png       static version (small multiples)
  interactive.html              interactive version (self-contained, open in a browser)
  screenshot_interactive_filtered.png
evaluation/
  results.csv                   anonymized measurements, one row per participant and question
  answer_key.csv                restaurants shown and correct answer for each question
  assignment_plan.csv           planned question order for each group
  results_template.csv          blank sheet with the results.csv columns
results_figures/                accuracy, time and participant charts (made by the analysis notebook)
notebooks/
  Assignment_3.ipynb (.html)           Task 1: builds the static and interactive versions
  Assignment_3_analysis.ipynb (.html)  Task 4: statistics and result charts
  menu_clean.csv, results.csv          copies next to the notebooks, for Google Colab
data/processed/menu_clean.csv  dataset from Assignment 2 (98 menu items)
```

## Run the visualizations
Open `visualizations/static.html` and `visualizations/interactive.html` in a browser (no internet needed). To rebuild them, run `notebooks/Assignment_3.ipynb` from the top. It reads `data/processed/menu_clean.csv` and writes to `visualizations/`.

## Run the analysis
Run `notebooks/Assignment_3_analysis.ipynb` from the top. It reads `evaluation/results.csv` and writes the three charts to `results_figures/`.

## Google Colab
Upload the notebook, then drag the CSV it needs into the **content** folder (`menu_clean.csv` for `Assignment_3.ipynb`, `results.csv` for `Assignment_3_analysis.ipynb`). File names are case-sensitive and must be lowercase. Then choose Runtime > Run all. In Colab the charts and pages are saved in temporary folders, so download them before closing.
