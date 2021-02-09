
# Project 2 - Operationalizing Machine Learning


*The key steps in this projects are including:

**Key Steps**
<ol>
<li>Authentication</li>
<li>Automated ML Experiment</li>
<li>Deploy the best model</li>
<li>Enable logging</li>
<li>Swagger Documentation</li>
<li>Consume model endpoints</li>
<li>Create and publish a pipeline</li>
</ol>


## Key Steps
### 1. Authentication 
For this experiment the lab which Udacity provided has been used. This step is skipped since this account is not authorized to create a security principal.

### 2. Automated ML Experiment
The steps here are including

**Dataset Description**<br>
**Uploading and Registering Dataset**<br>
**Compute Cluster Description**<br>
**AutoML Configuration and Run**<br>
**Choosing the Best Model**<br>
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
