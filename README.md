# C4ADS Internship Project
## Dual Use Nuclear Fuel Cycle Item Detection

**Members Involved in the Project**

C4ADS’s Program Director of Data & Technology: **Patrick Baine**

Team Project Lead: **Tim Dill**

Data Scientists: | **Tyler Russin** | **Emma Rose** | **Jan Jaap de Jong** | **Baisali Sant** |

### The C4ADS Organization

C4ADS is a nonprofit organization dedicated to providing data-driven analysis and evidence-based reporting on global conflict and transnational security issues.

They specialize in using cutting-edge technologies to manage, integrate, and analyze disparate data from diverse languages, regions, and sources, incorporating their own field research from conflict zones and fragile states. C4ADS seeks to engage with local and international audiences and produce compelling analyses on conflict and security issues. In doing so, they fill a critical gap left by the traditional public sector and profit-driven institutions.

The C4ADS website can be found [Here](https://c4ads.org)

**The Study**

Over two years, the Nuclear Threat Initiative (NTI) and the Center for Advanced Defense Studies (C4ADS) worked together on a pilot project to demonstrate the viability of using PAI and machine learning to detect high-risk and/ or illicit nuclear trade.

The work showed that automated data preparation could save hundreds of analyst hours and help identify twice as many potentially high-risk entities as previous manual efforts. In addition, when applied to a baseline study of more than four million records, machine learning techniques could identify 50 new leads for further review. During the two-year study, at least ten entities identified through these approaches were added to a U.S. government export control list, demonstrating that novel analytic approaches to PAI can produce law enforcement-relevant insights.

The work done during this internship has been saved to C4ADS's private GitHub. However, The findings from our study on detection of high-risk and/ or illicit nuclear trade have been written into a research paper that can be found [Here](https://github.com/tylerrussin/C4ADS-Research-Paper/blob/main/Finalized%20C4ADS%20Paper.pdf)

C4ADS has continued its research after our contributions during the internship and has published a paper on the most recent findings. The official report can be found [Here](https://c4ads.org/signals-in-the-noise)

## Notable Journal Entries Discussing Challenges During the Internship

**Labs Internship C4ADS Journal: Entry One**

March 17th, 2020

**Abstract**

The following contains information about current challenges we face in nuclear dual-use item detection and ideas for handling some of those challenges. The proposed concepts below are by no means proven nor do they claim to be optimal.

**First Objective: Define a Baseline Model (Top Priority)**

As it stands now we have all the means to create a baseline model given the tagged and untagged data we currently have access to. I believe it is in our best interest to construct the basic systems needed for this baseline model to start processing our data. This includes constructing a dataset consisting of the tagged data and a subsample of the untagged data. A barebones pipeline to clean the data and label the data simply based on being either tagged or not tagged. This pipeline would need to include simple imputing, removing of nan values, either one hot or categorical encoding, and a baseline model. 

Recommended Baseline Models:
 
-	Decision Tree Classifier 
-	Random Forest Classifier 

**Dissecting the Tagged Data** 

Much data cleaning, merging, and feature engineering can and will be done to improve the baseline model in the future. However, the priority has to be perfecting the target data which at the moment exist almost exclusively in the tagged dataset. As it stands the tagged data is only tagged by company identification numbers. The problem with this is a company can have many trade transactions. Some transactions may be involved in the dual-use nuclear cycle and some may not be. For example, a company such as Toyota might have one transaction that consists of windshield wipers and another separate transaction that consists of aluminum piping(a dual-use item). With how the tagged data stands right now both these transactions would be labeled as dual-use nuclear items due to their association with a common company. Our goal should be to break down the tagged dataset so that only the transaction consisting of aluminum piping is labeled as being involved in the dual-use nuclear cycle. Below are several potential solutions to solve this task. 

**Clustering: Finding the Patterns**

One solution is to try several different clustering techniques to try to split the tagged data into separate categories and then to manually analyze the clusters seeing if any are associated with dual-use trades. Clusters could be based on a variety of different columns such as company, number of shipments, cargo weight, and more. And everything would likely be analyzed based on the product description category.

**Brute Force: Manual Labeling**

At first, this might seem like an idea to not consider due to its tedious and time-consuming nature. However, if done correctly it could create a very strong foundation for any predictive models built from it. We have about 16,000 rows of tagged data. If we decided to manually label all 16,000 rows. We could split it between the four of us and we could likely label all of the data within a day or two. We would be labeling based on reading the product description and searching for dual-use items. Once labeled we could train a model that searched the rest of our data. We could then again manually label the newly tagged transaction that the model predicts. 

**Brute Force: Manual Labeling Modified for Automation**

One simplification would be to manually label the tagged data and as we are manually labeling keep track of keywords that we associate with dual-use transactions. Once the four of us collectively build up a strong list of keywords we could then write some simple code to search the rest of the tagged data for those keywords, slightly automating the labeling process. 

**Unique Words: Full Automation**

One way to potentially bypass the manual labeling process and still keep many of the benefits of manually labeling would be to do as follows. First, run through the entirety of the tagged data and produce a list of unique words found in the product descriptions(being sure to ignore certain stop words to minimize noise). Once we have a list of all the unique words found in the product description we could either. Label the tagged data as dual-use or not based on the value counts of the words. Assuming that higher value counts are more or less dual-use. Alternatively, we could manually look through the list of unique words and manually create a new list of words that we believe are involved in dual-use. We could then automatically label all of the tagged data based on if the product description contains words that are within our created list. This is essentially a big filter. It would not use any data science techniques, but it could potentially be the solution to our problem.

**Important Side Note Relating to Above Methods: Two Targets**

It might be wise to be labeling the tagged data in two ways: one column as dual-use transactions and another column as a pure nuclear transaction. So a transaction with aluminum pipes would be labeled as dual-use but not as nuclear use and something like uranium ore would be labeled as both dual-use and nuclear use. This could greatly help the accuracy of future models we build and give us extra room for exploration. 

**Important Side Note: Using a List of Keywords**

Important to note that with any of the above methods where some kind of list of keywords is made we could technically determine all dual-use trades by just filtering out if a trades product description contains any of our predefined keywords. This could be pretty accurate in finding transactions with dual-use items. However, solving our problem that way only allows transactions to be determined as dual-use or not based on product description and that might not be enough. Also, it might limit our potential since there could be hidden patterns in other features in our data that a predictive model might be able to pick up on. 

List Side Note Part Two

It should also be noted that once we have a list of keywords we are no longer limited to just labeling tagged data. We could apply our filtration to samples of other data in our master dataset to provide our models with more accurate data to predict.

**Labs C4ADS Journal: Entry Two**

March 19th, 2020 

**Results from Journey Entry One**

The goals described in entry one of this journal have been explored. The accomplished objectives were as follows. 

-	Created a baseline random forest classifier model
-	Dissected tagged data using unique words: full automation method
-	Two targets have been created: not obvious, and dual-use

Of the accomplished objectives, a baseline was defined and created. It scored 60% with an accuracy metric. However, Due to the unbalanced nature of our dataset using accuracy as our classification metric is not possible. The solution is to use a variety of other metrics such as ROC AUC scores. Doing work with this will be the next task.

As far as dissecting the tagged data we successfully used the unique word method proposed in journal entry one. The result was a list of dual-use keywords that we defined manually and a modified tagged dataset that includes a new column that labels the row as a dual-use transaction or not.

Two targets have been created: a not obvious list of words that are words that hinted at dual-use items but were either too broad or had too many other common industrial uses, and a dual-use list that includes very limited words that almost definitely would be involved in a nuclear transaction.

**Goals described in entry one not accomplished**

-	Clustering finding patterns
-	Brute force
-	Brute force modified
-	Exploration in using lists to find more tagged data in master data

Clustering and Brute force methods were not attempted. The current results we have from the unique word approach have created the further need for experimentation with these concepts. 

For exploration in using lists to find more tagged data in the master data file. The more data we feed our model the more computational cost and engineering that has to be done to handle data processing. More data means things like memory errors and long training times.

**Quick Note**

Moving forward the unique word: full automation concept will be referred to as a filter and/or our filtration method that is used to label rows. 

**Two Paths to Take Using the Dissected Tagged Data**

With the new dataset, we have transactions that we are almost certain are involved in the dual-use nuclear fuel cycle. However, we are only certain because we are extremely specific on our filtration. The filtration method is likely missing out on other dual-use transactions. Now our final future predictive model can live with this problem and likely still be performant, but proposed are two possibilities that if explored could boost a future model’s performance by improving the data labeling technique. 

**Improve labeling: Exploration with clustering**

One solution is to use clustering. We can look at our flagged transactions from our filtration. Then, analyze these specific transactions with different types of unsupervised learning methods likely clustering and see if any patterns emerge that are mostly only seen in our flagged transactions. If we can find these patterns we can do a search through the lab's 18 tagged datasets and see if our original filtration missed anything. At this point, we will have accomplished the original goal and we can use the result of this clustering to boost the accuracy of future models due to the improved training data.

Also, depending on how performate this theoretical clustering technique/model is it could be our final model.

**Improve labeling: Exploring with Supervised Learning**

Currently, we have a dataset of 54,000 rows. Of these rows, about five to six thousand are labeled as dual-use nuclear transactions the rest are not. A classification model could be trained on this dataset. The model would then learn more complex patterns that are associated with dual-use transactions. We could then have it predict on the data it was trained on and see if it predicts any new transactions or we could train it on a subset of new data and gather its predictions. Then those predictions would be our new labeled data that our future predictive models could train on.

**Experiments Between Dual-Use List and Not Obvious List**

A decision to use the Dual-Use List or the Not Obvious List has to be made. All words in the Dual-Use List are in the Not Obvious List, but not all words in the Not Obvious List are in the Dual-Use List.

**Dual-Use List**

The idea behind and nature of this list is to house the most specific possible words that are dead giveaways to any dual use transactions. To accomplish this we are extremely specific with our word choice. As a consequence of this, any filtration with this list will likely leave out some dual-use transactions.

**Not Obvious List**

By design this list contains words that are more common. When filtration is done with this list it flags dual-use transitions on a broader level. It succeeds at capturing flagged transactions but it has a much higher risk of falsely labeling a non-dual-use transaction as one. As a consequence having more falsely labeled data will affect any future predictive model’s ability to find patterns and make predictions.

**Tasks that need to be done**

-	Have to integrate new evaluation metrics on the baseline model 
-	Start creating a system for bulk testing different predictive models 
-	Perform several of the proposals discussed above
-	Growing the base training data set with additional feature engineering and other datasets found in C4ADS S3 Buckets
