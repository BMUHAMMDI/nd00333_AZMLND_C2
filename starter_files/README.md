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

## Future Work

  We can add more data or use the feature engineering technique to add more columns which may give us better result in our model training. Also, this project use ACI service to deploy the model which is known by its fast and simplicity and as a future improvement we can try to use AKS service that can expand but it will take more effort.

## Key Steps
   ### Part 1: Deploy model in Azure ML Studio 
   1. First, go to Datasets tab in ML Studio page and create dataset called "BankMarketing Dataset", figure 1 shows the registered datasets in our project workspace.
   
   ![Picture2](https://user-images.githubusercontent.com/52258731/103155518-bd8f4a00-47b1-11eb-9456-6441f11f4d8c.png)
   
   2. Next, go to Automated ML tab in Azure ML page and create Automated ML run called “ml-experiment-1”, within this step we will upload the dataset "BankMarketing Dataset" and configure the compute cluster. Once the experiment is successfully created, we will see the experiment in the experiment section and a new model is created. figure 2 shows the experiment as completed.  
   
   ![Picture3](https://user-images.githubusercontent.com/52258731/103155623-8c634980-47b2-11eb-9c8d-8a4b07f02eb3.png)
   
   3. After the experiment run completed, go to the Automated ML section, and find the recent experiment with a completed status. Click on it. Go to the "Model" tab and select the best model from the list and click it. Above it, a triangle button (or Play button) will show with the "Deploy" word. Click on it. Fill out the form with a meaningful name and description. For Compute Type use Azure Container Instance (ACI). Enable Authentication. Deployment takes a few seconds. After a successful deployment, a green checkmark will appear on the "Run" tab and the "Deploy status" will show as succeed.
   
   4. Then, enable application insights of the deployed model by editing the provided logs.py script using the terminal. Before that, we have to download the config.json file from the top left menu in the Azure portal and move this file to the same directory of logs.py. Modify logs.py script to be like this:
   
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

  5. After that, download a Swagger JSON file for deployed model from the details tab of endpoint and move it to the same directory of serve.py and swagger.sh.
     [Note: make sure Docker is installed and running on your computer before this step]
     
   6. Run two scripts:
   - serve.py will start a Python server on port 8000. This script needs to be right next to the downloaded swagger.json file. NOTE: this will not work if                  swagger.json is not on the same directory.
   
  - swagger.sh which will download the latest Swagger container, and it will run it on port 80. If you don't have permissions for port 80 on your computer, update the script to a higher number (above 9000 is a good idea).
  
  7. Next, go to http://localhost/ which should have Swagger running from the container (as defined in swagger.sh). we changed the port number from 80 t0 9001, so, we'll use http://localhost:9001 to reach the local Swagger service.
  
  8. On the top bar, where petsore.swagger.io shows, change it to http://localhost:8000/swagger.json, then hit the Explore button. It should now display the contents of the API for the deployed model as shwon in figure 6.
  
  ![Picture6](https://user-images.githubusercontent.com/52258731/103157463-18ca3800-47c4-11eb-8c84-61a94ae2a3fb.png)
  
  9. Now, we'll start consuming the deployed model, in Azure ML Studio, head over to the "Endpoints" section and find our endpoint. In the "Consume" tab, of the endpoint, a "Basic consumption info" will show the endpoint URL and the authentication types. Take note of the URL and the "Primary Key" authentication type.
  
  10. Next, using the provided endpoint.py replace the scoring_uri and key to match the REST endpoint and primary key respectively. The script issues a POST request to the deployed model and gets a JSON response that gets printed to the terminal.Running it should produce similar results as figure 7 and A data.json should be created.
  
  ![Picture7](https://user-images.githubusercontent.com/52258731/103157556-f2f16300-47c4-11eb-8362-70d93497b088.png)
  
  11. Finally, to get a summary about the response performance of the deployed model we'll update the provided benchmark.sh file with the same REST endpoint and primary Key. Running it with command [bash benchmark.sh] should produce similar results as figure 8.
  
  ![Picture8](https://user-images.githubusercontent.com/52258731/103157642-2f718e80-47c6-11eb-86dd-02b0dde5405c.png)
  
   ### Part 2: Publish an ML Pipeline using Python SDK
   1. Upload the provided notebook aml-piplines-with-automated-machine-learning-step.ipynb to Azure ML studio and update experiment, dataset, and compute cluster to match the existing Automated ML run. 
  2.	Run through the cells to create, publish, and consume the pipeline
  3. The following screenshots show the created pipeline as figure 9, the pipeline endpoint in Azure ML studio as figure 10, the dataset with AutoML as figure 11, the published pipeline overview where the status is active as figure 12, and the output of “Use RunDetails Widget” for the pipeline run as figure 13.

![Picture9](https://user-images.githubusercontent.com/52258731/103157974-e58aa780-47c9-11eb-86ae-51cf206c5fe7.png)

![Picture10](https://user-images.githubusercontent.com/52258731/103157980-fd622b80-47c9-11eb-910f-bf903ff85cb1.png)

![Picture11](https://user-images.githubusercontent.com/52258731/103157993-15d24600-47ca-11eb-8c67-8381fcf41324.png)

![Picture12](https://user-images.githubusercontent.com/52258731/103158010-40240380-47ca-11eb-8816-e8d8fe1f88bc.png)

![Picture13](https://user-images.githubusercontent.com/52258731/103158018-592cb480-47ca-11eb-8ffd-0b5ec69cd7c7.png)
 
## Screen Recording
https://www.youtube.com/watch?v=SKPvYNLECiQ

