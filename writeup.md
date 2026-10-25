# Assignment 2: Menu items across 7 restaurants

Code: `notebooks/Assignment_2.ipynb`. Original data: `data/raw/DataDictionary.xlsx`. Processed data: `data/processed/menu_clean.csv`. How to run: `README.md`.

## Task 1: Revisit your data and questions

Our dataset has 98 menu items from 7 restaurants (McDonald's, Taco Bell, Wendy's, Chipotle, Panera, Chili's, Cheesecake Factory) in three restaurant types, with 14 attributes. It has no missing values, duplicates or impossible values, and the calories match the listed nutrients for every item. Nine item names have trailing spaces and some restaurant names are missing apostrophes (`Wendys`).

Two things looked unusual. Wendy's has the highest sodium per calorie of any restaurant (the Baconator is 5,800 mg for 1,001 calories), so we checked it against Wendy's official nutrition information. The values are correct, so we keep them. The Taco Bell Black Bean Burrito was listed at 50 calories, but Taco Bell lists 380, so we correct it. For coverage, each restaurant type has only two or three restaurants, and Fast Casual is only Chipotle and Panera. Serving sizes range from 35 g to 665 g, so we compare per 100 g and per dollar as well as in totals.

We kept four questions because the data can support each one, and changed three of them:

1. **Composition vs portion (was Q1).** Do restaurant types differ in what their food is made of, or only in how much they serve?
2. **Size vs density (was Q2).** Which items are high in calories because they are large and which because they are dense?
3. **Value (was Q3).** Where do you get the most protein per dollar, and how much of the price is just portion size?
4. **Hidden sugar (Q4, kept).** Which non-sweet items have as much sugar as desserts? We now look at individual items, not category averages.

We dropped vegetarian vs non-vegetarian as its own question to stay focused. It can be a color or filter later.

## Task 2: Prepare the data

The notebook (Task 2 section) turns the original spreadsheet, which is never edited, into `data/processed/menu_clean.csv`, in this order:

- **Cleaning:** shorter column names, trailing spaces removed, restaurant names fixed.
- **Correction:** Black Bean Burrito calories 50 → 380. It is saved in `data/processed/corrections_log.csv` and marked `corrected` in the `data_status` column. No rows are removed.
- **New columns:** calories, protein, sodium, sugar and price per 100 g, protein per dollar and sodium per calorie, so items of different sizes can be compared.
- **Food groups:** the 15 food categories (eight have 4 or fewer items) are grouped into six: Handheld, Salad/Bowl, Breakfast, Sweet, Entree, Sides. The original category stays.

## Task 3: Explore the data

We used plain plots here and left the designed visualizations for Task 4. Histograms and a correlation matrix are in the notebook: every numeric attribute is right-skewed (a few very large items), and calories, protein, fat, sodium, serving size and price are all positively correlated (mostly 0.6 to 0.9), because bigger items have more of everything.

**Unusual observations.** Wendy's stands out in sodium per calorie (median 2.6 vs 1.7 to 2.5), driven by a few burgers and sandwiches. This is the outlier we checked in Task 1 and it is real.

![Sodium per calorie by restaurant](figures/fig3_sodium_per_calorie.png)

**Type vs restaurant, total vs per 100 g.** Median calories per item are 402, 828 and 1,035 for Fast Food, Fast Casual and Casual Dining, but per 100 g they are 245, 187 and 207, so the differences in totals come mostly from how much is served. The restaurants inside a type also differ: Fast Casual is Chipotle (about 620 g per item, 172 calories per 100 g) and Panera (about 290 g, 283 per 100 g). With two or three restaurants per type, a "type effect" can really be one restaurant, so we keep restaurants visible.

![Type vs restaurant](figures/fig4_type_vs_restaurant.png)

**Size, price and protein.** Salads are the lightest food group per 100 g (151 calories vs 235 for handhelds and 385 for sweets), yet 9 of the 12 salads have more calories than the median burger (580) because they are so large. Price vs protein mostly shows that bigger items cost more, so protein per dollar is more informative; it ranges from 3.8 (Chipotle) and 3.5 (Wendy's) down to 1.3 (Cheesecake Factory). Prices come from one location, so this is one sample.

![Serving size vs calories, price vs protein, protein per dollar](figures/fig5_relationships.png)

**Aggregation hides sugar.** Sweet items average 45 g of sugar, but non-sweet groups have medians of only 5 to 18 g, with a few items far above them. Non-sweet items hold 56% of all sugar in the dataset, and eight are above the outlier limit (21.5 g), five of them from Cheesecake Factory (Jamaican Black Pepper Shrimp 70 g, Chinese Chicken Salad 63 g). We first noticed these values while collecting the data, which is why hidden sugar became a domain question.

![Sugar by food group](figures/fig6_sugar_by_group.png)

**What exploration changed.** Q1 and Q3 now separate portion size from composition and price level, Q2 focuses on density, and Q4 should look at individual items instead of category averages.

## Task 4: Implement visualization alternatives

We implemented one direction per question.

### Direction A: serving size vs calories (Q2)

Points show serving size (x) and calories (y), color shows restaurant type, and dotted lines mark equal calories per 100 g, so density is the angle of a point from the origin. We tried it because size explains most of the calories (r = 0.77), so the interesting question is who sits above or below the line. **Worked:** Fast Food items cluster between the 200 and 300 cal/100 g lines, and the large Casual Dining and Fast Casual items sit below and to the right. **Did not work:** the 600 g Chipotle items stack up, and dots cannot be identified without hover. **Decision:** keep.

![Direction A](figures/fig7_directionA_size_density.png)

### Direction B: protein per dollar by restaurant (Q3)

One dot per item, one row per restaurant sorted by median, with a black line for the median. We switched from price vs protein to a ratio because price mostly tracks size. **Worked:** the ranking is easy to read, and the spread inside a restaurant is often larger than the gap between restaurants (Wendy's goes from 0 to 7.5). **Did not work:** the plot does not say which items are good value, and a ratio hides item size. **Decision:** modify, as a supporting view.

![Direction B](figures/fig8_directionB_protein_per_dollar.png)

### Direction C: hidden sugar (Q4)

**Attempt 1 (abandoned).** Our Assignment 1 sketch was a heatmap of average sugar by food group and type. It fails with this data: Entrees exist only in Casual Dining, so two cells are empty, several cells hold one to three items, and the Sweet row (25, 42, 75 g) takes the whole color scale so the other rows (2 to 21 g) look alike.

![Direction C, attempt 1](figures/fig9_directionC_heatmap_attempt.png)

**Attempt 2 (kept).** One dot per item, one row per food group, a dashed line at the outlier limit and labels on items beyond it. **Worked:** the eight outliers stand out and are named. **Did not work:** labels in the Entree row crowd each other. **Decision:** keep, but it needs item names on demand.

![Direction C, attempt 2](figures/fig10_directionC_sugar_items.png)

## Task 5: Interaction and multiple views

The static plots share one problem: dots cannot be identified. We built an interactive Plotly version in the notebook that puts Directions A and C side by side. We left out Direction B because the restaurant highlight covers most of what it did.

**Why two views.** Each item appears in both, and one question needs both: is an item with a lot of sugar also very large or very dense? For example, the Chinese Chicken Salad (63 g sugar) is large (600 g) but not unusually dense (about 200 cal/100 g).

**Interactions, each for a task:**
- **Hover** shows item, restaurant, size, calories, sugar and price, replacing the hand-placed labels.
- **Type legend:** clicking a type hides it in both views at once.
- **Restaurant dropdown** fades every other restaurant in both views, so the item stays in context. We chose fading over filtering because one restaurant alone has too few dots to show a pattern.

We did not add zoom or pan because 98 points fit on screen. Plotly in a plain notebook cannot link a click in one view to the other, so hover, the shared legend and the dropdown connect the views.



## Task 6: Iterate and refine

We refined the linked view from Task 5 (version 1 above), because it is the only direction that connects two of our questions.

**Iteration 1: from a strip of dots to ranked bars.**
- **Problem:** 35 of the 37 Handheld items have 12 g of sugar or less, so that row is a pile of overlapping dots. The view also did not show what we want to know: which non-sweet items reach dessert-level sugar.
- **Change:** the right view became a ranked bar chart of the 15 non-sweet items with the most sugar, with the median dessert (46 g) as a reference line. This changes the marks (bar length), the aggregation (15 items) and the scope (non-sweet only).
- **Result:** only the Jamaican Black Pepper Shrimp (70 g) and the Chinese Chicken Salad (63 g) are above the median dessert, with the Hotcakes (45 g) just below. The cost is the overview of all items, which the Task 4 strip still shows.


**Iteration 2: connect the views with marks, not only with hover.**
- **Problem:** nothing in the left view showed which dots were the high-sugar items; you had to hover.
- **Change:** the eight non-sweet items above the 21.5 g limit (from Task 3) became larger diamonds with a black outline in the size view. We used a shape because color already encodes type.
- **Result:** seven of the eight are large (450 g or more); only the Hotcakes (250 g) are small. But only 7 of the 30 items of 450 g or more are diamonds, so being large does not mean being high in sugar.

## Task 7: Current design

The current design is the last notebook cell (Task 7, Plotly): **size vs calories** on the left and **the 15 non-sweet items with the most sugar** on the right. Hover gives details, a legend click hides a type in both views, and a dropdown fades all but one restaurant. In the left view, diamonds mark the items above the sugar limit, and a note under the plot explains them (the only change from iteration 2).

**Questions it supports.** Size vs density (Q2), hidden sugar (Q4), and whether the high-sugar items are also the large or dense ones. It does not support Q1 or Q3 directly; those are covered by the Task 3 boxplots and Direction B.

**Main design decisions.** Position encodes the two quantities in each view and color encodes restaurant type in both. Density comes from reference lines instead of a third axis. Sugar is shown as ranked bars limited to non-sweet items, with desserts reduced to one line. The views are linked by the diamonds, the shared legend and the dropdown. A restaurant is highlighted by fading, so every item stays visible.

**How it differs from earlier versions.** The Assignment 1 heatmap of average sugar did not work with this data. The Task 4 plots were static and one question each. The Task 5 version had a crowded strip of dots, and its views were linked only by hover.

**What works well.** In our own testing, hover, the legend and the dropdown update both views, the bars make the comparison with desserts direct, and the diamonds show that most high-sugar items are large. Nobody else has tried it yet.

**What we are unsure about.**
- Is 15 the right number of bars, and do people miss the overview of all items?
- Sugar is in grams per item, and large items have more of everything. A 600 g salad can reach 63 g without being unusually sweet per bite. We chose grams because the question is how much sugar a person eats, but per 100 g would tell a different story.
- Will people find the legend and dropdown, and understand the diamonds?
- Are faded dots too faint to read?
- Data limits: Fast Casual is only two restaurants, and prices come from one location.

**What we want to evaluate in Assignment 3.** Can people tell whether an item is high in calories because it is large or dense? Can they name the non-sweet items that match a dessert, and how fast compared with the strip chart? Do they discover the legend and dropdown unprompted, and do they understand the diamonds? Is grams per item or per 100 g the better sugar measure?
