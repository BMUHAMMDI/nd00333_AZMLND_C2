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
   1. First, go to Datasets tab in ML Studio page and create dataset called "BankMarketing Dataset", figure 1 shows the registered datasets in our project workspace.
   
   ![Picture2](https://user-images.githubusercontent.com/52258731/103155518-bd8f4a00-47b1-11eb-9456-6441f11f4d8c.png)
   
   2. Next, go to Automated ML tab in Azure ML page and create Automated ML run called “ml-experiment-1”, within this step we will upload the dataset "BankMarketing Dataset" and configure the compute cluster. Once the experiment is successfully created, we will see the experiment in the experiment section and a new model is created. figure 2 shows the experiment as completed.  
   
   ![Picture3](https://user-images.githubusercontent.com/52258731/103155623-8c634980-47b2-11eb-9c8d-8a4b07f02eb3.png)
   
   3. After the experiment run completed, go to the Automated ML section, and find the recent experiment with a completed status. Click on it. Go to the "Model" tab and select the best model from the list and click it. Above it, a triangle button (or Play button) will show with the "Deploy" word. Click on it. Fill out the form with a meaningful name and description. For Compute Type use Azure Container Instance (ACI). Enable Authentication. Deployment takes a few seconds. After a successful deployment, a green checkmark will appear on the "Run" tab and the "Deploy status" will show as succeed.
   
   4. Then, we will enable application insights of the deployed model by editing the provided logs.py script using the terminal. It should be similar to this:
   
   
   ```
   from azureml.core import Workspace
from azureml.core.webservice import Webservice

# Requires the config to be downloaded first to the current working directory
ws = Workspace.from_config()

# Set with the deployment name
name = "bankmarketing-model"

# load existing web service
service = Webservice(name=name, workspace=ws)

# enable application insight
service.update(enable_app_insights=True)

logs = service.get_logs()

for line in logs.split('\n'):
    print(line)
 ```  
 Figure 4 shows the output of the script run and figure 5 shows the application insights enabled in the details tab of the endpoint [deployed model].
 
   ![Picture4](https://user-images.githubusercontent.com/52258731/103155715-3e9b1100-47b3-11eb-96ff-51db326ac5c2.png)
   
   ![Picture5](https://user-images.githubusercontent.com/52258731/103155777-c719b180-47b3-11eb-881f-40feab7ad92e.png)

 
## Screen Recording
*TODO* Provide a link to a screen recording of the project in action. Remember that the screencast should demonstrate:

## Standout Suggestions
*TODO (Optional):* This is where you can provide information about any standout suggestions that you have attempted.
