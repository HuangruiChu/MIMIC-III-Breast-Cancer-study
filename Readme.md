# Breast Cancer Analysis based on MIMIC III

# **1. Introduction**

*This project for Yale Course BIS 638: Clinical Database Management Systems and Ontologies Final Project*

## 1.1 Background
- Breast cancer is the most common cancer in women in the United States. Symtoms include a new lump in the breast, abnormal discharge from the nipple, etc.
- It is important to study breast cancer because it is a dangerous disease. It is also the second leading cause of cancer death among women overall and the leading cause of cancer death among hispanic women.  
<br/>

## 1.2 Research Questions
### 1.	Exploratory Analysis:
#### a.Gender Distribution. 
#### b.Age distribution 
Age Distribution of breast cancer patients (when visiting hospitals, diagnosed, dead) V.S International organization’s recommendation for women's health check and breast cancer high-occurrence age. 
#### c.Survival ratio (survived V.S dead) 
#### d.ICU stay time
### 2.	Ontology Problem 
#### a.Synonym 
#### b.Drug Usage
### 3.	What are the attributes that are most correlated to breast cancer? 
Utilizing random forest to rank the feature importance based on Gini parameters
### 4.	 Prediction on breast cancer

## 1.3 Data Sources

The data comes from [MIMIC-III](https://physionet.org/content/mimiciii/1.4/), which is a large, freely-available database comprising deidentified health-related data associated with over forty thousand patients who stayed in critical care units of the Beth Israel Deaconess Medical Center between 2001 and 2012. The database includes information such as demographics, vital sign measurements made at the bedside (~1 data point per hour), laboratory test results, procedures, medications, caregiver notes, imaging reports, and mortality (including post-hospital discharge).

MIMIC supports a diverse range of analytic studies spanning epidemiology, clinical decision-rule improvement, and electronic tool development. It is notable for three factors: it is freely available to researchers worldwide; it encompasses a diverse and very large population of ICU patients; and it contains highly granular data, including vital signs, laboratory results, and medications.

<div  align="center"> 
<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTeV6ahtZ6-POUjabTbHU973fbwmNAqleU5ag&usqp=CAU" width = "100" alt="img" />
</div>


  - To access MIMIC-III, our group have finished [CITI Training for Data or Specimens Only Research](https://www.citiprogram.org/verify/?wf27bcd4b-1305-43f1-a4f9-16549df886fa-51130798). The certifications are shown below.

<div  align="center"> 

<img src="https://raw.githubusercontent.com/HuangruiChu/MIMIC-III-Breast-Cancer-study/main/CITI_certification_Huangrui_Chu.png" width = "300" alt="img" />
<img src="https://raw.githubusercontent.com/HuangruiChu/MIMIC-III-Breast-Cancer-study/main/CITI_certification_Mingrui_Huang.png" width = "300" alt="img" />
<img src="https://raw.githubusercontent.com/HuangruiChu/MIMIC-III-Breast-Cancer-study/main/CITI_certification_Yaoqi_Li.jpg" width = "300" alt="img" />
<img src="https://raw.githubusercontent.com/HuangruiChu/MIMIC-III-Breast-Cancer-study/main/CITI_certification_Kuankuan_Sun.png" width = "300" alt="img" />
</div>

Data files have be downloaded and saved in google drive, and will be loaded into google colab. The data are for research study only and we would not upload them to github.

<br/>

## 1.4 Methods 
<br/>
<img src="https://mark.trademarkia.com/logo-images/epoint-sa/vertabelo-86225942.jpg" height = "50" alt="img" />
<img src="https://www.sqlite.org/images/sqlite370_banner.gif" height = "50" alt="img" />
<img src="https://miro.medium.com/max/776/1*Lad06lrjlU9UZgSTHUoyfA.png" height = "50" alt="img" />

<br/>

- Use [Vertabelo Database Modeler](https://vertabelo.com/) to design an ER model, then generate a MySQL script.
- Use google colab to install SQLite, read in SQL script and create a database with MIMIC III data.
- Use [OLS](https://www.ebi.ac.uk/ols/index) as an ontology resource, link the disease terms in Disease Ontology to the patient data representation (icd9_code)

## 2 Data Resource

- The data files we will use include:
  - `PATIENTS`: Defines each SUBJECT_ID in the database, i.e. defines a single patient
  - `ADMISSIONS`: Define a patient’s hospital admission, HADM_ID
  - `DIAGNOSES_ICD`: Contains ICD diagnoses for patients, most notably ICD-9 diagnoses
  - `D_ICD_DIAGNOSES`: Definition table for ICD diagnoses
  - `ICUSTAYS`: Defines each ICUSTAY_ID in the database, i.e. defines a single ICU stay
  - `PRESCRIPTIONS`: Contains medication related order entries, i.e. prescriptions
  - `LABEVENTS`: Contains information regarding laboratory based measurements.

  - We Create an ER model with Vertabelo for our project

![ER](https://github.com/HuangruiChu/MIMIC-III-Breast-Cancer-study/assets/65276824/522f337a-aeaa-4902-b1b4-97a3b49afe01)


  - The whole MIMIC Schema and Entity-Relationship (ER) Diagram can be seen via this [link](https://mit-lcp.github.io/mimic-schema-spy/)



  # **3. Exploratory Data Analysis**

  ## 3.1 Gender proportion

We know that male rarely get breast cancers. Here we first want to have a brief idea of the gender propotion of breast cancer in MIMIC III.

![image](https://github.com/HuangruiChu/MIMIC-III-Breast-Cancer-study/assets/65276824/196439c0-9bc8-4d0c-ba4d-1f38a8b546aa)

First, we use the breast cancer table to graph the gender proportion. Unlike the majority of other cancer with higher incidence rates in males, 229,060 people were diagnosed with breast cancer in the United States in 2012, with less than 1% of these cases appearing in men. (3) Overall, there are 1 male and 143 females and the gender ratio is 0.69% and 99.31% this is the same as our hypothesis. 

## 3.2 Age distribution 

We want to know the age distribution of breast cancer patients (when visiting hospitals, diagnosed, dead) V.S International organization’s recommendation for women health check and breast cancer high-occurrence age.
![age](https://github.com/HuangruiChu/MIMIC-III-Breast-Cancer-study/assets/65276824/ae94a94e-76b7-442d-a79b-7f67a580aa7c)
From the graph, we can see that most patients will be diagnosed after 40 years old, and between 45-70 patients have high mortality. Hence, our result is consistent with the recommendation provided by the CDC that women who are 50 to 74 years old and are at average risk for breast cancer get a mammogram every two years. 
## 3.3 Survival ratio (survived V.S dead)

We want to learn about the survival ratio (survived V.S dead) of breast cancer.
![survival](https://github.com/HuangruiChu/MIMIC-III-Breast-Cancer-study/assets/65276824/f091ab86-d5cb-4d94-b108-df6e4676b04f)

Then, we use the breast cancer table to graph the survival ratio with 53.12% survival and 46.88% dead. The adjusted survival ratios were 0.674 for high-volume hospitals, indicating that patients who had undergone breast cancer surgery at hospitals with higher surgical volume had a lower risk of death, and were likely to achieve longer-term survival.

## 3.4 ICU Stay

- We created a table called ICU STAYS that include variables such as the row id, time the patient gets in the hospital and time the patient gets out of the hospital. We want to pay more attention to the variable los, which stands for length of stay. 
- In another table, we sum the length of stay and group it by the hospital admission id to get the total icu stay time for every patient. 
- Using the above two tables, we were able to calculate the mean and median of the icu stay time, which are 3.32 and 1.9 respectively. Knowing the ICU stay time is important for many things such as bed management in hospital. 
![ICU](https://github.com/HuangruiChu/MIMIC-III-Breast-Cancer-study/assets/65276824/c33cc3cf-2468-4ca8-afc5-2f6ab250567c)
# **4. Ontology**
**Breast cancer Synonyms**
## 4.1 Retrieve DOID terms (and their synonyms) that have ICD9CM codes
Please refer to the notebook for this part.
## 4.2 Create Table that maps DOID with ICD9 

First, we create an empty table. Then we feed the data extracted from [OLS](https://www.ebi.ac.uk/ols/ontologies/doid/terms?iri=http%3A%2F%2Fpurl.obolibrary.org%2Fobo%2FDOID_1612) about Breast Cancer into this mapping table
## 4.3 Create Prescription Table
Please refer to the notebook for this part.
# **5. Feature Selection**

We use RandomForestRegressor and choose 75% quantile as the threshold to extract important features to decrease feature space dimensions.

![rf](https://github.com/HuangruiChu/MIMIC-III-Breast-Cancer-study/assets/65276824/1bdcf599-eda3-4822-986d-bf80411252c8)
Here is the example of top 5 important variables.

# **6. Logistic Regression**

It is significant to train a model to predict whether a person can get breast cancer so that we can prevent the disease easier. We used the five most important features we get above to conduct a logistic regression. From the confusion matrix, we can see that the model classifies most points correctly. The accuracy of this model is 76.32%, which is pretty decent. AUC is a measurement of how accurate a model is in classifying correctlt. We can see that the area above y=x and below the AUC curve is large. So it again proves that our model is pretty good.
![confusion](https://github.com/HuangruiChu/MIMIC-III-Breast-Cancer-study/assets/65276824/1c7a3b80-4374-45d0-8d14-05662e750ab3)

![AUC](https://github.com/HuangruiChu/MIMIC-III-Breast-Cancer-study/assets/65276824/237b6000-001a-490b-b4e5-34d0c0ad269c)

ROC curve for Logistic Regression, showing true positive vs. false positive rate

# **7. Reference & Acknowlegement**
- Center for Disease Control and Prevention, Breast Cancer Basic Information. Access available https://www.cdc.gov/cancer/breast/basic_info/symptoms.htm on December 10, 2022. 

- Johnson, A., Pollard, T., & Mark, R. (2016). MIMIC-III Clinical Database (version 1.4). PhysioNet. https://doi.org/10.13026/C2XW26.

- Johnson, A. E. W., Pollard, T. J., Shen, L., Lehman, L. H., Feng, M., Ghassemi, M., Moody, B., Szolovits, P., Celi, L. A., & Mark, R. G. (2016). MIMIC-III, a freely accessible critical care database. Scientific Data, 3, 160035.

- Goldberger, A., Amaral, L., Glass, L., Hausdorff, J., Ivanov, P. C., Mark, R., ... & Stanley, H. E. (2000). PhysioBank, PhysioToolkit, and PhysioNet: Components of a new research resource for complex physiologic signals. Circulation [Online]. 101 (23), pp. e215–e220.

- Sharma, Ganesh N., et al. "Various types and management of breast cancer: an overview." Journal of advanced pharmaceutical technology & research 1.2 (2010): 109.

- Ly, Diana, et al. "An international comparison of male and female breast cancer incidence rates." International journal of cancer 132.8 (2013): 1918-1926.

- Centers for Disease Control and Prevention. (2022, September 26). Basic information about breast cancer. Centers for Disease Control and Prevention. Retrieved December 1, 2022, from https://www.cdc.gov/cancer/breast/basic_info/index.htm  

- Chen, Chin-Shyan, et al. "Does high surgeon and hospital surgical volume raise the five-year survival rate for breast cancer? A population-based study." Breast cancer research and treatment 110.2 (2008): 349-356.


- https://github.com/yijunyang/database/blob/main/MIMIC3_Project.ipynb

- Thanks to instructors Kei-Hoi Cheung, Ph.D. (Kei.cheung@yale.edu) and Ronald "George" Hauser, M.D. (ronald.hauser@yale.edu) for providing this informative course. Also, thanks to our teaching fellow Han Yu (han.yu@yale.edu) for providing advices on this project.

