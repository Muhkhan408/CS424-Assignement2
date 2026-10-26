# Assignment 2: Menu items across 7 restaurants

## Task 1: Revisit your data and questions

The dataset includes 98 menu items offered by 7 restaurants (McDonald's, Taco Bell, Wendy's, Chipotle, Panera, Chili's, Cheesecake Factory), which fall into 3 categories of restaurants, and each item has 14 features. The data contains no nulls, duplicates, or unreasonable values, all calorie counts align with the nutritional content listed for each item. Additionally, 9 item titles end in a space and several restaurant titles lack apostrophes (e.g., Wendys).

I noticed two things that were a bit odd. I checked Wendy's against their own official nutritional data, since they have more sodium per calorie than any other restaurant (the Baconator has 5,800 mg for 1,001 calories). Their numbers are correct, therefore, I will leave them as they appear. However, I corrected the Taco Bell Black Bean Burrito which was listed as having 50 calories when Taco Bell lists it as having 380, so we corrected it. For coverage, each restaurant type has only two or three restaurants, and Fast Casual is only Chipotle and Panera. Serving sizes range from 35 g to 665 g, so we compare per 100 g and per dollar as well as in totals.


The four questions were retained since they are supported by the data, and three were modified as follows:

1. **Food composition vs. food portion size (formerly question 1).** Do different restaurants use different ingredients, or only different portion sizes? 2. **Food size vs. food density (formerly question 2).** What foods are calorie-rich because of their size, and what foods are calorie-rich because of their density? 3. **Cost effectiveness (formerly question 3).** In what food item is there the highest proportion of protein per dollar spent, and how much does it cost solely due to portion size? 4. **Hidden sugar content (question 4, retained).** In which non-dessert food items is the sugar content comparable to that of desserts?

Vegetarianism is an aspect that we will not be asking about specifically, but it may be used in a filter later on.

## Task 2: Prepare the data

The notebook (in Task 2 section) makes an output from the uneditable spreadsheet to `\menu_clean.csv` in this sequence:

- **Data Cleaning**: shortening the column names, removal of trailing spaces, fixing the names of the restaurants. - **Correction**: calories of Black Bean Burrito 50 → 380. It is saved in `data/processed/corrections_log.csv` and the status in `data_status` is `corrected`. Rows are not deleted. - **Columns added**: calories, protein, sodium, sugar, and price per 100g, protein per dollar, sodium per calorie. This makes comparison of items of various size possible. - **Food categories**: 15 types of foods (8 having 4 or fewer items) are divided into six: Handheld, Salad/Bowl, Breakfast, Sweet, Entree, Sides.

## Task 3: Explore the data

We used plain plots here and left the designed visualizations for Task 4. Histograms and a correlation matrix are in the notebook: every numeric attribute is right skewed (a few very large items), and calories, protein, fat, sodium, serving size and price are all positively correlated (mostly 0.6 to 0.9), because bigger items will tend to have a larger amount of everything.

Distribution!(https://github.com/Muhkhan408/CS424-Assignement2/blob/main/Task%203%20Exploratory%20plots/fig1_distributions.png)

Correlations(https://github.com/Muhkhan408/CS424-Assignement2/blob/main/Task%203%20Exploratory%20plots/fig2_correlation.png)

* *Strange results.** Wendy’s is an unusual case with regard to sodium per calorie (2.6 vs. 1.7-2.5). This is the outlier we examined in Task 1. It is not a statistical error.

Restaurant sodium per calorie plot

* Type versus Restaurant; Median calories per item are 402, 828 and 1,035 for Fast Food, Fast Casual and Casual Dining, but per 100 g they are 245, 187 and 207, so the differences in totals come mostly from how much is served. The restaurants inside a type also differ: Fast Casual is Chipotle (about 620 g per item, 172 calories per 100 g) and Panera (about 290 g, 283 per 100 g). With two or three restaurants per type, a "type effect" can really be one restaurant, so we kept restaurants visible.


![Type vs restaurant](https://github.com/Muhkhan408/CS424-Assignement2/blob/main/Task%203%20Exploratory%20plots/fig4_type_vs_restaurant.png)

* *Size, price and protein.** Salads are the lightest food group per 100 g (151 calories vs 235 for handhelds and 385 for sweets), yet surprisingly 9 of the 12 salads have more calories than the median burger (580) because they are so big. Price vs protein mostly shows that bigger items cost more, so protein per dollar is more informative; it ranges from 3.8 (Chipotle) and 3.5 (Wendy's) down to 1.3 (Cheesecake Factory). Prices come from one location, so this is one sample.


![Serving size to calories, Price to protein, Protein per dollar](https://github.com/Muhkhan408/CS424-Assignement2/blob/main/Task%203%20Exploratory%20plots/fig5_relationships.png)

* *Sugar gets lost in aggregation.Sweet products show an average of 45 g of sugar, and non-sweet groups are only at 5 to 18 g median, even if some items are above. Non-sweet products contain 56% of the overall sugar, and eight items passed the outlier threshold (21.5 g), with five of them coming from Cheesecake Factory (Jamaican Black Pepper Shrimp 70 g, Chinese Chicken Salad 63 g). We made the first observation of the values during data collection, therefore the hidden sugar became the domain question.*

![Sugar by food group](https://github.com/Muhkhan408/CS424-Assignement2/blob/main/Task%203%20Exploratory%20plots/fig6_sugar_by_group.png)

* *Changes in the exploration.* Q1 and Q3 distinguish between portion and composition and price levels, Q2 is related to density, and Q4 has to be based on items rather than categories.

## Task 4: Implement visualization alternatives

One direction was used per question.

### Direction A: serving size vs calories (Q2)

Points indicate (x) serving sizes and (y) calories, and colors indicate types of places where the meals come from; dotted lines illustrate the equal number of calories in 100 g, thus the point angle from the origin is its density. We have tried this because we believed that size allows explaining the given number of calories (r = 0.77), thus the question is who is out of or under this line. **WORKED:** The fast food items were clustered between the 200 and 300 cal/100g lines while the big Casual Dining and Fast Casual items were below and to the right. **Did not work:** the Chipotle items at 600 g stacked. **DECISION:** keep

![Direction A](https://github.com/Muhkhan408/CS424-Assignement2/blob/main/Task%204%20Static%20Visuals/fig7_directionA_size_density.png)

### Direction B: protein per dollar by restaurant (Q3)

A single dot represents one item, while each restaurant displays its statistics in a single row, arranged in accordance with median values. The choice of representation has been changed from price per protein to a ratio of price due to the fact that price is closely associated with size of the item. **Success:** It is easy to understand the ranking, and the dispersion within the restaurant is usually greater than the difference between restaurants (Wendy's range is from 0 to 7.5). **Failure:** Does not indicate which items are worth buying, and a ratio masks the size of an item. **Conclusion:** Modify as a supporting visualization.

![Direction B](https://github.com/Muhkhan408/CS424-Assignement2/blob/main/Task%204%20Static%20Visuals/fig8_directionB_protein_per_dollar.png)

### Direction C: hidden sugar (Q4)

* *Attempt 1 (discarded)* The draft version of Assignment 1 was a heatmap of average levels of sugar displayed according to food groups and types. This method did not work here, as there are only entrees in Casual Dining, which means there are empty values for these cells. In addition, there are only one to three items in a cell, and the Sweet row (25, 42, 75 g) uses the full color scale, which makes the other values less importan (2 - 21g).

![Direction C, attempt 1](https://github.com/Muhkhan408/CS424-Assignement2/blob/main/Task%204%20Static%20Visuals/fig9_directionC_heatmap_attempt.png)

* *Attempt 2 (kept).** One dot for each food group item, one row for each type of food, a discontinuous line showing the outlier limit, item labels are above that line. **Did work:** the eight outliers are labelled. **Successful:** eight outliers become apparent, labeled. **Unsuccessful:** overlapping labels in the Entrees line. **Conclusion:** keep, but requires item labeling upon request.

![Direction C, attempt 2](https://github.com/Muhkhan408/CS424-Assignement2/blob/main/Task%204%20Static%20Visuals/fig10_directionC_sugar_items.png)

## Task 5: Interaction and multiple views

The static plots have one issue: dots cannot be identified. An interactive Plotly version was created, which placed Direction A and C in one view together. The Direction B was left out because it is mostly covered by the restaurant highlight. 

**Why two views.** Every item appears in both graphics, but the question can be answered using both plots: if an item with high sugar content is either big or heavy. For instance, the Chinese Chicken Salad has high sugar content (63g) and is big (600g) but of average weight (200 cal/100g). 

**Interactions, each for a task:**
- **Hover** shows item, restaurant, size, calories, sugar and price, replacing the hand-placed labels.
- **Type legend:** clicking a type hides it in both views at once.
(https://github.com/Muhkhan408/CS424-Assignement2/blob/main/Task%205%20Added%20Views/preview.webp)
- **Restaurant dropdown** fades every other restaurant in both views, so the item stays in context. We chose fading over filtering because one restaurant alone has too few dots to show a pattern.
(https://github.com/Muhkhan408/CS424-Assignement2/blob/main/Task%205%20Added%20Views/fig11_plotly_notebook.png)


We did not add zoom or pan because 98 points fit on screen. Plotly in a plain notebook cannot link a click in one view to the other, so hover, the shared legend and the dropdown connect the views.



## Task 6: Iterate and refine

We modified the associated display from Task 5 (illustrated above) as it is the sole pathway connecting both questions posed in relation to the question of whether the brands we chose incorporate too much sugar. 


**Iteration 1: from a strip of dots to ranked bars.**
- **Problem:** 35 out of the 37 Handheld items feature 12 grams of sugar or fewer, making this row quite tedious as it appears cluttered with dots. The view also failed to provide relevant information associated with non-sweet products with a lot of sugar.
- **Change:** in the new version, the figure was transformed into a bar chart that shows the top 15 non-sweet items according to sugar content and features median dessert (46 grams of sugar) as a guideline. This significant adjustment leads to the complete change of the measures (length of the bars), the manner of estimation (15 items) and the scope of the solution (only non-sweet).
- **Result:** only Jamaican Black Pepper Shrimp (70 grams) and Chinese Chicken Salad (63 grams) are above the median level, with Hotcakes having 45 grams of sugar slightly below. The cost is the overview of all items, which the Task 4 strip still shows.
(https://github.com/Muhkhan408/CS424-Assignement2/blob/main/Task%206%20Refined/fig13_iteration1_ranked.png)


**Iteration 2: connect the views with marks, not only with hover.**
- **Problem:** nothing in the left view showed which dots were the high-sugar items; you had to hover.
- **Change:** the eight non-sweet items above the 21.5 g limit (from Task 3) became larger diamonds with a black outline in the size view. We used a shape because color already encodes type.
- **Result:** seven of the eight are large (450 g or more); only the Hotcakes (250 g) are small. But only 7 of the 30 items of 450 g or more are diamonds, so being large does not mean being high in sugar.
(https://github.com/Muhkhan408/CS424-Assignement2/blob/main/Task%206%20Refined/fig15_current_design.png)

## Task 7: Current design

(https://github.com/Muhkhan408/CS424-Assignement2/blob/main/Task%207%20Final%20Visual/fig15_current_design%20(1).png)

The current design is the last notebook cell (Task 7, Plotly): **size vs calories** on the left and **the 15 non-sweet items with the most sugar** on the right. Hovering anywhere provides more information, clicking the legend removes a type of items in both views, and a dropdown option shows only one restaurant at a time. The left side of the plot has a diamond-shaped symbol indicating items having more sugar than the limit, and there is a note below the plot that explains the meaning of the diamonds (only the change made since iteration 2).


**Questions it supports.** Size vs density (Q2), hidden sugar (Q4), and whether the high-sugar items are also the large or dense ones. It does not support Q1 or Q3 directly; those are covered by the Task 3 boxplots and Direction B.

**Main design decisions.** Position is responsible for conveying both quantities in each view, while color indicates the type of restaurant in both. The density is derived from reference lines rather than a third axis. Sugar is represented using ranked bars limited to only non-sweet goods, while the desserts are displayed on a single line. The views are interrelated with diamonds, the identical legend and the dropdown. The restaurant is highlighted through fading and everything remains visible.

**How it differs from earlier versions.** The heatmap in Assignment 1 did not make use of this data. The plots in Task 4 were static and responded to one question only. The Task 5 version featured a visually-heavy strip of dots whereas views were connected by mouse hovering.

**What works well.** In our own trials hover, legend and dropdown successfully update both views, while the bars make the comparison with desserts even clearer and diamonds have shown that most sugar-rich products are also big. No one has tried a solution like this one. 

**What we are unsure about.**
- Is 15 the right number of bars, and do people miss the overview of all items?
- Sugar is in grams per item, and large items have more of everything. A 600 g salad can reach 63 g without being unusually sweet per bite. We chose grams because the question is how much sugar a person eats, but per 100 g would tell a different story.
- Will people find the legend and dropdown, and understand the diamonds?
- Are faded dots too faint to read?
- Data limits: Fast Casual is only two restaurants, and prices come from one location.

**What we want to evaluate in Assignment 3.** Are the respondents able to determine whether an item is high-calorie because of its sheer size or density? How well do they identify unsweet foods similar to dessert and how quickly they achieve that in relation to our strip chart? Do they recognize the legend and drop-down menu without being told about it? Are they able to understand diamonds? 

