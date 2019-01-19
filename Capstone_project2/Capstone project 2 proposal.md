---
title: "An automated inventory system via a machine learning approach"
author: "Ray Gunawidjaja"
date: "March 19, 2018"
output: html_document
---

<b>Problem statement</b> <br>
<i>What is the problem you want to solve?</i><br>
Having an efficient and error-proof inventory management system is central to every business. Poor inventory management system will result in financial losses due to items being misplaced, mislabelled, stolen, etc.   

<b>How does it benefit the client</b><br>
<i>Who is your client and why do they care about this problem? What will your client DO or DECIDE based on your analysis that they would not have otherwise?</i><br>
The client is a logistic company who wish to improve their inventory management system to enhance accuracy and speed. As part of the effort, I propose a computer program that can identify objects from a livestream video and subsequently generate a list of the identified objects to a simple csv file. The list will be used as a new inventory list, for inventory reconciliation, or for further analysis. 

<b>Data source</b> <br>
<i>What data are you going to use for this? How will you acquire the data?</i><br>
The list of required data sets are as follows:  
The data is a series of images of various labelled objects. These images will be obtained from, e.g., http://deeplearning.net/datasets/. For the proof-of-concept testing of the computer program, I will be taking pictures of common household objects (e.g., boxes, kitchenwares, toys).  

<b>Proposed approach</b> <br>
<i>Briefly outline your approach to solving this problem while keeping in mind that it might change later.</i><br>
The tasks for this project are as follows:  
1. Establish the framework for using opencv along with python to obtain and analyze a livestrem video of various objects.<br>
2. Obtain and prepare the appropriate data sets that consist of labelled objects from the sources listed above.<br> 
3. Generate various machine learning/deep lerning models that are capable of identifying objects from a livestream video.<br>
4. Select the most promising model.<br>
5. Test model using validation data.<br>
6. Present results.<br> 

<b>Deliverable</b> <br>
<i>What are your deliverable? These typically include code, along with a paper and/or a slide of deck.</i><br>
As part of the requirement for completing this project, the following is the list of deliverables:<br>
1. Code. Well-documented in github.<br> 
2. Final paper. Uploaded in github that contains the following elements: statement of problem, approach, finding, ideas for further research, and up to 3 concrete recommendations for your client on how to use your findings.<br> 
3. Slide deck. Use any standard presentation tools to create your deck.<br> 
4. Shared project. Present in an office hour, create an online video, or writing a blog post.<br> 




