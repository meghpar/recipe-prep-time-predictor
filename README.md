# Meals in Minutes: Predicting Prep Time with Nutritional Insights
## 

**Name(s)**: Meghana Paruchuri and Rishitha Talluri

## INTRODUCTION
For this project, we will be working with the recipe and ratings dataset that describes different foods to make and related information. 
The main question we are aiming to answer is: what types of recipes tend to take longer to cook? Specifically, we aim to use the "healthiness" of the recipe to predict cooking time.

Understanding the relationship between nutrition and cooking time is important in helping home cooks make more informed decisions about what to prepare, especially if they're looking for healthier recipes that fit into tight schedules.
It could also provide insight onto how the nutritional values of a dish may be correlated with its cooking time.

The initial dataset for recipes has 231637 rows × 12 columns, whereas the dataset for ratings has 1132367 rows × 5 columns. When we move forward with the next part of the project, we will clean and merge the two datasets.
The columns that are relevant to our research include:
- "minutes": how long it takes to prepare the recipe
- "nutrition": nutrition information written as [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]
- "id": the recipe id


## DATA CLEANING AND EXPLORATORY DATA ANALYSIS

## 🧼 Data Cleaning
### 1. Merging Recipes and Ratings
- Combined the recipes and ratings datasets using a left join on recipe ID
- This ensures every recipe is preserved while incorporating all related user ratings and reviews
- Resulted in a wider dataset where each recipe may have multiple entries (one per rating)

### 2. Replacing Zero Ratings with NaN
- Ratings with a value of 0 likely means the recipe was "not rated" rather than one that truly received a rating of 0
- Replacing them with NaN ensures they’re excluded from average calculations, improving accuracy and general analysis of our data

### 3. Creating Average Ratings per Recipe
- Computed the mean rating for each recipe
- Mapped this back into the main dataset as a new column avg_rating
- This creates a useful metric for recipe quality based on the general consensus of all the reviews for each recipe

### 4. Splitting Nutrition Information into Columns
- The original nutrition column was a single string with all nutrient values
- Used regex to extract numeric values and split them into individual columns (calories, fat, protein, etc.)
- This enables us to perform quantitative analysis of the different nutrition values, allowing us to model and visualize these values

### 5. Removing Extreme Outliers
- Removed recipes with extremely long preparation times (>2 days) and high calorie counts (<4000 cals)
- These extreme values are most likely user entry errors or edge cases that will skew our analysis
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>name</th>
      <th>minutes</th>
      <th>calories</th>
      <th>fat</th>
      <th>sugar</th>
      <th>sodium</th>
      <th>protein</th>
      <th>saturated_fat</th>
      <th>carbs</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1 brownies in the world    best ever</td>
      <td>40</td>
      <td>138.4</td>
      <td>10.0</td>
      <td>50.0</td>
      <td>3.0</td>
      <td>3.0</td>
      <td>19.0</td>
      <td>6.0</td>
    </tr>
    <tr>
      <td>1 in canada chocolate chip cookies</td>
      <td>45</td>
      <td>595.1</td>
      <td>46.0</td>
      <td>211.0</td>
      <td>22.0</td>
      <td>13.0</td>
      <td>51.0</td>
      <td>26.0</td>
    </tr>
    <tr>
      <td>412 broccoli casserole</td>
      <td>40</td>
      <td>194.8</td>
      <td>20.0</td>
      <td>6.0</td>
      <td>32.0</td>
      <td>22.0</td>
      <td>36.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <td>412 broccoli casserole</td>
      <td>40</td>
      <td>194.8</td>
      <td>20.0</td>
      <td>6.0</td>
      <td>32.0</td>
      <td>22.0</td>
      <td>36.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <td>412 broccoli casserole</td>
      <td>40</td>
      <td>194.8</td>
      <td>20.0</td>
      <td>6.0</td>
      <td>32.0</td>
      <td>22.0</td>
      <td>36.0</td>
      <td>3.0</td>
    </tr>
  </tbody>
</table>

## 1️⃣ Univariate Analysis
### Distribution of Calories
 <iframe src="assets/uni_calories.html" width="800" height="600" frameborder="0"></iframe>
 This plot is a histogram demonstrating the distribution of calories. We can see that the curve is right skewed, with the majority of recipes falling under the 200 - 400 calorie range, then trailing off as the number of calories increases. It is important to note that there are a few outliers in the 3000 and above range as well.

### Distribution of Preparation Times
 <iframe
 src="assets/uni_minutes.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>
 This plot is also a histogram showing the univariate distribution of the 'minutes' column. This curve is heavily right skewed, where the vast majority of recipes take under 49 minutes to make, with minimal to none taking above 550 minutes

## 2️⃣ Bivariate Analysis
### Distribution of Preperation Times by Protein Level
<iframe
 src="assets/bi_box.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>
This box plot reveals that recipes with higher protein levels (PDV) may have slightly longer cooking times, as shown by the upward trend in median prep time across protein groups. This suggests that "healthier" high-protein recipes typically take more time to prepare than lower-protein options, helping answer our question about how recipe healthiness relates to cooking duration.

## 3️⃣ Interesting Aggregates  
### Distribution of Average Prepartion Time based on Recipe by Sodium Level
 <iframe
 src="assets/ishealthy_aggregate.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>
 The "healthy" tag plot actually shows that recipes marked healthy have shorter average prep times compared to non-healthy recipes. While this isn't what we expected there are other factors that could explain this. Unhealthy recipes may be family-sized portions (casseroles, baked goods) while healthy ones may be single size portions.

### Distribution of Healthy Recipes and Average Preperation Time
<iframe
 src="assets/sodium_aggregate.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>
The lower sodium level recipes have shorter average prep times compared to higher sodium level recipes. This could also be explained by other factors such as scale of recipes. Restaurant meals take awhile to make, are high in sodium, and produce high volumes of food.

## 4️⃣ Imputation
There are no NaN values in the nutrition columns (which are the features we are using), so we don't need to conduct any imputation.

## Framing a Prediction Problem
### Problem Type
This is a regression problem, as the target variable we are predicting — the number of minutes it takes to make a recipe — is a continuous numerical value rather than a categorical variable.
### Response Variable
We are predicting the minutes column, which represents how long it takes to prepare a recipe.
### Features Used
Our features come from the nutrition data, which includes:
<ul>
  <li>Calories</li>
  <li>Sodium</li>
  <li>Protein</li>
  <li>Carbohydrates</li>
</ul>
These features are available before a user begins cooking, making them valid for training the prediction model

### Why This Problem?
Understanding the relationship between a recipe's nutritional information and how long it takes to prepare can help users make better choices based on health goals and time constraints. Especially with people who live busy, on-the-go lifestyles – like the average college student – it is important to understand if they can make healthy recipes in minimal time.
### Evaluation Metric
We are going to use the $R^2$ metric to evaluate how well the features are correlated with the dependent variable, preparation time. This makes it an insightful metric for regression, as we want our model to fit our data well (as this means more accurate predictions). This is directly indicated by an $R^2$ value close to 1.
## Baseline Model
This is a linear regression model that predicts the cooking time in minutes it takes to make a recipe based on calories and protein. Minutes, calories, and protein are all quantitative features in the model. We didn't need to do any encodings because all the features were quantitative. However, the features were standardized using StandardScaler to ensure they were on the same scale, which helps improve the performance of linear regression by preventing features with larger magnitudes from dominating the model. The model was trained on 80% of the data, with the remaining 20% reserved for testing. Performance was evaluated using MAE and R². This model performs poorly because we have a high MAE of approximately 68.96. We get a R² close to 0 which means the model explains almost none of the variance in cooking times.
## Final Model
Sodium_level helps distinguish between processed and fresh recipes. Higher-sodium dishes often require less prep time as they may be prepackaged, while low-sodium meals may take longer since you may need to make them from scratch.  Log_calories transformation accounts for the non-linear relationship between calorie content and cooking time, preventing high-calorie outliers from skewing predictions. The modeling algorithm chosen for this task was Random Forest Regressor. The preprocessing pipeline standardizes log_calories and one-hot encodes sodium_level. The model was improved using GridSearchCV, optimizing mean squared error, and the best hyperparameters were a max_depth of 45 and 100 n_estimators. The final model achieves an R² of approximately 0.437, a major improvement from the baseline model which had an R² of approximately 0.03 . The improvement means our final model predicts cooking times much more accurately than the baseline. It now explains 44% of what affects cooking time and makes smaller errors in predictions. 
FIXME: WHY SODIUM LEVEL AND LOG CALORIES



