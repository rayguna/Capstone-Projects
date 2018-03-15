
# Capstone Project 1: IR spectral analysis of organic compounds via a machine learning approach


***

## Table of contents    
#### 1. Introduction   
#### 2. Data overview   
#### 3.  Machine learning approach  
#### 4.  Preliminary conclusions and next steps   


### Appendix 
#### Appendix A. Technical fundamentals
#### Appendix B. Data wrangling
#### Appendix C. Inferential statistics
#### Appendix D. Machine learning
#### Appendix E. List of dependent files

***

### 1.   Introduction

<i>1.1 Problem statement</i> 
<p align="justify">
With a rapid advancement in the fabrication of lightweight, compact, high-speed, and low-cost electronic/optoelectronic devices, the approach to using infrared spectroscopy for chemical identification is becoming widespread. For instance, Si-Ware Systems utilizes MEMS technology to fabricate an FTIR system in a chip size module (http://www.neospectra.com/products-overview/), in contrast to the traditional bench-top and bulky FTIR system. While it is relatively straightforward to correlate the infrared spectra of a defined class of compounds or a pure material, the spectra obtained from an FTIR measurement are generally ambiguous; it requires trained personnel to interpret the data, the process is tedious process, and is prone to errors. Thus, there is a need to establish a routine algorithm that yield a reliable and consistent result. The goal of this project is to improve the accuracy in a computer based chemical characterization of materials using IR spectroscopy.
</p>


<i>1.2. How does it benefit the client?</i>  
<p align="justify">
The client is an FTIR spectroscopy sensor manufacturer that desires a universal method for unambiguously identifying the chemical compounds of an unknown material from the measured FTIR spectra. The conventional approach used to identify FTIR spectra is to mathematically compare the unknown spectra against all possible spectra within the database (i.e., a spectral matching approach). The reference compounds with similar spectral features to the unknown material are ranked according to their similarity index. If it is done on a defined class of material, this process is very likely to yield an accurate result; however, for an entirely unknown material it will require a trained professional to then rationalize the result. The human interference is not necessarily consistent and error-proof. The proposed approach is to utilize machine learning algorithms (e.g., neural network and cluster analysis) to take into account the chemical information in combination with the spectral matching approach. A successful model is expected to yield an error proof routine to chemical compound identification that yields consistent results. 
</p>

<i>1.3. Data source</i>     

The list of required data sets are as follows:  

1. The main reference/training data set of over 40,000 spectra of known chemical compounds will be automatically downloaded from NIST website (http://webbook.nist.gov/chemistry/vib-ser/) with the aid of a python script.  

2. A group frequency table will be downloaded or web-scraped from a certain website to aid in chemical group assignment of occuring peaks in the FTIR spectra. There are a number of websites that carry this information, but the appropriate website will be selected for its reliable source of information and completeness.  

3. A list of representative FTIR spectra that have been properly assigned will be downloaded from NIST website (http://cccbdb.nist.gov); it will be used to verify the accuracy of the peak assignment that rely on the group frequency table.    



<i>1.4. Proposed approach</i>    
  
The tasks for this project are as follows:    

1. Download the appropriate data sets from the sources listed above.  

2. Perform cluster analysis of the reference data set to reduce the data size to a reasonable number (e.g., by chemical elements, molecular weight).    

3. Standardize reference spectra (uniformize units, perform spectra treatments as needed, e.g., interpolation, smoothing).  
4. Elucidate the spectral features by correlating with information provided in the group frequency table and utilizing EDA. Verify the accuracy of the peak assignment by using representative data from fully assigned spectra.   

5. Develop models for materials identification based on a combination of spectral matching algorithms and machine learning tools (e.g., neural network, cluster analysis).  

6. Select relevant test data sets that range form simple to hard and test the model. Improve the model as needed.  

7. Build a user friendly GUI (if time allows).  

8. Deploy application and present results.  


### 2. The dataset
<p align="justify">
<i>2.1. Basic principle</i>  
The mid-infrared absorption spectrum of organic materials can be correlated with the functional groups that make up its molecular structure. The knowledge of the material's functional groups can be used to derive its molecular structure. To name a few, this knowledge is useful for materials identification in forensics science, reverse engineering of products, or for the purpose of correlating the material's properties to its chemical structure.   

Please refer to <b>Appendix A</b> for the definition of the various technical terminologies and for more detailed technical fundamentals. 
</p>

<p align="justify">
<i>2.2. A general overview of approach</i>
<b>Figure 2.1</b> below summarizes the entire process involved in this work. The steps are as follows:  

- The first step is taking the NIST chemical list that contains over 70,000 chemicals. Data wrangling is performed on the data to yield a shorter chemicals list. The shorter list contains no null entries and include only a list of organic chemicals.   
- Next, inferential statistics is performed on the data and representative list of chemicals that represent certain organic functional groups are selected.   
- The corresponding infrared spectra of this chemicals list are downloaded from NIST website. This data serves as our dataset. 
- Subsequently, machine learning and deep learning models are considered. The best model is selected.  
- The final model is expected to be able to extract the functional groups that are present in an unknown IR spectrum with a high accuracy.   
</p>

<img src="data/Fig 2.1. Flowchart.png" width="400"><br>
<b>Figure 2.1.</b> A flowchart summarizing the processess carried out in this work.

</i>2.3. Data wrangling<i>
Below shows a portion of the starting NIST chemical list. It is seen that the list consists of just three columns: chemical name, chemical formula, and CAS number. CAS stands for chemical abstract service, which refers to a unique identification number. Note that any of these fields may contain NaN. 


```python
#import pandas
import pandas as pd

#load NIST_chemicals_list.csv and inspect
pd.read_csv('data/NIST_chemicals_list.csv', names=['Name','Formula', 'CAS']).head() 
```
<img src="data/Table 1.png"><br>
<br>

<p align="justify">
The following summarizes the data wrangling steps, some findings, and the outcome:

- All rows that contain null entries in any of the three columns are deleted.<br>
- Only the list of compounds that are non ions, non radicals, and non-polymeric are retained. <br>
- Only the list of organic compounds containing the following elements: C, H, O, N, S, P, F, Cl, Br, and I are retained. <br>
- Each entry in the final DataFrame has a unique CAS number, but there are duplicate names and isomers and allotropes.<br>
- Shortened the NIST chemical list from 72,618 to 36,416. <br>
- The shortened DataFrame has been exported into a csv file called 'NIST_chemicals_list_organic.csv'. <br>

See <b>Appendix B</b> for details.

The data wrangling step yields a shortened chemicals list with two additional columns (shown below):  

- Elements: a list of the unique elements in the compound. This list can be used to check whether the compound is organic or not (i.e., organic compounds contain just a handful of elements in the periodic table). <br>
- Mw: the total molecular mass of the compound. This is proportional to the size of the molecule.<br>  


```python
#load NIST chemicals_list_organic.csv and inspect
pd.read_csv('data/NIST_chemicals_list_organic.csv').head() 
```
<img src="data/Table 2.png"><br>

<br>

<p align="justify">
<i>2.4. Inferential statistics</i>
Taking the shortened chemicals list with the two additional columns, I perform inferential statistics. The following summarizes the efforts, some findings, and the outcome:

- The calculated Mws are accurate within 10% with respect to those posted on NIST website. <br>
- The distribution of Mws are generally not normal with the median being in the 100-200 range. <br>
- The shortened NIST chemicals database mostly consist of the following functional groups:'ester', 'ketone', 'alcohol', 'alkane', 'alkene', 'amine', 'aldehyde', 'acid', 'halide', 'thiol', and 'benzene'.  <br>
- IR spectra (in jcamp format) are downloaded to represent each of these group. <br>
- The Pearson correlation coefficient of the representative spectra from each group reveal that the alkane spectra have a high correlation, whereas the halides have a low correlation. <br>
- Generally, there is very little correlation between spectra, even within the same group. For spectra that have a high correlation, the features are expected to be mostly identical. <br>
- Probably, we cannot rely simply on the overall shape of the spectrum to distinguish functional groups. <br>   
- For the purpose of identifying unknown compounds from its IR spectra, we therefore need to undertake features selection. <br>
- One specific approach is to correlate each functional group with specific regions in the spectrum.<br>
- There is also a need to utilize more sophisticated algorithms (i.e., machine and deep learning algorithms) to more effectively correlate the chemicals' functional groups to its distinct features.<br>  

See <b>Appendix C</b> for details.
</p>
<p align="justify">
In most cases, little linear correlation is seen between spectra within the same group. This justifies the use of the more advanced machine learning tools that are available in Scikit-learn. This step yields the needed dataset. Each row in the dataset represents a single infrared spectrum. Each spectrum is labelled as one of ten functional groups (see below).
</p>

```python
pd.read_csv('data/NIST_selected_organic_spectra.csv').head()
```
 
<img src="data/Table 3.png"><br>
<br>

### 3.  Machine learning approach

To utilize the dataset in Scikitlearn, first I convert the functional group labels into MultiLabelBinarizer (see below). The following summarizes the efforts, some findings, and the outcome:

- Five machine learning models that are suitable for multilabel classification problems (http://scikit-learn.org/stable/modules/multiclass.html) are assessed (i.e., DecisionTreeClassifier, DTC; ExtraTreeClassifier,ETC; ExtraTreesClassifier , ETC_E;  KNeighborsClassifier ,KNC; and RandomForestClassifier. RFC). Below compares the accuracy comparison between the five models. <br>


<img src="data/Fig 3.1. compare_models.png"><br>

<b>Figure 3.1.</b> Algorithm comparison.

- Initial screening reveals that ExtraTreesClassifier and KNeighborsClassifier perform better than the other three algorithms. <br>
- Next, I utilize dimensionality reduction techniques (i.e., PCA) along with these two machine learning algorithms and optimize the parameters in the respective models. <br>
- Using KNeighborsClassifier, the best score that is achieved is 69.0% (Components = 20 , neighbors = 5). This is a very modest score. Using, the best score is lower than KNeighborsClassifier, ie., 63% (components = 20 , estimators = 200). <br>   
- Ensemble methods are attempted (i.e., RandomForestClassifier and ExtraTreesClassifier), but the scores did not exceed 69.0%. <br>  
- To determine the uncertainty of the score for KNeighborsClassifier, I carry out k-fold cross-validation, which yields an accuracy of 69.896 ± 5.145%. <br> 
- To further improve the model accuracy, I will consider a neural network model. <br>   
- First, I will attempt a basic neural network algorithm to achieve an accuracy above 0.90. <br> 
- If it is not achievable, I will consider employing a convolutional neural network algorithm. <br>

See <b>Appendix D</b> for details.  

To determine the uncertainly of the models' score, I carry out k-fold validation for the best model using the optimized parameters, which yields: 69.896 ± 5.145%.<br>


### 4.  Preliminary conclusions and next steps

- KNeighborClassifier is found to be the best machine learning model with accuracy of about 70%. A 70%  accuracy, however, is not acceptable. <br>
- Next, neural network models will be considered to achieve a better accuracy than 70%. <br>
    

***


## Appendix

### Appendix A. Technical fundamentals
Refer to the accompanying file:  Appendix A. Technical_fundamentals.ipnyb

### Appendix B. Data wrangling
Refer to the accompanying file:  Appendix B. Data_wrangling.ipnyb

### Appendix C. Inferential statistics
Refer to the accompanying file:  Appendix C. Inferential_statistics.ipnyb

### Appendix D. Machine learning
Refer to the accompanying file:  Appendix D. Machine_learning.ipnyb

### Appendix E. List of dependent files

E1. csv files: <br>
    - data/NIST_chemicals_list.csv <br>
    - data/NIST_chemicals_list_organic.csv <br>
    - data/NIST_selected_organic_spectra.csv <br>

E2. py files: <br>
    - common.py <br>
    - my_jcamp.py <br>
    
E3. jcamp files  <br>
    - Files are downloaded from NIST website (https://webbook.nist.gov/chemistry/) <br>
    - Downloaded files are stored in the folder called reference. Due to the large number of files, these are not included in this Github project.

