# Operationalizing Machine Learning

  In this project, we will use Azure ML studio to configure an Automated ML experiment, deploy the best model, enable logging to monitor and collect data from the deployed model, and interact with this model. Next, we will use Python SDK to create, publish, and consume a pipeline.


## Architectural Diagram

In the following diagram, we will define all the steps of our project from start to end, and give an overview of each step: 

![Capture11](https://user-images.githubusercontent.com/52258731/103152216-d50d0980-4796-11eb-8e89-d7554df934e0.JPG)

-	Creating compute cluster: configure a compute cluster to manage the resource of experiments through project steps. 
-	Training AutoML model: create an experiment using Automated ML in Azure ML Studio. 
-	Deployment of the best model: deploy the best model from the generated Automated ML models.
-	Enabling logging: to enable Application Insights and retrieve logs of the deployed model.
-	Consuming the Best Model via HTTP API: consume the deployed model using Swagger.
-	Creating Pipeline Using Python SDK: create and schedule ML pipeline run. 
-	Publishing and Consuming Pipeline: publish and interact with a pipeline via an HTTP API endpoint.

## Future Work:

  We can add more data or use the feature engineering technique to add more columns which may give us better result in our model training. Also, this project use ACI service to deploy the model which is known by its fast and simplicity and as a future improvement we can try to use AKS service that can expand but it will take more effort.

## Key Steps
   ### Part 1: Deploy model in Azure ML Studio 



## Screen Recording
*TODO* Provide a link to a screen recording of the project in action. Remember that the screencast should demonstrate:

## Standout Suggestions
*TODO (Optional):* This is where you can provide information about any standout suggestions that you have attempted.
