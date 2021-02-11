
# Project 2 - Operationalizing Machine Learning

## Purpose
This project is part of the Udacity Azure ML Engineer Nanodegree.Here, we have a plan to use Azure ML to to predict if a client will subscribe in the banking based on several independent variables that we have. [Bankmarketing Datset](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv).We trained our model using AutoML approach and best algorithm selected and deployed as an webservice to Azure Container Instance. At the end, we automated the procedures by creating and deploying a pipeline.


## Architectural Diagram
These are steps we followed in this project. The diagram below shows the workflow of this project. First, we need to create a Security Principal (SP), then an AutoML experiment is run and the best model have been chosen and deployed, later the deployed model is consumed through the REST endpoint. Finally, we automated he workflow by making a pipeline and publishing it. 

![diagram](images/diagram.png)


### 1. Authentication 
Here, we need to create a Security Principal (SP) to interact with the Azure Workspace.For this experiment the lab-service which Udacity provided has been used. 

### 2. Automated ML Experiment
The steps here are including
In this step, I created an AutoML experiment to run using the [Bank Marketing](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv) Dataset which was loaded in the Azure Workspace, choosing **'y'** that is customer subscribe or not as the dependent variable.

*Figure 1: Bank Marketing Dataset*
![](images/1.Dataset.JPG)
I uploaded this dataset into Azure ML Studio in the *Registered Dataset* Section using the url provided in the project.

For the compute cluster, I used the **Standard_DS12_v2** for the Virtual Machine and 1 as the **minimum number of nodes**.

I ran the experiment using classification, without enabling Deep Learning. The run took some time to test various models and found the best model for the task.

*Figure 2-3-4-5: Create an AutoML experiment*
![Create an AutoML experiment](images/CreateAutoMLrun.png)
After selecting the dataset which I'll work with, I chose an experiment name and the targey column for the training.

![Create an AutoML experiment](images/CreateAutoMLrun6.png)
Since it's a classification problem, I specified the task type without enabling deep learning.

![Create an AutoML experiment](images/CreateAutoMLrun5.png)
I checked *Explain best model* to have insights on the best model, and chose *Accuracy* as the primary metric the trained models will be compared by.

![Create an AutoML experiment](images/CreateAutoMLrun4.png)
I reduced the *Exit Criterion* to 1 hour and *Concurrency* to 5 concurrent iterations max.

*Figure 6: AutoML run*
![AutoML run](images/AutoMLrun2.png)
The experiment is running, and many models are being trained on the **Bank Marketing** Dataset to find the best one.

*Figure 7: AutoML run Complete*
![AutoML run](images/AutoMLrun3.png)
After a while, the eperiment is complete and I can access the models that were trained and find the best model.

The best model for this classification problem was a **Voting Ensemble** model with **0.91958** Accuracy.

*Figure 8: Best model*
![Best model](images/Bestmodel.png)
I access the best model to learn more about its metrics and other details.

*Figure 9-10-11: Best model metrics*
![Best model metrics](images/Bestmodelmetrics.png)
In this section, I can see all of the model's metrics, such as *Accuracy*.

![Best model metrics](images/Bestmodelmetrics2.png)
I can see some graphs such as *Recall* and *False Positive Rate*.

![Best model metrics](images/Bestmodelmetrics3.png)
There are many other details that I can access on the side by checking any value I'm interested in.

After the AutoML run completed, the best model summary showed the best run and the algorithm used. Here the best model is a Voting Ensemble model, which had an accuracy of **0.91897**.<br> A Voting Ensemble is a machine learning model that trains on an ensemble of numerous models and predicts an output based on their highest probability of chosen class as input. <br>

### 3. Deploy the Best Model
In this experiment, the best model obtained used the Voting Ensemble algorithm. During deployment, **authentication is enabled**. Authentication is crucial for the continuous flow of operations, as Continuous Integration and Delivery systems rely on uninterrupted flows. The screenshot below shows thet *Key based authentication enabled* as *true*.<br>
The model is deployed using **Azure Container Instance**. Azure Container Instance service uses key-based authentication and is disabled by default. Deploying the best model allows interaction with the HTTP API servie and data can be sent over POST requests. <br><br>

### 4. Enable Application Insights 
Application Insights is a very useful tool to detect anomalies and visualise performance. It can be enabled before or after deployment and the following information can be collected from the endpoint: 

**Enabling Application Insights using Python SDK**<br> 
The logs.py script is used to enable application insights after deployment. 


### 5. Swagger Documentation
Swagger is a tool that helps build, document and consume RESTful web services. It explains what types of HTTP requests that an API can consume like POST and GET. <br><br> 

### 6. Consume Model Enpoints
The script endpoint.py is used to interact with the deployed model after setting the scoring_uri and the primary key. endpoint.py runs against the API and produces JSON output from the model. The endpoint.py in the repository sends two data instances and when run it returns the following output. 

### 7. Create and Publish a Pipeline
**Creating and Running a Pipeline**<br>
The Jupyter Notebook provided is run after configuring necessary details. <br>
Pipelines are a great way to automate workflows. Published pipelines allow external services to interact with them so that they can do work more efficiently.

A new pipeline is created with an AutoML step. The pipeline experiment is run and details are displayed using RunDetails widget. The pipeline run overview shows the run status and details. 
