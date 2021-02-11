
# Project 2 - Operationalizing Machine Learning

## Purpose
This project is part of the Udacity Azure ML Engineer Nanodegree.Here, we have a plan to use Azure ML to to predict if a client will subscribe in the banking based on several independent variables that we have. [Bankmarketing Datset](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv).We trained our model using AutoML approach and best algorithm selected and deployed as an webservice to Azure Container Instance. At the end, we automated the procedures by creating and deploying a pipeline.


## Architectural Diagram
These are steps we followed in this project. The diagram below shows the workflow of this project. First, we need to create a Security Principal (SP), then an AutoML experiment is run and the best model have been chosen and deployed, later the deployed model is consumed through the REST endpoint. Finally, we automated he workflow by making a pipeline and publishing it. 

![diagram](https://user-images.githubusercontent.com/40363872/107598953-f35a8780-6bd3-11eb-9c3b-1f25bbdf1a95.png)



### 1. Authentication 
Here, we need to create a Security Principal(SP) to interact with the Azure Workspace.For this experiment the lab-service which Udacity provided has been used. 

### 2. Automated ML Experiment
The steps here are including

In this step, I created an AutoML experiment to run using the [Bank Marketing](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv) Dataset which was loaded in the Azure Workspace, choosing **'y'** that is customer subscribe or not as the dependent variable.

*Figure 1: Bank Marketing Dataset*
![1 Dataset](https://user-images.githubusercontent.com/40363872/107599075-3ae11380-6bd4-11eb-925d-d3bf813731e3.JPG)



For the compute clustering, I used the **Standard_DS12_v2** for the Virtual Machine and 1 as the **minimum number of nodes** and 5 as the **maximum number of nodes**.I ran the experiment using classification, without enabling Deep Learning. The run took some time to test various models and found the best model for the task.
![2 AutoML run1](https://user-images.githubusercontent.com/40363872/107599600-ef2f6980-6bd5-11eb-8f09-74cbeccff9c1.JPG)


The experiment is running, and many models are being trained on the **Bank Marketing** Dataset to find the best one.
![3 AutoML running](https://user-images.githubusercontent.com/40363872/107599652-19812700-6bd6-11eb-8589-3e9e379856e8.JPG)


After a while, the eperiment is complete and I can access the models that were trained and find the best model.
![4 AutoML detail](https://user-images.githubusercontent.com/40363872/107599690-3158ab00-6bd6-11eb-985a-5b26e5f8bb3c.JPG)


The best model for this classification problem was a **Voting Ensemble** model with **0.92** Accuracy.A Voting Ensemble is a machine learning model that trains on an ensemble of numerous models and predicts an output based on their highest probability of chosen class as input. <br>
![6  AutoML Models](https://user-images.githubusercontent.com/40363872/107599715-459ca800-6bd6-11eb-9207-66a3a48b0bba.JPG)

I access the best model to learn more about its metrics and other details.In this section, I can see all of the model's metrics, such as *Accuracy*.
![8  AutoML Run Metrics](https://user-images.githubusercontent.com/40363872/107599745-65cc6700-6bd6-11eb-86d4-5269845125d1.JPG)


Here I added the graphs such as *ROC* and *Confusion Matrix*.
![9 AutoML Best model  confusion matrix](https://user-images.githubusercontent.com/40363872/107599798-9c09e680-6bd6-11eb-9c63-89f8d4fe7835.JPG)


![10 AutoML Best model  ROC](https://user-images.githubusercontent.com/40363872/107599809-a88e3f00-6bd6-11eb-988b-c0cb342885f0.JPG)



### 3. Deploy the Best Model
In this experiment, the best model obtained used the Voting Ensemble algorithm. During deployment, **authentication is enabled**. Authentication is crucial for the continuous flow of operations, as Continuous Integration and Delivery systems rely on uninterrupted flows. The screenshot below shows thet *Key based authentication enabled* as *true*.<br>

The model is deployed using **Azure Container Instance**. Azure Container Instance service uses key-based authentication and is disabled by default. Deploying the best model allows interaction with the HTTP API servie and data can be sent over POST requests. <br><br>

![11  Deploy the best AutoML model](https://user-images.githubusercontent.com/40363872/107599967-26eae100-6bd7-11eb-9b86-597328fc1243.JPG)

The model is successfully deployed, and we can access the model endpoint in the Endpoints section of Azure ML Studio.

![12  Model Deployment success](https://user-images.githubusercontent.com/40363872/107600017-47b33680-6bd7-11eb-8b16-53ec9dacf948.JPG)


### 4. Enable Application Insights 
Application Insights is a very useful tool to detect anomalies and visualise performance. It can be enabled before or after deployment and the following information can be collected from the endpoint.Enabling Application Insights and Logs could have been done at the time of deployment, but for this project we achieved it using Azure Python SDK.
We choose the best model for deployment and enable "Authentication" while deploying the model using Azure Container Instance (ACI). The executed code in logs.py enables Application Insights. "Application Insights enabled" is disabled before executing logs.py.


Running the logs.py script requires interactive authentication, after successfully logging in, we can see the logs in the screenshots above.
![Logging Enabled](https://user-images.githubusercontent.com/40363872/107600164-c3ad7e80-6bd7-11eb-9b24-cd3da4607951.JPG)


Firstly, I edited the logs.py file and set application insight to true and then executed the python script. After running the logs.py the Application insight got enabled in the Azure ML Studio. Since the application insights have been enabled, it's shown as *true* in the properties section
![Details for Endpoint - Copy](https://user-images.githubusercontent.com/40363872/107660545-40267880-6c3d-11eb-9b2e-aa2c0eb719c3.JPG)


Here, we use this tool to detect anomalies performance such as respond time.
![18  Application Insights dashboard](https://user-images.githubusercontent.com/40363872/107600187-d32cc780-6bd7-11eb-866f-d0a8a761b93f.JPG)



### 5. Swagger Documentation
Swagger is a tool that helps build, document and consume RESTful web services. It explains what types of HTTP requests that an API can consume like POST and GET. <br><br>
To consume our best AutoML model using Swagger, we first need to download the **swagger.json** file provided to us in the Endpoints section of Azure Machine Learning Studio.
Then we run the **swagger.sh** and **serve.py** files to be able to interact with the swagger instance running with the documentation for the HTTP API of the model.



Swagger documentation is loaded in the localhost using swagger.json from the deployed model.t This is the input for the /score POST method that returns our deployed model's preditions.

![19  Swagger parameters](https://user-images.githubusercontent.com/40363872/107600283-1f780780-6bd8-11eb-8539-8886cfef75f8.png)

Here, The HTTP POST request is a method used to submit data. The parameters and responses of the HTTP POST
![20  Swagger 2](https://user-images.githubusercontent.com/40363872/107600290-269f1580-6bd8-11eb-99fc-84407f242516.JPG)


### 6. Consume Model Enpoints
Finally, it's time to interact with the model and feed some test data to it. We do this by providing the scoring_uri and the key to the endpoint.py script and running it. 

The script endpoint.py is used to interact with the deployed model after setting the scoring_uri and the primary key. endpoint.py runs against the API and produces JSON output from the model. The endpoint.py in the repository sends two data instances and when run it returns the following output.

![Python Endpoint](https://user-images.githubusercontent.com/40363872/107600428-9ca37c80-6bd8-11eb-93c0-15949c95b5dc.JPG)


After executing the endpoint.py script the model sends follwing response, i benchmarked the endpoint with Apache benchmark.
![Benchmark](https://user-images.githubusercontent.com/40363872/107663192-f12e1280-6c3f-11eb-9794-c71665a432e4.JPG)


### 7. Create and Publish a Pipeline

The Jupyter Notebook provided is run after configuring necessary details. <br>
Pipelines are a great way to automate workflows. Published pipelines allow external services to interact with them so that they can do work more efficiently.
For this step, I used the aml-pipelines-with-automated-machine-learning-step Jupyter Notebook to create a Pipeline. I created, consumed and published the best model for the bank marketing dataset using AutoML with Python SDK.


*After updating the notebook to have the same keys, URI, dataset, cluster, and model names already created, I run through the cells to create a pipeline.*

* Create a compute cluster for python SDK*
![Python SDk2](https://user-images.githubusercontent.com/40363872/107601040-6109b200-6bda-11eb-9206-cf18bce6a73f.JPG)




*The pipeline created in the Pipelines section of Azure ML Studio.*
![Pipeline created](https://user-images.githubusercontent.com/40363872/107664955-ce9cf900-6c41-11eb-8e9f-4516ddb4ee87.JPG)

*Here, I create the experiments*
![Python SKD 1](https://user-images.githubusercontent.com/40363872/107675925-6d7b2280-6c4d-11eb-87d8-13e6b902573f.JPG)


*After the pipeline experiment, Run details are displayed using RunDetails widget. The pipeline run overview shows the run status and details.*

![RunDetails](https://user-images.githubusercontent.com/40363872/107676158-ad420a00-6c4d-11eb-86f2-6bda1794c733.png)

*Pipeline in Azure Studio*
![Pipeline Endpoints](https://user-images.githubusercontent.com/40363872/107601142-a9c16b00-6bda-11eb-8e82-a4ce6fc61816.JPG)

*Pipeline Overview in Azure Studio. Here, I show the Pipeline Overview in the Azure ML Studio. Also, here, the REST endpoint in Azure ML Studio, with a status of ACTIVE is in this part.

![Pipeline run completed](https://user-images.githubusercontent.com/40363872/107601164-bd6cd180-6bda-11eb-9ba4-b0246824dc94.JPG)


## Screen Recording

[Youtube Link](https://youtu.be/bvivpnhP_js)

## Suggestion
I think our accuracy for the model is best, but it was better to enable Deep Learning, definitly it takes more time to run but it can improve our result. Deep learning is the future and work best with different datasets. Also, another think that I can suggest is to solve the dataset with  imbalance issue. Also, I feel it is better before using this dataset, we can use some dimention reduction techniques such Random forest and PCA, then use the new datsets for the AutoMl.

*Error showing Imbalance data*
![Implance data](https://user-images.githubusercontent.com/40363872/107665589-71ee0e00-6c42-11eb-94f7-50b5fa45b705.JPG)








