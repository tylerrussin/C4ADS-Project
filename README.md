# C4ADS Internship Project
## Dual Use Nuclear Fuel Cycle Item Detection

**Members Involved in the Project**

C4ADS’s Program Director of Data & Technology: **Patrick Baine**

Team Project Lead: **Tim Dill**

Data Scientists: | **Tyler Russin** | **Emma Rose** | **Jan Jaap de Jong** | **Baisali Sant** |

### The C4ADS Organization

C4ADS is a nonprofit organization dedicated to providing data-driven analysis and evidence-based reporting on global conflict and transnational security issues.

They specialize in using cutting-edge technologies to manage, integrate, and analyze disparate data from diverse languages, regions, and sources, incorporating their own field research from conflict zones and fragile states. C4ADS seeks to engage with local and international audiences and produce compelling analyses on conflict and security issues. In doing so, they fill a critical gap left by the traditional public sector and profit-driven institutions.

The C4ADS website can be found [Here](https://c4ads.org/)

**The Study**

Over two years, the Nuclear Threat Initiative (NTI) and the Center for Advanced Defense Studies (C4ADS) worked together on a pilot project to demonstrate the viability of using PAI and machine learning to detect high-risk and/ or illicit nuclear trade.

The work showed that automated data preparation could save hundreds of analyst hours and help identify twice as many potentially high-risk entities as previous manual efforts. In addition, when applied to a baseline study of more than four million records, machine learning techniques could identify 50 new leads for further review. During the two-year study, at least ten entities identified through these approaches were added to a U.S. government export control list, demonstrating that novel analytic approaches to PAI can produce law enforcement-relevant insights.

The work done during this internship has been saved to C4ADS's private GitHub. However, The findings from our study have been written into a research paper that can be found [Here](https://github.com/tylerrussin/C4ADS-Research-Paper/blob/main/Finalized%20C4ADS%20Paper.pdf)

C4ADS has continued its research after our contributions during the internship and has published a paper on the most recent findings. The official report can be found [Here](https://github.com/tylerrussin/C4ADS-Research-Paper/blob/main/C4ADS%20Official%20Report.pdf)

### Notable Journal Entries Discussing Challenges During the Internship

**Labs C4ADS Journal: Entry One**

**Abstract**

The following contains information about current challenges we face and ideas for handling some of those challenges. All proposed methods listed below are debatable and should be debated amongst our group. It should also be noted that these potential solutions and concepts have not been created from any research or exploration into the current challenges we face. Rather they are just the result of me laying in bed thinking with an admittedly limited knowledge base in the fields of data and computer science. The proposed concepts below are by no means proven nor do they claim to be optimal. They are simply just constructed from my imagination. That all being said please be as transparent as possible with your opinions on the proposed topics. 

**First Objective: Define a Baseline Model (Top Priority)**

As it stands now we have all the means to create a baseline model given the tagged and untagged data. I believe it is in our best interest to construct the basics for this baseline model before anything else. This includes but is not limited to constructing a dataset consisting of the tagged data and a subsample of the other data. A barebones pipeline to clean the data and label the data simply based on being either tagged or not tagged. This includes removing any Simple imputing or removing of nan values, either one hot or categorical encoding, and definition of a predictive model to first be used as our baseline. 

I recommend any of the following models:
 
-	Decision Tree Classifier 
-	Random Forest Classifier 

I should note that I am leaning towards classification models for our approach mostly because I believe most of our data exist in more of a non-linear format. However, this is something we should discuss more together. 

**Expectation from baseline model**

The expectation for the baseline model regardless of the model used is expected to have a low accuracy metric. This is largely due to the nature of how the target of the model will be defined. As it stands now the target column is simply and purposefully just the tagged company identification number rows. Which has some legitimacy but also a lot of noise. 

**Dissecting the Tagged Data** 

Much data cleaning, merging, and feature engineering can and will be done to improve the baseline model in the future. However, the priority has to be perfecting the target data which at the moment exist almost exclusively in the tagged dataset. As it stands the tagged data is only tagged by company identification numbers. The problem with this is a company can have many trade transactions but on some transactions involved in the dual-use nuclear cycle and some not. For example, a company such as Toyota might have one transaction that consists of windshield wipers and another separate transaction that consists of aluminum piping. With how the tagged data stands right now both these transactions would be labeled as dual-use nuclear items due to their association with a common company. Our goal should be to break down the tagged dataset so that only the transaction consisting of aluminum piping is labeled as being involved in the dual-use nuclear cycle. Essentially there is a lot of noise in the tagged data and this noise will in a sense confuse our model, ultimately stunning its accuracy. Because of this, we need to trim the tagged data into a more targeted subset. Below I present several potential solutions to solve this task. 

**Clustering: Finding the Patterns**

 One solution is to try several different clustering techniques to try to split the tagged data into separate categories(clusters) and then to manually analyze the clusters and see if any of them are associated mostly with dual-use trades. Clusters could be based on a variety of different columns such as company, number of shipments, cargo weight, and more. And everything would likely be analyzed based on the product description category. Admittedly I am slightly against this approach if it works, it has potential and it does use data science to solve the problem, but we will likely not find the patterns we are looking for in order to dissect the tagged data. That is not to say that we can’t use clustering for other parts of this project though. Once we acquire a strongly labeled dataset we can then use clustering to see the patterns(if any) between dual-use transactions and then search for those similar patterns in our master dataset. 

**Brute Force: Manual Labeling**

At first this might seem like an idea to not consider due to the tedious and time-consuming nature. However, if done correctly it could create a very strong foundation for any predictive models built from it. We have about 16,000 rows of tagged data. If we decided to manually label all 16,000 rows. We could split it between the four of us and we could likely label all of the data within a day or two. We would be labeling based on reading the product description Once labeled we could train a model that searched the rest of our data. We could then again manually label the newly tagged transaction that the model predicts. 

**Brute Force: Manual Labeling Modified for Automation**

Once simplification would be to manually label the tagged data and as we are manually labeling keep track of keywords that we associate with dual-use transactions. Once the four of us collectively build up a strong list of keywords we could then write some simple code to search the rest of the tagged data for those keywords, slightly automating the labeling process. 

**Unique Words: Full Automation**

One way to potentially bypass the manual labeling process and still keep many of the benefits of manually labeling would be to do as follows. First, run through the entirety of the tagged data and produce a list of unique words found in the product descriptions(being sure to ignore certain stop words to minimize noise). Once we have a list of all the unique words found in the product description we could either. Label the tagged data as dual-use or not based on the value counts of the words. Assuming that higher value counts are more or less dual-use(this idea is obviously flawed, and I gave it like 5 seconds of thought) Or we could manually look through the list of unique words and manually create a new list of words that we believe are involved in dual-use. We could then automatically label all of the tagged data based on if the product description contains words that are within our created list. This is essentially a big filter. It wouldn’t use any data science techniques, but it could potentially be the solution to our problem. As I stand right now I believe that this might be the best option.

**Important Side Note Relating to Above Methods: Two Targets**

It might be wise to be labeling the tagged data in two ways: one column as dual-use transactions and another column as a pure nuclear transaction. So a transaction with aluminum pipes would be labeled as dual-use but not as nuclear use and something like uranium ore would be labeled as both dual-use and nuclear use. This could greatly help the accuracy of future models we build and give us extra room for exploration. 

**Important Side Note: Using a List of Keywords**

Important to note that with any of the above methods where some kind of list of keywords is made we could technically determine all dual-use trades by just filtering out if a trades product description contains any of our predefined keywords. This could be pretty accurate in finding transactions with dual-use items. However, solving our problem that way only allows transactions to be determined as dual-use or not based on product description and that might not be enough. Or it might limit our potential since there could be hidden patterns in other features in our data that a predictive model might be able to pick up on. 

List Side Note Part Two

It should also be noted that once we have a list of keywords we are no longer limited to just labeling tagged data. We could apply our filtration to samples of other data in our master dataset to provide our models with more accurate data to predict.

**Future Model Notes: Brief**

I'm trying to avoid thinking about models and future feature engineering too much because a lot of other work should be done first. However, I had this thought and I wanted to write it down somewhere. Once we have a working predictive model it can be used to label data more accurately in our master dataset and then that more accurately labeled data could be trained into a new model that would be even better at making predictions. We would be essentially training an AI with another AI
