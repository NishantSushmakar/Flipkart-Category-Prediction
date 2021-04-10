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

- Checked For the NULL Values in Description, two Rows don't have a description and by checking on the flipkart site also i didn't find any description on them so i drop those two rows because they won't affect the performance significantly.

<img src =https://github.com/NishantSushmakar/Flipkart-Category-Prediction/blob/main/images/img2.PNG>

- The Columns that look relevant for the problem statement were only taken into account and a new dataframe was created using *description* and *Category of Product*
- The *Cateogry of Product* posed a severe problem because some of the category tree contained only the product name and were not mentioned properly for eg : *Rasav Jewels Yellow Gold Diamond 18 K Rin* . Now one can figure out this product belongs to the category of Jewellery so i observed different categories same as mentioned in the example and found out the keywords which will allow me to identify those category into one main category , this was important as it help to reduce the number of classes present in the category.
- After checking these categories and appending them in the main categories there were still some categories left which could not be identified to which category to assign to so all the categories with less than equal to 84 value count were merged into one main category called **Miscellaneous**. Finally I was left with 19 Classes to predict which were earlier more than 200.

## Text Preprocessing
- While observing the descriptions through the help of visualizations i found out that there were some irrelvant words such as 'rs','flipkartcom','free','shipping','product','genuine','delivery','flipkart','cash','best','price','detail','buy','day','relpacement','guarantee' ,  these words were included in my custom stopwords to be removed from the text.
- First step was to remove any contractions in the words such as didn't => did not and for that a contraction library in python was used.
- Seond step was to remove all the html tags and links present in the description 
- Third step was to lowercase every word in the description as **WORD** and **word** would be considered two different words during the vectorisation even though they are the same. 
- Removed all the digits from the description because they won't provide any type of value to our descriptions.
- Removed all stopwords present in the custom stopwords list i made and also what is already present in the spacy stopwords vocab.
- Regular expression were used to remove all the punctuations , extra white spaces , extra newline characters and extra tabs in the descriptions.
- After Cleaning the Description Lemmatization was done overall the words instead of Stemming as Lemmatization keeps the words which are present in the dictionary and have a meaning instead of stemming it aggressively cut shorts the word to one without meaning.Lemmaitzation was done by making the text into a spacy object and lemma was then extracted from it.<br>

After Lemmatization the vocab size was 23042 (i.e 23042 unique words in the corpus).<br>

## Evaluation Metric
Predicting 19 Categories is a Multi class problem and Measuring the effectiveness of the model was done through *F1 Macro Score* which Averages with equal weight to f1 score of every class because it gives equal importance of prediction of each class , *accuracy* can be misleading in this problem statement with imbalanced categories as it can predict the majority accuracy correctly and leaving the minority classes and giving a higher accuracy in such cases.

## Validation Strategy 
<img src=https://github.com/NishantSushmakar/Flipkart-Category-Prediction/blob/main/images/classes.PNG><br>
As the Category of Product were skewed (i.e imbalanced) Stratified K fold cross validation seemed more relevant as the random cross validation can choose all the majority classes in one fold giving a false representation of the population while training and testing. K was choosen to be 5 as 20% of data could be validated upon.

# Experiments

## 1. Applying Machine Learning Algorithms with Bag Of Words 
Vectorisation of Bag of Words gave an array with feature of 23042 , due to the problem of curse of dimensionality i reduced the dimension of features to 1600 using PCA(principal component analysis) after normalizing it because it almost preserved 90% of information. <br>
The Supervised classification algorithm such as Random Forest,KNN,Gaussian Naive Bayes,Logistic Regression, Support Vector Machines.<br>
Results :<br>
<img src=https://github.com/NishantSushmakar/Flipkart-Category-Prediction/blob/main/images/exp1train.PNG><br>
The fold score in the table are calculated in such a way that if fold 1 is stated then fold 1 will be taken for testing(validation score) and rest all the folds are used for the training score.<br>
<img src=https://github.com/NishantSushmakar/Flipkart-Category-Prediction/blob/main/images/exp1val.PNG><br>

SVM clearly is much effective in classification than other algorithms with 0.939 F1 Score and then with slight difference Logistic Regression also can be effective algorithm for prediction, Gaussian Naive Bayes performed poorly on the data as it has strong assumption of normal distribution on features where as knn performed also good but not above 0.88 F1 score , random forest overfits the data.








