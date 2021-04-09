# Flipkart-Category-Prediction

## About Dataset
* The Dataset Contains 20,000 Datapoints with 15 features.
* The features were *uniq_id*, *crawl_timestamp*, *product_url*, *product_name*,
*product_category_tree*, *pid*, *retail_price*, *discounted_price*,*image*, *is_FK_Advantage_product*, *description*, *product_rating*,
*overall_rating*, *brand*, *product_specifications*.
* The dataset does not contain the category for the product but has product_category_tree which can be used to extract the category.
* Main Feature of the Dataset for prediction is Description.

## Data Preprocessing
- Extracted the product category from the product_category_tree<br>
The first string before the '>>' will be used as the target variable(Product Category)<br>
For Eg: "Footwear >> Women\'s Footwear >> Casual Shoes >> Boots" **Footwear was extracted as the category**<br>
- But there were some Category tree such as **Eternal Gandhi Super Series Crystal Paper Weight...** which don't have a specific category on the site also
<img src=https://github.com/NishantSushmakar/Flipkart-Category-Prediction/blob/main/images/Capture.PNG>

- Checking For the NULL Values in Description 2 Rows don't have a description and by checking on the flipkart site also i didn't find any description on them so i drop those two rows because they won't affect the performance significantly.

<img src =https://github.com/NishantSushmakar/Flipkart-Category-Prediction/blob/main/images/img2.PNG>

- The Columns that look relevant for the problem statement were only taken into account and a new dataframe was created using *description* and *Category of Product*
