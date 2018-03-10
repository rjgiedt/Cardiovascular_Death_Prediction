# Cardiovascular Death Prediction
Files related to an insight data science project focused on predicting death in atherosclerosis patients from the MIMICIII database. 

### Project Explanation
Machine Learning and Deep Learning to Predict Cardiovascular Complication and Mortality

#### Summary

Cardiovascular mortality is one of the leading causes of death in the developed world. In this project, I utilize the MIMIC III dataset to explore the application and effectiveness of traditional and state of the art machine learning and deep learning techniques to patients diagnosed with coronary atherosclerosis. While this project focuses on a specific disease, the methods and techniques illustrated here have broad application in analysis of medical and temporal datasets. Source Code for this project can be found on Github.

#### Problem Background

An estimated 16.3 million Americans have coronary artery disease, which is ~7% of all U.S. citizens over the age of 20. Coronary artery disease occurs when atherosclerotic plaques, which are made up of cholesterol and other fatty substances, significantly occlude the arteries which provide blood to the heart muscle. The consequences of this disease can range from angina (chest pain) to a rupture of the plaque leading to arterial clot formation and heart tissue death (Myocardial Ischemia). For more information see:

#### AHA Media Player

Coronary artery disease (CAD) occurs when the inside (the lumen) of one or more coronary arteries narrows, limiting the…
watchlearnlive.heart.org	

#### Data Set

For this project I utilized the freely available MIMIC III Database: https://mimic.physionet.org/ . The dataset is made up ~ 47,000 Unique patients with over 650,000 diagnoses. Each patient and diagnosis is composed of a rich list of attributes including patient demographic information, lists of medications, a history of diagnoses and other potentially predictive medical characteristics in patients.


(Taken from the MIMIC III website) An Overview of the MIMIC III database and the information included.
To begin analysis of this data, we want to find all of the patients in the database with a diagnosis of coronary atherosclerosis; this corresponds to an ICD9 code of 414.01. Furthermore, we want to find which of these patients survived through the study versus those which were deceased. Additionally, we want to remove those patients who were more than 89 years old at anytime in the study as, 1. Physicians will more than likely treat these patients differently due to their age, and 2. Exact ages for these patients aren’t recorded in the Mimic III database. Results from this analysis including the size of our final dataset can be seen below.


#### Analysis of the MIMIC III Database to understand the patient population with coronary atherosclerosis diagnoses.
With these datasets we can being to conduct preliminary data analysis on the demographic characteristics found in our population dividing the deceased from those patients who survived. For a more complete data analysis, please see the linked github Jupyter notebooks.


#### A few examples of demographic information illustrating differences in death rates.
Looking at just these three examples, we can see that race seems to have some affect on outcome, as the average patient who dies is more likely to not self classify as white. Similarly, these patients who die are far more likely to be admitted to the ICU via the ER and are more likely to be on Medicare insurance. Of course there are confounders for this data (for example, Medicare patients are also more likely to be older), but with a more extensive analysis, a seemingly reasonable hypothesis would be that applying a machine learning approach on demographic/ admission/ and other data would allow us to predict patients most likely to pass away.

#### Machine Learning

Random Forest Cross Validation Accuracy versus Number of Features Selected
To employ a machine learning approach, I created a feature set involving all demographic data available (race, religion, language, insurance type, marriage status, age, sex among others) as well as a naive interpretation of medical history including # of appointments they have been to. Combined, after one-hot encoding our feature set is ~ 120 features. Training a random forest classifier with this data and analyzing the effects of feature number, it appears (without additional hyperparamter optimization) that our optimal cross validation score will be ~ 0.69. Further optimization via gridsearch yields a maximum score of 0.72.

#### Deep Learning

While 0.72 accuracy may be helpful for some applications, in the case of medical and patient care, it unfortunately does not provide predications accurate enough for physicians to use in the clinic. Indeed, this value would yield a large proportion of false positives in the data set.


#### Simplified version of a typical RNN operating on medical visits.
To get a better predication of this data, an ideal solution would take into account patients past appointments i.e. what has happened in a patients history, in addition to demographic information. An ideal approach for this type of data is a recurrent neural net (RNN). Briefly, RNNs are a type of neural net setup for designed for taking in temporal data. As seen above, the structure of an RNN is designed such that we can conduct operations on individual time points and utilize information from each time point to predict data at the next, an ideal fit for working with medical diagnoses with patients. Unlike the above diagram though, we only care about a final output, not necessarily outputs at each diagnosis.

Setting up an RNN for our data (see Github for implementation details), I was able to incorporate the diagnoses of individual patients over time, yielding a cross-validated AUC of 0.82, a substantial improvement from the 0.72 cross validation score. While its great that our score increased, frustratingly, RNN (and neural nets in general) while more sophisticated than typical machine learning methods, are nearly uninterpretable black boxes with their hidden layers and other under the hood characteristics.


#### Simplified Attention RNN Model.
As a solution for this, the final analysis of this project I implemented was based on a great paper out of Georgia Tech focusing on an interpretable attention RNN for medical data (RETAIN). The idea of an attention RNN is that we will be able to gain information on the hidden layers that we missed in our RNN interpretation above, while still gaining from the sophistication and in this case, time data incorporation, found in an RNN. Altering this model for our purposes and implementing it yielded a cross validated AUC of 0.81, nearly as good as found in our RNN. In addition, this model can also tell us what medical codes were significant when analyzing our data. In checking the most significant medical codes from our data, I found that, unsurprisingly, heart attacks were one of the biggest contributors to death.

#### Conclusions

Here I implemented several different methods of machine learning and deep learning in an attempt to predict mortality in patients with coronary atherosclerosis. While utilizing patient demographic information and limited information about their interaction with physicians, we were able to come to a reasonable predication about their mortality risk. By utilizing an RNN, we could improve on this prediction, with the penalty of making the model uninterpretable. By utilizing an attention RNN I was able to come to a similar predictive value while better understanding the variables that lead to risk for the patient.

The ideal user of this system would be care providers, hospitals and insurance companies. Using machine learning and/or deep learning techniques, it should be possible, as shown here, to better predict patients who would benefit from aggressive physician intervention in order to save lives. Furthermore, there would be substantial cost savings with such a system as heart attacks or other cardiovascular diseases are nearly always acute events, requiring expensive emergency transportation and care for patients.
