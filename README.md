# Recipe-and-Rating-Analysis
Final project for DSC 80

## Introduction
On Food.com, thousands of recipes come paired with detailed nutrition facts (calories, fat, sugar, sodium, protein, saturated fat, carbohydrates) and user ratings. In my project, I ask: **“Is there a correlation between a recipe’s carbohydrate content (carbs) and its total calories?”** Readers who care about meal planning, weight management, or simple nutritional trade‐offs will find this especially useful—if high‐carb recipes reliably drive up calorie counts, then lowering carbs may be an effective way to reduce calories without digging deeper into ingredient lists.

The **recipes** dataset had 83,782 rows(recipes) and 12 columns.  The relevant columns include:
The **recipes** dataset contains the following fields:

* **id**: a unique integer identifier for each recipe.
* **contributor\_id**: the integer user-ID of the person who submitted the recipe.
* **name**: the recipe’s title (a string).
* **minutes**: total preparation time in minutes (integer).
* **n\_steps**: the number of instruction steps (integer).
* **submitted**: the date the recipe was posted on Food.com, in `YYYY-MM-DD` format.
* **tags**: a comma-separated list of Food.com tags (e.g. `dessert`, `vegetarian`, `30-minute meals`) stored as a string.
* **nutrition**: a bracketed list of seven values—calories (kcal), total\_fat (% DV), sugar (% DV), sodium (% DV), protein (% DV), saturated\_fat (% DV), and carbohydrates (% DV)—for example:

  ```
  [250, 10, 25, 15, 20, 5, 35]
  ```

The **interactions** dataset had 731,927 rows (reviews/ratings) and 5 columns. The columns include: 

* **user\_id**: unique integer ID of the user who submitted the rating or review.
* **recipe\_id**: integer ID of the recipe being rated (corresponds to `id` in the recipes file).
* **date**: date of the interaction in `YYYY-MM-DD` format.
* **rating**: numeric star rating (1–5).
* **review**: optional free-text comment (string), which may be blank.


Merging them yields a combined dataset of 234,429 rows × 26 columns. In order to aid the exploration of my project question, I parsed the nutrition column and created separate numeric fields:


| New Column          | Source in `nutrition` | Description                           |
| ------------------- | --------------------- | ------------------------------------- |
| **`calories`**      | nutrition\[0]         | Total calories (kcal) per serving     |
| **`total_fat`**     | nutrition\[1]         | Total fat as PDV (“% of daily value”) |
| **`sugar`**         | nutrition\[2]         | Sugar as PDV                          |
| **`sodium`**        | nutrition\[3]         | Sodium as PDV                         |
| **`protein`**       | nutrition\[4]         | Protein as PDV                        |
| **`saturated_fat`** | nutrition\[5]         | Saturated fat as PDV                  |
| **`carbs`**         | nutrition\[6]         | Carbohydrates as PDV                  |

From the dataset, the most two relevant columns are calories (Total calories (kcal) per serving) and carbs (Carbohydrates as PDV).

## Data Cleaning and Exloratory Data Analysis
### Data Cleaning
#### 1. Merging Datasets
The datasets recipes and interactions were left merged on id and recipe_id. This step allows analysis of how recipe attributes relate to user feedback.
#### 2. Replace 0's with NaN
Ratings of 0 indicate missing values because the lowest rating that is valid is usually 1. Ratings of 0 were replaced with NaN using .replace(0, np.nan) to prevent skewed analysis. 
#### 3. Create Average Rating Column
Because recipes have multiple ratings, I calculated the average rating for each recipe to help summarize feedback and added it as a column (avg_rating) to the merged dataset. 
#### 4. Parsed Nutrition column into separate columns for calories, total_fat, sugar, sodium, protein, saturated_fat, carbs
This allows for easier analaysis between individual components fo the nutrition data.
#### 5. Remove Outliers
I removed recipes with calories that were 3,000 calories or over. The recommended daily calorie intake for adults can range from 1,600 to 3,000, with the standard reference point being 2,000 calories. 
#### 6. Checked for missing values
I checked if there were missing values in the calories and carbs columns verify data completeness. 

#### The table below shows the first five unique recipes from the cleaned DataFrame, limited to the relevant columns. 
#### The table below shows the first five unique recipes from the cleaned DataFrame:

| name                            | id     | contributor_id | user_id | n_steps | n_ingredients | avg_rating | calories | carbs | total_fat | sugar | sodium | protein | saturated_fat |
|---------------------------------|--------|---------------:|--------:|--------:|--------------:|-----------:|---------:|------:|----------:|------:|-------:|--------:|--------------:|
| brownies in the world best ever | 333281 |        985201  | 386585  | 10      | 9             | 4.0        | 138.4    | 6.0   | 10.0      | 50.0  | 3.0    | 3.0     | 19.0          |
| 1 in canada chocolate chip…     | 453467 |     1,848,091  | 424680  | 12      | 11            | 5.0        | 595.1    | 26.0  | 46.0      | 211.0 | 22.0   | 13.0    | 51.0          |
| broccoli casserole              | 306168 |       50,969   | 29782   | 6       | 9             | 5.0        | 194.8    | 3.0   | 20.0      | 6.0   | 32.0   | 22.0    | 36.0          |
| millionaire pound cake          | 286009 |      461,724   | 813055  | 7       | 7             | 5.0        | 878.3    | 39.0  | 63.0      | 326.0 | 13.0   | 20.0    | 123.0         |
| 2000 meatloaf                   | 475785 |    2,202,916   | 2204364 | 17      | 13            | 5.0        | 267.0    | 2.0   | 30.0      | 12.0  | 12.0   | 29.0    | 48.0          |


### Univariate Analyses
<iframe
  src="assets/calorie_below2000.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>



