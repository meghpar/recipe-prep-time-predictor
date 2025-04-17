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

<iframe src="assets/recipe_ratings_head.html" width="800" height="400" frameborder="0">
</iframe>




## 1️⃣ Univariate Analysis

### Distribution of Calories
 <iframe
 src="assets/uni_calories.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>
 This plot is a histogram demonstrating the distribution of calories. We can see that the curve is right skewed, with the majority of recipes falling under the 200 - 400 calorie range, then trailing off as the number of calories increases. It is important to note that there are a few outliers in the 3000 and above range as well.

### Distribution of Preparation Times
 <iframe
 src="assets/uni_minutes.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>
 This plot is also a histogram showing the univariate distribution of the 'minutes' column. This curve is heavily right skewed, where the vast majority of recipes take under 49 minutes to make, with minimal to none taking above 550 minutes

## 1️⃣ Bivariate Analysis