# Amazon-Sales-Analysis
![alt text](data/th.jpg)
###
## Goal
###
This project provides a comprehensive analysis of Amazon product data sourced from Kaggle, aimed at exploring various aspects of product performance, customer behavior, and sales trends. The goal of the project is to generate actionable insights that can inform business strategies related to product development, pricing, promotions, and customer engagement.

## Data Source:

Dataset: Amazon product data sourced from Kaggle, including attributes such as product names, prices, discount percentages, ratings, review sentiments, and more.
Link: [click here](https://www.kaggle.com/datasets/karkavelrajaj/amazon-sales-dataset/data)

## Dataset Overview
This dataset is having the data of 1K+ Amazon Product's Ratings and Reviews as per their details listed on the official website of Amazon

Features

- product_id - Product ID
- product_name - Name of the Product
- category - Category of the Product
- discounted_price - Discounted Price of the Product
- actual_price - Actual Price of the Product
- discount_percentage - Percentage of Discount for the Product
- rating - Rating of the Product
- rating_count - Number of people who voted for the Amazon rating
- about_product - Description about the Product
- user_id - ID of the user who wrote review for the Product
- user_name - Name of the user who wrote review for the Product
- review_id - ID of the user review
- review_title - Short review
- review_content - Long review
- img_link - Image Link of the Product
- product_link - Official Website Link of the Product

## Project Structure:
-	data/: Contains the original and cleaned datasets used in the analysis.
-	notebooks/: Jupyter notebooks containing the code for data cleaning, analysis, and visualization.
-	plots/: Generated visualizations that are included in the analysis.

## Technologies Used:

-	Python: For data analysis, including libraries like Pandas, NumPy, and Scikit-learn.
-	Matplotlib & Seaborn: For data visualization.
-	NLTK & VADER Sentiment Analysis: For analyzing the sentiment of customer reviews.
-	Jupyter Notebooks: For interactive data exploration and analysis.

## Analysis and Insights

### Category Distribution:

![alt text](plots/cd6d8d04-9896-465e-b482-de2d230509c5.png)

1. Level 1 Category Distribution:

	•	Dominant Categories:
	•	Home & Kitchen is the most represented category, with 448 products. This suggests that Home & Kitchen is a major focus area, likely reflecting high demand or a wide variety of products in this space.
	•	Electronics and Computers & Accessories are also heavily represented, indicating these are significant categories within the dataset.
	•	Less Represented Categories:
	•	Categories like Toys & Games, Car & Motorbike, and Health & Personal Care have significantly fewer products, indicating these may be niche areas or secondary focuses.

2. Level 2 Category Distribution:

	•	Top Subcategories:
	•	Kitchen & Home Appliances and Accessories & Peripherals dominate at this level, which aligns with the focus on Home & Kitchen and Computers & Accessories from Level 1.
	•	Home Theater, TV & Video, and Heating, Cooling & Air Quality are also significant subcategories, reflecting specific product focuses within the broader categories.
	•	Diverse Representation:
	•	There is a wide variety of subcategories, which suggests that within the main categories, there is a broad range of products catering to specific needs (e.g., Wearable Technology, Mobiles & Accessories).

3. Level 3 Category Distribution:

	•	Highly Specific Product Types:
	•	Small Kitchen Appliances and Smartphones & Accessories are the most represented Level 3 categories, indicating a deep focus on these specific product types.
	•	The distribution at this level shows a sharp drop-off after the top categories, with many subcategories having a small number of products, which might reflect niche markets or highly specialized products.

##
### 1. How Does Discount Percentage Impact Sales
```py
amazon_orange = '#FF9900'
amazon_blue = '#146EB4'
amazon_grey = '#555555'
amazon_light_grey = '#F0F0F0'

discount_impact = df.groupby('discount_percentage').agg({
    'rating' : 'mean',
    'rating_count' : 'sum'
}).round(2).reset_index()
discount_impact

#Impact of Discount Percentage and Rating on Sales
plt.figure(figsize=(10,6))
sns.scatterplot(x=discount_impact['discount_percentage'],y=discount_impact['rating'],hue=discount_impact['rating_count'],palette='rocket')
plt.title('Impact of Discount Percentage and Rating on Sales', fontsize=12)
plt.xlabel('Discount Percentage', fontsize=14)
plt.ylabel('Average Rating', fontsize=12)
plt.legend(title ='Sale Count',bbox_to_anchor=(1.05, 1), loc=2, borderaxespad=0.)

#sales against discount percentage
plt.figure(figsize=(12,7))
sns.lineplot(x=discount_impact['discount_percentage'],y=discount_impact['rating_count'],color=amazon_orange,linestyle='--',marker='o')
plt.title('Impact of Discount Percentage on Sales (Rating Count)', fontsize=16)
plt.xlabel('Discount Percentage', fontsize=14)
plt.ylabel('Total Sales (Rating Count)', fontsize=14)
plt.grid(True)
plt.show()

#ratings against discount percentage
plt.figure(figsize=(12,7))
sns.lineplot(x=discount_impact['discount_percentage'],y=discount_impact['rating'],color=amazon_blue,linestyle='--',marker='o')
plt.title('Impact of Discount Percentage on Customer Ratings', fontsize=16)
plt.xlabel('Discount Percentage', fontsize=14)
plt.ylabel('Average Rating', fontsize=14)
plt.grid(True)
plt.show()

```
![alt text](plots/48a8dfc9-5443-4b7c-a791-9d36608d19ff.png)
![alt text](plots/2809a41c-c8c8-4e23-a11f-7f9ea21a7b62.png)
![alt text](plots/7f774e21-90ec-4312-bee5-1112d22e495b.png)

1. Impact of Discount Percentage on Average Ratings (Scatter Plot)

	•	General Trend:
	•	There is a slight negative correlation between discount percentage and average ratings. As the discount percentage increases, the average rating tends to decrease slightly, though the relationship is not very strong.
	•	Ratings fluctuate around 4.0 to 4.2 across various discount levels, but as discounts approach higher levels (above 60%), ratings tend to drop below 4.0 more frequently.
	•	Possible Interpretation:
	•	Perceived Value: Customers might perceive heavily discounted products as lower quality, leading to slightly lower ratings. This perception could be due to the expectation that premium products are less likely to be deeply discounted.
	•	Attracting Price-Sensitive Buyers: Higher discounts might attract more price-sensitive customers who may have higher expectations, leading to more critical reviews.
	•	Business Implication:
	•	Balancing Discounts and Quality Perception: While offering discounts can boost sales, it’s essential to maintain a balance so that the perceived quality of products isn’t compromised, which can lead to lower customer satisfaction.

2. Impact of Discount Percentage on Sales (Rating Count)

	•	Sales Volume Trend:
	•	Sales (as indicated by rating count) show a significant increase as discount percentage increases, peaking around the 40-60% discount range. Beyond this range, sales start to decline, especially as discounts approach 80-90%.
	•	The highest peaks in sales are observed around the 40% and 60% discount levels.
	•	Possible Interpretation:
	•	Optimal Discount Range: There seems to be an optimal discount range where sales are maximized, specifically between 40% and 60%. This could indicate that this discount range is most appealing to consumers, driving higher sales volumes.
	•	Diminishing Returns: Beyond a certain discount threshold, the appeal may decrease, or the market might be saturated, leading to a drop in sales despite higher discounts.
	•	Business Implication:
	•	Strategic Discounting: Focus on offering discounts in the 40-60% range to maximize sales volume. Discounts higher than this may not yield proportional increases in sales and might affect profitability negatively.

3. Combined Impact of Discount Percentage and Ratings on Sales (Bubble Plot)

	•	Visualization:
	•	The bubble plot combines the discount percentage with both sales volume (bubble size) and average rating (y-axis), providing a comprehensive view of how these factors interact.
	•	Larger bubbles are concentrated around the 40-60% discount range, indicating that this range not only maximizes sales but also maintains relatively good ratings.
	•	Possible Interpretation:
	•	Sweet Spot: The intersection of moderate discounts and maintaining good ratings occurs in the 40-60% discount range, reinforcing the idea that this range is optimal for both sales and customer satisfaction.
	•	Lower Ratings with Higher Discounts: As discounts increase beyond this range, both the size of the bubbles (sales) and the height on the y-axis (ratings) tend to decrease, suggesting diminishing returns on both fronts.
	•	Business Implication:
	•	Targeted Discount Strategies: Utilize the 40-60% discount range as a standard strategy to maximize both sales volume and maintain product ratings. This approach ensures a balance between appealing to price-sensitive customers and maintaining a positive product image.

4. Overall Insights and Recommendations:

	•	Optimal Discounting Strategy: Focus on offering discounts in the 40-60% range, where sales volume is maximized and ratings remain relatively high. Avoid excessive discounts unless necessary, as they may lead to diminishing returns in both sales and customer satisfaction.
	•	Perception Management: Be cautious with high discounts as they might lower perceived value, which can impact customer ratings. Consider pairing discounts with marketing that emphasizes quality to counteract any negative perceptions.
	•	Further Analysis: It could be valuable to explore how these trends vary across different product categories to see if the optimal discount range differs depending on the type of product.

##
### 2. What Features of Product Descriptions Correlate with Higher Sales and Ratings?

```py
df['description_length'] = df['about_product'].apply(len)
df['description_sentiment'] = df['about_product'].apply(lambda x: TextBlob(x).sentiment.polarity)

# Correlation analysis
correlation_matrix = df[['description_length', 'description_sentiment', 'rating', 'rating_count']].corr()

# Plot heatmap
plt.figure(figsize=(10, 8))
sns.heatmap(correlation_matrix, annot=True, cmap='coolwarm')
plt.title('Impact of Product Descriptions on Sales and Reviews', fontsize=16)
plt.show()

# Scatter plot for description length vs. rating count
plt.figure(figsize=(12, 7))
sns.scatterplot(x='description_length', y='rating_count', data=df, color=amazon_blue, alpha=0.6)
plt.title('Description Length vs. Sales (Rating Count)', fontsize=16)
plt.xlabel('Description Length', fontsize=14)
plt.ylabel('Sales (Rating Count)', fontsize=14)
plt.grid(True)
plt.show()

# Scatter plot for description sentiment vs. rating
plt.figure(figsize=(12, 7))
sns.scatterplot(x='description_sentiment', y='rating', data=df, color=amazon_orange, alpha=0.6)
plt.title('Description Sentiment vs. Customer Ratings', fontsize=16)
plt.xlabel('Description Sentiment', fontsize=14)
plt.ylabel('Average Rating', fontsize=14)
plt.grid(True)
plt.show()
```

![alt text](plots/18df1ad3-f3fb-4502-8a00-20a64bc61b14.png)
![alt text](plots/58873257-4b3d-4218-ba9a-324981ad2c2d.png)
![alt text](plots/a4f81161-8b42-401f-8dee-ead3982be24e.png)

1. Correlation Heatmap: Impact of Product Descriptions on Sales and Reviews

	-	Correlation Values:
	-	Description Length vs. Sales (rating_count): There is a very weak negative correlation (-0.0082), indicating that longer descriptions do not significantly affect sales volume.
	-	Description Length vs. Ratings: The correlation between description length and ratings is also very weak (0.019), suggesting that the length of the product description does not have a significant impact on how customers rate the product.
	-	Description Sentiment vs. Sales (rating_count): A weak positive correlation (0.045) suggests that slightly more positive descriptions might lead to marginally higher sales, but the effect is not strong.
	-	Description Sentiment vs. Ratings: There is a slightly stronger positive correlation (0.027) between sentiment and ratings, indicating that more positively worded descriptions may lead to slightly higher customer ratings.
	-	Overall Interpretation:
	-	The correlations are generally weak, implying that other factors may be more significant in influencing sales and ratings than description length or sentiment alone.

2. Scatter Plot: Description Length vs. Sales (Rating Count)

	-	Scatter Pattern:
	-	The scatter plot shows a broad distribution of sales across different description lengths, with no clear pattern indicating that longer descriptions lead to higher sales.
	-	The majority of products have descriptions ranging between 0 and 500 characters, with some outliers in both low and high sales volumes.
	-	A few products with extremely high sales are scattered across various description lengths, but no clear trend emerges.
	-	Possible Interpretation:
	-	Efficient Information Delivery: It’s possible that concise descriptions (within the 0-500 character range) are sufficient to convey the necessary information, without the need for lengthy descriptions.
	-	Focus on Key Information: Instead of focusing on length, it may be more effective to ensure that the description contains relevant, concise information that addresses customer needs.

3. Scatter Plot: Description Sentiment vs. Customer Ratings

	1	Scatter Pattern:
	-	The plot shows that most products have a sentiment score ranging from 0 to 0.4, corresponding to slightly positive to moderately positive sentiments.
	2	Ratings are clustered around the 4.0 mark, with no clear linear relationship between sentiment and ratings. There are instances of high ratings (above 4.5) even with slightly negative sentiments.
	3	Products with highly positive sentiments (above 0.5) do not consistently achieve higher ratings, which suggests that overly positive descriptions do not guarantee better ratings.
	-	Possible Interpretation:
	-	Neutral to Positive Sentiment Suffices: Descriptions with neutral to moderately positive sentiment seem to be the most common and do not detract from customer ratings. Overly positive descriptions do not necessarily enhance customer perception.
	4	Authenticity Over Positivity: Consumers may value authenticity over highly positive language, so descriptions that accurately reflect the product’s qualities may be more effective in maintaining good ratings.

Business Implications:

1.	Focus on Content Quality Over Length:
	-	Given the weak correlation between description length and sales or ratings, the focus should be on providing clear, concise, and relevant information rather than extending the length unnecessarily.
	-	Prioritize essential product details, benefits, and features that directly address customer needs.
2.	Moderate Sentiment in Descriptions:
	-	While a moderately positive tone is beneficial, it’s crucial not to overdo it. Authenticity in product descriptions may resonate better with consumers, leading to trust and potentially better ratings.
3.	Further Exploration:
	-	It could be valuable to perform further analysis to identify specific words or phrases within the product descriptions that correlate strongly with high ratings or sales. This could involve more advanced text mining techniques or sentiment analysis.

Conclusion:

The analysis suggests that while product description length and sentiment have some impact, they are not the primary drivers of sales or ratings. Focus on providing valuable, authentic content that resonates with customer needs. Further exploration of specific language within descriptions could yield more actionable insights.

##
### 3. Which Product Categories Have the Highest Customer Satisfaction?
```py
category_highest_rating = df.groupby(['Level_1','Level_2','Level_3']).agg({
    'rating': 'mean'
}).reset_index()
category_highest_rating.sort_values(by='rating',ascending=False)

plt.figure(figsize=(12,6))
sns.barplot(data=category_highest_rating,y='Level_1',x='rating',color=amazon_orange)
plt.title('Average Customer Satisfaction by Category (Level_1)', fontsize=16)
plt.xlabel('Average Rating', fontsize=14)
plt.ylabel('Product Category', fontsize=14)
plt.grid(True)
plt.show()

plt.figure(figsize=(12,7))
sns.barplot(data=category_highest_rating,y='Level_2',x='rating',color=amazon_orange)
plt.title('Average Customer Satisfaction by Category (Level_3)', fontsize=16)
plt.xlabel('Average Rating', fontsize=14)
plt.ylabel('Product Category', fontsize=14)
plt.grid(True)
plt.show()

plt.figure(figsize=(14,12))
sns.barplot(data=category_highest_rating,y='Level_3',x='rating',color=amazon_orange)
plt.title('Average Customer Satisfaction by Category (Level_3)', fontsize=16)
plt.xlabel('Average Rating', fontsize=14)
plt.ylabel('Product Category', fontsize=14)
plt.grid(True)
plt.show()
```
![alt text](plots/e6a02bbf-c505-4dc6-9db2-a805738d468e.png)
![alt text](plots/baa69775-b348-4c63-8270-b167ad10853c.png)
![alt text](plots/a9a65775-b435-40be-97a1-3984fadc200d.png)


1. Average Customer Satisfaction by Category (Level_1)

	-	High Satisfaction Categories:
	-	Office Products and Musical Instruments have the highest average ratings, indicating strong customer satisfaction in these broad categories. This suggests that products in these categories are meeting or exceeding customer expectations.
	-	Health & Personal Care and Home & Kitchen also show high levels of satisfaction, making them key areas of strength.
	-	Lower Satisfaction Categories:
	-	Car & Motorbike and Home Improvement have the lowest average ratings among the Level_1 categories. This could indicate issues with product quality, customer expectations, or both in these categories.
	-	Implications:
	-	Focus on High-Performing Categories: Continue to leverage strengths in categories like Office Products and Musical Instruments through marketing and product expansion.
	-	Investigate Lower-Performing Areas: Further analysis into why Car & Motorbike and Home Improvement are lagging could reveal areas for improvement, such as product quality, pricing, or customer support.

2. Average Customer Satisfaction by Category (Level_3)

	-	High Satisfaction Subcategories:
	-	Office Electronics, Office Paper Products, and Tablets are the top-performing subcategories, indicating very high customer satisfaction. These subcategories likely offer products that meet specific, well-defined customer needs.
	-	Accessories and Components also show strong performance, reflecting well on the variety and quality offered in these detailed product areas.
	-	Lower Satisfaction Subcategories:
	-	Car Accessories and Microphones have some of the lowest average ratings among Level_3 categories, which could signal potential issues related to product quality, functionality, or customer service in these niches.
	-	Networking Devices and Monitors are also on the lower end of the satisfaction spectrum, which may reflect challenges in technology or customer expectations.
	-	Implications:
	-	Targeted Improvements: For subcategories like Car Accessories and Microphones, it may be worth diving deeper into customer reviews to identify specific issues that can be addressed through product redesigns, better quality control, or improved customer support.
	-	Leverage Top Subcategories: High-performing subcategories like Office Electronics and Tablets can be highlighted in marketing campaigns, promotions, or even used as benchmarks for improving other product lines.

Business Recommendations:

1.	Product Development and Quality Control:
	-	Invest in product development and quality control for lower-performing categories and subcategories, particularly Car & Motorbike, Home Improvement, and Car Accessories.
	-	Consider introducing new products or updating existing ones in high-satisfaction areas to capitalize on existing customer satisfaction.
2.	Customer Feedback and Experience:
	-	Collect and analyze more detailed customer feedback in the lower-rated categories to understand the specific pain points.
	-	Improve customer experience in these areas, possibly through enhanced customer support, clearer product descriptions, or better post-purchase services.
3.	Marketing and Promotions:
	-	Highlight and promote the top-performing categories and subcategories in marketing efforts to reinforce positive customer perceptions and drive sales in these areas.
	-	Use insights from high-performing categories to inform strategies in lower-performing ones, such as focusing on key features that customers value.
4.	Cross-Category Strategy:
	-	Consider cross-selling strategies that pair high-satisfaction products with lower-rated ones to potentially boost the perceived value of lower-performing products.
	-	Explore whether customer satisfaction trends differ by demographic or region to tailor product offerings more precisely.

Conclusion:

This analysis provides a clear picture of where customer satisfaction is highest and where there may be issues to address. By focusing on both leveraging high-performing categories and improving lower-rated ones, the business can work towards increasing overall customer satisfaction and driving growth.

##
### 4. What is the Relationship Between Product Ratings and Sales Volume?
```py
# Scatter plot for rating vs. sales volume
plt.figure(figsize=(12, 7))
sns.scatterplot(x='rating', y='rating_count', data=df, color='blue', alpha=0.6)
plt.title('Product Ratings vs. Sales Volume', fontsize=16)
plt.xlabel('Average Rating', fontsize=14)
plt.ylabel('Sales (Rating Count)', fontsize=14)
plt.grid(True)
plt.show()

# Correlation calculation
correlation, _ = spearmanr(df['rating'], df['rating_count'])
print(f'Spearman correlation between rating and sales volume: {correlation:.2f}')
```
![alt text](plots/67332042-55ca-4ebd-84e0-83650822bfc3.png)

Key Observations:

1.	Cluster Around High Ratings:
	-	The majority of products cluster around the 3.5 to 4.5 average rating range. This suggests that most products receive fairly positive ratings, indicating general customer satisfaction across the board.
2.	Sales Concentration:
	-	Higher sales volumes are primarily concentrated within the 4.0 to 4.5 rating range. Products with ratings in this range tend to have the highest sales volumes, with several products achieving exceptionally high sales.
	-	There are some outliers with very high sales even beyond this range, but these are less common.
3.	Lower Ratings and Sales:
	-	Products with ratings below 3.5 generally have much lower sales volumes, indicating that lower-rated products are less popular or less likely to drive high sales. This trend suggests that customer satisfaction, as reflected in product ratings, plays a significant role in sales performance.
4.	Limited Impact of Ratings Beyond a Threshold:
	-	There are relatively few products with ratings below 2.5 or above 4.5, and these extremes do not show a clear pattern in terms of sales. This indicates that while a moderate to high rating (around 4.0) is important, ratings beyond 4.5 do not necessarily lead to a proportional increase in sales.

Possible Interpretation:

-	Quality-Driven Sales:
-	The clustering of higher sales within the 4.0-4.5 rating range suggests that customers are more likely to purchase products that are perceived to have good quality. This aligns with the idea that customer reviews and ratings significantly influence purchasing decisions.
-	Threshold Effect:
-	There seems to be a threshold around the 4.0 rating mark, where products below this threshold experience diminishing returns in sales, while those above it are more likely to drive higher volumes. This could indicate that customers use a 4.0 rating as a benchmark for quality.
-	Outliers:
-	The few products with very high sales despite lower ratings could indicate niche markets or strong brand loyalty, where specific product features outweigh overall ratings.

Business Implications:

1.	Focus on Maintaining High Ratings:
	-	Ensuring that products consistently achieve ratings around or above 4.0 should be a priority, as this rating level is associated with higher sales volumes. This can be achieved through continuous product quality improvements and by addressing customer feedback effectively.
2.	Marketing Strategy:
	-	Highlight products that have ratings of 4.0 or higher in marketing campaigns, as these are more likely to attract customers and drive sales.
	-	For products with ratings below 3.5, consider investigating the causes of dissatisfaction and implementing targeted improvements or promotions to boost their appeal.
3.	Product Development:
	-	Use customer feedback and reviews to identify and address the factors that contribute to lower ratings. Improving these aspects could help lift sales for underperforming products.
4.	Customer Engagement:
	-	Engage customers who leave ratings around the 4.0 mark to understand what drives their satisfaction and to encourage them to provide detailed reviews, which can further help in refining product offerings.

Conclusion:

This analysis reinforces the importance of maintaining high product ratings to drive sales. The relationship between ratings and sales suggests that achieving and maintaining an average rating around 4.0 or higher can significantly impact sales performance. Products that fall below this threshold should be carefully reviewed and improved to avoid losing market share.

##
### 5. How Effective are Promotional Strategies in Driving Sales?

```py
df['roi'] = (df['rating_count'] * df['discounted_price']) / df['actual_price'] 

# Group by discount percentage
promotion_roi = df.groupby('discount_percentage').agg({'roi': 'mean'}).reset_index()

# Plot ROI by discount percentage
plt.figure(figsize=(16,7))
sns.barplot(x='discount_percentage', y='roi', data=promotion_roi, palette='viridis')
plt.title('ROI of Promotions by Discount Percentage', fontsize=16)
plt.xlabel('Discount Percentage', fontsize=14)
plt.xticks(np.arange(0, len(promotion_roi['discount_percentage']), step=5)) 
plt.ylabel('ROI', fontsize=14)
plt.show()
```

![alt text](plots/88c23688-c1da-478b-8e9b-0f1d64cbafba.png)

Key Observations:

1.	High ROI at Specific Discount Levels:
	-	There is a significant spike in ROI around the 5% and 10% discount levels, with the highest ROI occurring at approximately 6%. This suggests that minimal discounts can generate substantial returns, likely because they are enough to incentivize purchases without drastically reducing profit margins.
	-	Other notable spikes occur around the 15-20% and 30-35% discount ranges, indicating additional “sweet spots” where discounts are effective in driving profitable sales.
2.	Diminishing ROI Beyond Certain Discount Levels:
	-	As the discount percentage increases beyond 40%, the ROI generally diminishes. There are few exceptions, such as at approximately 66%, but overall, higher discounts tend to yield lower returns.
	-	Discounts above 70% show a sharp decline in ROI, indicating that excessive discounts may not be sustainable or profitable.
3.	Scattered ROI Distribution:
	-	The ROI values are spread across various discount levels, but the general trend suggests that lower to moderate discounts (between 5% and 40%) are more effective in generating higher returns compared to deeper discounts.

Possible Interpretation:

-	Optimal Discounting Strategy:
-	The data indicates that lower to moderate discounts, particularly around 5-10% and 15-20%, provide the best ROI. These levels may strike the right balance between offering value to customers and maintaining profit margins.
-	Risk of Over-Discounting:
-	High discounts (above 50%) may attract more sales volume but do so at the expense of profitability. The steep decline in ROI at higher discount levels suggests that such promotions should be used cautiously and possibly limited to clear inventory or target highly price-sensitive customers.
-	Targeted Promotions:
-	The spikes at specific discount levels suggest that targeted promotions with these discount rates could be strategically deployed to maximize ROI during sales campaigns or special events.

Business Implications:

1.	Strategic Discounting:
	-	Focus on offering discounts in the 5-20% range for regular promotions, as these have been shown to generate high ROI. Consider using higher discounts sparingly, only in situations where clearing inventory or achieving specific sales targets is necessary.
2.	Profitability Analysis:
	-	For products or categories that consistently require deeper discounts to drive sales, reassess the pricing strategy, cost structure, or market positioning to ensure long-term profitability.
3.	Promotion Planning:
	-	Use these insights to plan promotional campaigns that maximize ROI. For example, running a 5-10% discount promotion during high-traffic periods could yield significant returns without drastically cutting into profit margins.
4.	Inventory Management:
	-	Apply deeper discounts strategically for inventory management purposes, ensuring that even when higher discounts are offered, they are aligned with the overall profitability goals of the business.

Conclusion:

The analysis highlights that not all discounts are equally effective in driving profitable sales. By focusing on the optimal discount ranges identified, businesses can enhance their promotional strategies to achieve better financial outcomes while still attracting customers. Caution should be exercised with high discount levels, as they often result in lower ROI.

##
### 6. How Do Product Categories Perform in Terms of Price Sensitivity?

```py
import statsmodels.api as sm

# Regression model to predict sales based on price
X = df[['actual_price', 'discounted_price']]
X = sm.add_constant(X)
y = df['rating_count']

model = sm.OLS(y, X).fit()
print(model.summary())

# Plot price sensitivity across categories
plt.figure(figsize=(12, 7))
sns.scatterplot(x='discounted_price', y='rating_count', hue='Level_1', data=df, palette='coolwarm', alpha=0.6)
plt.title('Price Sensitivity Across Categories [Level 1]', fontsize=16)
plt.xlabel('Discounted Price', fontsize=14)
plt.ylabel('Sales (Rating Count)', fontsize=14)
plt.grid(True)
plt.legend(title='Category',bbox_to_anchor=(1.05, 1), loc=2, borderaxespad=0.)
plt.show()

plt.figure(figsize=(12, 7))
sns.scatterplot(x='discounted_price', y='rating_count', hue='Level_2', data=df, palette='coolwarm', alpha=0.6)
plt.title('Price Sensitivity Across Categories [Level 2]', fontsize=16)
plt.xlabel('Discounted Price', fontsize=14)
plt.ylabel('Sales (Rating Count)', fontsize=14)
plt.grid(True)
plt.legend(title='Category',bbox_to_anchor=(1.05, 1), loc=2, borderaxespad=0.)
plt.show()

plt.figure(figsize=(12, 7))
sns.scatterplot(x='discounted_price', y='rating_count', hue='Level_3', data=df, palette='coolwarm', alpha=0.6)
plt.title('Price Sensitivity Across Categories [Level 3]', fontsize=16)
plt.xlabel('Discounted Price', fontsize=14)
plt.ylabel('Sales (Rating Count)', fontsize=14)
plt.grid(True)
plt.legend(title='Category',bbox_to_anchor=(1.05, 1), loc=2, borderaxespad=0.)
plt.show()
```
```
                            OLS Regression Results                            
==============================================================================
Dep. Variable:           rating_count   R-squared:                       0.001
Model:                            OLS   Adj. R-squared:                 -0.000
Method:                 Least Squares   F-statistic:                    0.7997
Date:                Sun, 25 Aug 2024   Prob (F-statistic):              0.450
Time:                        21:07:15   Log-Likelihood:                -14121.
No. Observations:                1194   AIC:                         2.825e+04
Df Residuals:                    1191   BIC:                         2.826e+04
Df Model:                           2                                         
Covariance Type:            nonrobust                                         
====================================================================================
                       coef    std err          t      P>|t|      [0.025      0.975]
------------------------------------------------------------------------------------
const             1.487e+04   1067.098     13.932      0.000    1.28e+04     1.7e+04
actual_price        -0.2736      0.352     -0.777      0.438      -0.965       0.418
discounted_price     0.2778      0.588      0.472      0.637      -0.876       1.432
==============================================================================
Omnibus:                     1396.044   Durbin-Watson:                   1.798
Prob(Omnibus):                  0.000   Jarque-Bera (JB):           124006.948
Skew:                           5.957   Prob(JB):                         0.00
Kurtosis:                      51.484   Cond. No.                     1.41e+04
==============================================================================

Notes:
[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
[2] The condition number is large, 1.41e+04. This might indicate that there are
strong multicollinearity or other numerical problems.

```
![alt text](plots/1a72441d-a2d7-41ad-9e11-2be76d9fe742.png)
![alt text](plots/7778a71e-9c06-4503-935c-40228e203ba0.png)
![alt text](plots/f3b8704a-0fe4-474f-94fa-6f2f10424f66.png)


1. Price Sensitivity Across Categories (Level 1)

-	Sales Concentration at Lower Price Points:
	-	Most sales are concentrated at lower discounted price points, generally below 10,000 units of currency. This trend is consistent across all categories, indicating that lower-priced products tend to sell in higher volumes.
	-	Computers & Accessories and Electronics categories show some of the highest sales volumes at these lower price points, suggesting strong demand for affordable tech products.
-	Diminished Sales at Higher Prices:
	-	As discounted prices increase beyond 10,000, the number of sales drops sharply across all categories. This trend suggests that higher-priced products are less price-sensitive, meaning consumers may be more selective or price-conscious at these higher price points.
-	Category Specificity:
    -	Categories such as Home & Kitchen and Office Products also show significant sales, but generally at lower price points. These products may have a broader appeal or meet essential needs, driving sales despite their lower price sensitivity.

2. Price Sensitivity Across Categories (Level 2)

-	Detailed Subcategory Analysis:
-	The scatter plot at Level 2 reveals more granular insights into specific subcategories. For instance, subcategories like Accessories & Peripherals and Headphones, Earbuds & Accessories exhibit high sales at lower price points, reinforcing the trend seen at Level 1.
-	Subcategories related to technology (e.g., Networking Devices, Laptops) show a wider range of price points, but sales are still concentrated at the lower end, indicating that even within tech, affordability drives volume.
-	Price Insensitivity at Higher Price Points:
-	Higher-priced subcategories, such as Home Theater, TV & Video, have fewer data points at higher prices, suggesting that these products might appeal to a niche market that is less sensitive to price or that such products are simply less frequently purchased.

3. Price Sensitivity Across Categories (Level 3)

-	Highly Specific Product Types:
-	At Level 3, the scatter plot reveals the price sensitivity of very specific product types. For instance, Cables & Accessories and Memory Cards show very high sales at lower price points, indicating that these are highly price-sensitive items.
-	Laptops and Smartphones show sales across a broader range of prices but still follow the general trend of higher sales at lower prices.
-	Low Sales at Premium Price Points:
-	Products with higher discounted prices, such as Traditional Laptops and Televisions, show significantly lower sales, reinforcing the trend that high-price items sell less frequently.

Business Implications:

1.	Focus on Affordable Product Lines:
	-	Given that most sales occur at lower price points, consider focusing on product lines that can be offered at affordable prices. This strategy is especially relevant for categories like Computers & Accessories, Electronics, and Home & Kitchen.
2.	Targeted Discounts:
	-	For higher-priced items, consider targeted discount strategies that make these products more accessible without significantly eroding profit margins. Special promotions or bundles might help boost sales in these categories.
3.	Differentiation in Premium Segments:
	-	For products in the premium price range, differentiation through quality, brand reputation, and added value (e.g., warranties, exclusive features) may help drive sales despite the lower volume.
4.	Category-Specific Strategies:
	-	Different categories and subcategories show varying levels of price sensitivity. Tailoring pricing strategies to the specific characteristics of each category can optimize sales and profitability.
5.	Inventory and Marketing Strategies:
	-	Inventory management should consider the high turnover rate of lower-priced items, ensuring that these products are always in stock. Marketing efforts should highlight the affordability and value of these high-demand products.

Conclusion:

This analysis shows a clear trend of higher sales volumes at lower price points across most categories, indicating strong price sensitivity among consumers. Strategic pricing, particularly for higher-priced items, will be crucial in balancing sales volume with profitability. Tailored approaches for different categories will maximize the effectiveness of promotions and pricing strategies.

##
### 7. What is the Impact of User Reviews on Product Success?

```py
# Sentiment analysis on review content
df['review_sentiment'] = df['review_content'].apply(lambda x: TextBlob(x).sentiment.polarity)

# Correlation analysis
correlation_matrix = df[['review_sentiment', 'rating', 'rating_count']].corr()

# heatmap for correlations
plt.figure(figsize=(10, 8))
sns.heatmap(correlation_matrix, annot=True, cmap='coolwarm')
plt.title('Impact of User Reviews on Product Success', fontsize=16)
plt.show()

# Scatter plot for review sentiment vs. sales
plt.figure(figsize=(12, 7))
sns.scatterplot(x='review_sentiment', y='rating_count', data=df, color='red', alpha=0.6)
plt.title('Review Sentiment vs. Sales Volume', fontsize=16)
plt.xlabel('Review Sentiment', fontsize=14)
plt.ylabel('Sales (Rating Count)', fontsize=14)
plt.grid(True)
plt.show()
```
![alt text](plots/009db6fb-2057-45bb-a44e-f9330f03a10a.png)
![alt text](plots/4636601f-e9e1-4e03-9d9d-fd0a04e79a30.png)

Correlation Heatmap Analysis

1.	Review Sentiment vs. Rating:
	-	There is a positive correlation (0.19) between review sentiment and product ratings. This suggests that as the sentiment of reviews becomes more positive, the average product rating tends to increase, although the relationship is relatively weak.
2.	Review Sentiment vs. Rating Count (Sales Volume):
	-	The correlation between review sentiment and rating count is slightly negative (-0.021), indicating that the sentiment of reviews does not have a strong direct relationship with the number of sales. This suggests that even products with neutral or slightly negative reviews can still generate significant sales, possibly due to factors like brand loyalty or product features.
3.	Rating vs. Rating Count:
	-	There is a small positive correlation (0.094) between product ratings and rating count, indicating that higher-rated products tend to have slightly higher sales volumes. However, this correlation is weak, suggesting that other factors (such as price, brand, or promotion) may play a more significant role in driving sales.

Scatter Plot Analysis: Review Sentiment vs. Sales Volume

1.	Cluster Around Neutral Sentiment:
	-	The majority of sales are concentrated around products with neutral to slightly positive review sentiments (around 0.0 to 0.4). This suggests that extreme sentiments (either highly negative or highly positive) are less common, and moderate reviews dominate the landscape.
2.	High Sales with Moderate Sentiment:
	-	Products with the highest sales volumes often have review sentiments around 0.2 to 0.4, indicating that a moderately positive sentiment is sufficient to drive significant sales. This could imply that consumers are more influenced by a balanced perspective than by overwhelmingly positive or negative reviews.
3.	Low Sales with Negative Sentiment:
	-	Products with negative sentiments (below 0.0) tend to have much lower sales volumes, which is expected. This trend suggests that negative reviews can deter potential buyers, reducing overall sales.

Business Implications:

1.	Importance of Managing Customer Reviews:
	-	Given the positive correlation between review sentiment and product ratings, it’s essential to manage customer reviews effectively. Encouraging satisfied customers to leave positive reviews can help improve the product’s overall rating, which may lead to slightly higher sales.
2.	Balanced Review Sentiment:
	-	The concentration of high sales around neutral to slightly positive review sentiments suggests that consumers value balanced and authentic reviews. Brands should focus on maintaining a realistic portrayal of their products in reviews, as overly positive reviews might be perceived as inauthentic.
3.	Addressing Negative Reviews:
	-	Since negative reviews are associated with lower sales volumes, addressing the concerns raised in these reviews is crucial. Providing responsive customer support and resolving issues quickly can help mitigate the impact of negative reviews.
4.	Leveraging Reviews in Marketing:
	-	Highlighting positive reviews in marketing materials, especially those that provide a balanced view, can build trust with potential buyers and encourage purchases. Consumers often rely on reviews as a key decision-making factor, so showcasing authentic, positive feedback can be a powerful tool.

Conclusion:

This analysis underscores the significant but nuanced role that user reviews play in product success. While positive sentiment in reviews correlates with higher ratings, the relationship with sales is more complex, with many factors at play. Managing reviews effectively and addressing negative feedback can help improve both product ratings and sales performance.

##
### 8. How Do High-Value Products Compare Across Categories?

```py
# Filter for high-value products
high_value_products = df[df['actual_price'] > df['actual_price'].quantile(0.9)]  # Top 10% by price

# Compare sales and ratings across categories
high_value_summary = high_value_products.groupby('Level_1').agg({
    'rating_count': 'sum',
    'rating': 'mean'
}).reset_index()

# Plot high-value products performance
plt.figure(figsize=(10, 7))
sns.barplot(y='rating', x='Level_1', data=high_value_summary, palette='magma')
plt.title('Performance of High-Value Products Across Categories', fontsize=16)
plt.xlabel('Average Rating', fontsize=14)
plt.ylabel('Product Category', fontsize=14)
plt.grid(True,linestyle='--')
plt.show()
```

![alt text](plots/5c9bb3c4-56de-4bd3-bb6e-d8402048c831.png)

Key Observations:

1.	Consistently High Ratings Across Categories:
	-	All three categories—Computers & Accessories, Electronics, and Home & Kitchen—show average ratings above 4.0, indicating that high-value products in these categories are generally well-received by customers. This suggests a high level of customer satisfaction for premium products across these segments.
2.	Slight Variation in Ratings:
	-	The Home & Kitchen category appears to have the highest average rating, slightly edging out Electronics and Computers & Accessories. This could indicate that high-value products in the Home & Kitchen segment might meet or exceed customer expectations more consistently than those in the other two categories.
3.	Customer Trust in High-Value Products:
	-	The high ratings across all categories suggest that customers trust the quality and value of premium products, which is crucial for maintaining brand loyalty and encouraging repeat purchases.

Business Implications:

1.	Focus on High-Value Segments:
	-	Given the high average ratings, it is clear that customers appreciate high-value products across these categories. The business should continue to focus on maintaining the quality and performance of these products to sustain high levels of customer satisfaction.
2.	Leverage Positive Customer Feedback:
	-	The consistently high ratings can be leveraged in marketing strategies. Highlighting customer satisfaction with high-value products can attract more buyers who are looking for quality and reliability, especially in the competitive segments of Electronics and Home & Kitchen.
3.	Product Development and Enhancement:
	-	While the ratings are high, there’s always room for improvement. Gathering detailed customer feedback can help identify specific areas where these products excel and where there is potential for enhancement, ensuring that the business continues to meet or exceed customer expectations.
4.	Strategic Pricing and Promotions:
	-	Given the positive reception of high-value products, the business might explore strategic pricing that reflects the premium quality while still appealing to a broad customer base. Promotions that highlight the superior ratings of these products can also be effective in driving sales.

Conclusion:

This analysis shows that high-value products across Computers & Accessories, Electronics, and Home & Kitchen categories are well-regarded by customers, as reflected in their strong average ratings. Maintaining the quality and performance of these products is key to sustaining customer satisfaction and driving continued success in these segments.

## Recommendations:

-	Discounting Strategy: Focus on discounts within the 40-60% range to maximize sales and maintain product ratings. Use deeper discounts selectively to clear inventory or boost sales for specific product lines.
-	Enhanced Product Descriptions: Prioritize clear, concise, and relevant information in product descriptions, and ensure the content is authentic to build customer trust.
-	Leverage High-Satisfaction Categories: Continue to develop and promote products in categories with high customer satisfaction. Explore opportunities to cross-sell with lower-rated products to enhance their perceived value.
-	Review Management: Actively manage customer reviews to maintain high product ratings and address negative feedback promptly to prevent it from affecting sales.
-	Targeted Promotions: Optimize promotional strategies by focusing on discount ranges that provide the best ROI and are aligned with overall profitability goals.

## Conclusion:

The insights from this analysis can serve as a valuable resource for businesses looking to refine their e-commerce strategies on platforms like Amazon. By leveraging these findings, companies can better align their offerings with customer expectations, optimize pricing and promotions, and ultimately drive growth in a competitive market.

## Feedback Request:

Thank you for taking the time to review this project! Your feedback is crucial for my continuous improvement. Please share any thoughts, suggestions, or areas where I can enhance this analysis. Your insights will help me refine my approach and deliver even better outcomes in future projects. Thank you!