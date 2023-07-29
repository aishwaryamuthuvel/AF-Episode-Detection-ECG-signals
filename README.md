# AF-Episode-Detection-ECG-signals

## Abstract 
This an algorithm to automatically detect Atrial Fibrillation (AF) episodes from the ECG data collected from the patients. Atrial Fibrillation is a condition of abnormal and non-uniform beating of the heart. It is caused due to irregularity in the beating of the atrial chambers of the heart at an abnormally increased pace. The algorithm proposed in this paper uses the length of the R-R intervals extracted from the ECG signal as input parameters and classifies it as AF or non-AF. Neural Networks (NN), Support Vector Machine (SVM) and Decision Tree algorithms were trained and used for classification. Dimensionality reduction on the feature set was attempted using the Gini impurity indexes obtained from the Decision tree algorithm and the feature set size was reduced significantly without a significant decrease in the model performance. 

## Introduction

The ECG wave of a typical heartbeat consists of three main components – P wave, QRS complex and T wave. The first “P wave” is created when the impulse travels through the atria, the upper chambers, of the heart. The second part, the “QRS complex” is created by the impulse travelling through the ventricles, the lower chambers, of the heart. The third part, the “T wave” is caused by the electrical recovery of the ventricular chambers to return to their resting state. 

<p align="center">
<img src="https://github.com/aishwaryamuthuvel/AF-Episode-Detection-ECG-signals/blob/main/ECG.png" width=30% height=30% />
</p>

A normal episode of AF is generally identified by the absence of the P wave or by the unevenness and higher frequency of the ventricular rate for a period of at least 30 seconds. Thus, an AF episode could be identified from an ECG wave either by analyzing the Atrial activity, that is the P wave or by identifying irregularity in the ventricular rate by measuring the time interval between two consecutive R peaks – the R-R interval. The R peak is the most prominent characteristic within the ECG, making it relatively simple to detect. Therefore, the R-R interval irregularity is deduced from a patient’s ECG record to automatically detect an AF episode in them. 

##  Dataset

The dataset provided with the project is the ECG data collected from the electrophysiology department of Erasmus Medical Centre in Rotterdam. All these data were obtained from patients within 10 days of Coronary artery bypass surgery because patients after cardiac surgery tend to have AF frequently. Each ECG from a patient is a 12-lead ECG where information is gathered from 12 different areas of the heart. Each ECG recording was semi-automatically analyzed and R peaks were annotated, which was later manually verified by a physician to label irregularities. Each of these annotated ECG data was collected and stored in a text file as raw data with 3 columns - timestamp, R-R peak interval (ms) and type of heartbeat.

## Data Pre-processing

As a first step of preprocessing, the ECG recordings were split into samples of length 30 seconds. Data cleaning was also a part of the preprocessing steps. The ECG samples with insufficient information or with information that cannot be used for analysis were removed. All the R-R intervals that were obtained from each ECG sample were placed into one of the 30 bins created by splitting the range from 200ms to 1700ms by 50ms. This gives the frequency of R-R intervals present in a particular bin. The frequency obtained is normalized, shuffled and stored. The preprocessed data consists of 150,000 30 sec ECG records. As mentioned in preprocessing steps, R-R intervals were normalized and stored in the first 30 columns of a 31 column Excel sheet. The 31st column is filled with either 1 or 0 where 1 represents AF and 0 represents non-AF as reported by a physician. Amongst 150000 records, 36537 are categorized as AF and 113463 are categorized as non-AF.

## Data Downsampling

Once all the steps of preprocessing have been completed, data are classified into training, testing and validation datasets. Since the ratio of AF to non-AF is large it gives rise to an imbalanced dataset. To cope up with an imbalanced data set, we randomly chose 73200 records of non-AF. This maintained the ratio between the number of AF and non-AF records as 0.5 and balances the dataset.

Finally, 80% of the dataset was declared as training dataset and the remaining 20% was split equally and declared as validation and test dataset. Training data contained 87789 records and validation and testing data each contained 10974 records.

## Classification

As classification algorithms, Neural networks, Support Vector Machine and Decision Tree were chosen. The normalized frequency of the R-R intervals (extracted from the 30 seconds long ECG sample) in 30 bins each of size 50 milliseconds and in the range 200 to 1700 milliseconds is taken as input for the classifiers. The output of these classifiers will be the class to which the input 30-second ECG sample belongs – AF or non-AF. We used Machine Learning models from the Scikit-learn Python library for training and testing all the classifiers. In order to find the optimum training parameters of each algorithm, models were trained using the training data and then evaluated on the validation data and the model that had the best performance on the validation data was chosen.

## Dimensionality Reduction Using Gini Index

From the decision tree model, the Gini index of each parameter could be computed. These were used as a measure of the importance of the input feature. For each feature, the amount of variance of the data represented by it could be calculated using the equation below where n is the total number of features, m is the feature for which the variance is calculated and Gi is the Gini index corresponding to the feature I.

<p align="center">
<img src="https://github.com/aishwaryamuthuvel/AF-Episode-Detection-ECG-signals/blob/main/Gini_formula.png" width=30% height=30% />
</p>

The plot in the below figure shows the cumulative variance value versus the number of features that were considered. 

<p align="center">
<img src="https://github.com/aishwaryamuthuvel/AF-Episode-Detection-ECG-signals/blob/main/Variance.png" width=30% height=30% />
</p>



