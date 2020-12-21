# Data & AI Tech Immersion Workshop – Product Review Guide and Lab Instructions

## AI, Experience 3 - Better models made easy with Automated Machine Learning

- [Data &amp; AI Tech Immersion Workshop – Product Review Guide and Lab Instructions](#data-amp-ai-tech-immersion-workshop-%e2%80%93-product-review-guide-and-lab-instructions)
  - [AI, Experience 3 - Better models made easy with Automated Machine Learning](#ai-experience-3---better-models-made-easy-with-automated-machine-learning)
  - [Technology overview](#technology-overview)
  - [Scenario overview](#scenario-overview)
  - [Exercise 1: Creating a model using automated machine learning](#exercise-1-creating-a-model-using-automated-machine-learning)
    - [Task 1: Create an automated machine learning experiment using the Portal](#task-1-create-an-automated-machine-learning-experiment-using-the-portal)
    - [Task 2: Review the experiment run results](#task-2-review-the-experiment-run-results)
    - [Task 3: Register the Best Model](#task-3-register-the-best-model)
  - [Exercise 2: Understanding the automated ML generated model using model explainability](#exercise-2-understanding-the-automated-ml-generated-model-using-model-explainability)
    - [Task 1: Setup New Compute Instance](#task-1-setup-new-compute-instance)
    - [Task 2: Upload the starter notebook](#task-2-upload-the-starter-notebook)
  - [Exercise 3 (Optional): Train and evaluate a model using Azure Machine Learning](#exercise-3-optional-train-and-evaluate-a-model-using-azure-machine-learning)
    - [Task 1: Upload and open the starter notebook](#task-1-upload-and-open-the-starter-notebook)
  - [Wrap-up](#wrap-up)
  - [Additional resources and more information](#additional-resources-and-more-information)

## Technology overview

Azure Machine Learning service provides a cloud-based environment you can use to prep data, train, test, deploy, manage, and track machine learning models.

![Azure Machine Learning overview](media/intro.png 'Azure Machine Learning overview')

Azure Machine Learning service fully supports open-source technologies. So you can use tens of thousands of open-source Python packages with machine learning components. Examples are PyTorch, TensorFlow, and scikit-learn. Support for rich tools makes it easy to interactively explore and prepare data and then develop and test models. Examples are [Jupyter notebooks](https://jupyter.org) or the [Azure Machine Learning for Visual Studio Code](https://marketplace.visualstudio.com/items/itemName/ms-toolsai.vscode-ai/overview) extension.

By using Azure Machine Learning service, you can start training on your local machine and then scale out to the cloud. With many available [compute targets](https://docs.microsoft.com%/en-us/azure/machine-learning/service/how-to-set-up-training-targets), like Azure Machine Learning Compute and [Azure Databricks](https://docs.microsoft.com/en-us/azure/azure-databricks/what-is-azure-databricks), and with [advanced hyperparameter tuning services](https://docs.microsoft.com/en-us/azure/machine-learning/service/how-to-tune-hyperparameters), you can build better models faster by using the power of the cloud. When you have the right model, you can easily deploy it in a container such as Docker. So it's simple to deploy to Azure Container Instances or Azure Kubernetes Service. Or you can use the container in your own deployments, either on-premises or in the cloud. For more information, see the article on [how to deploy and where](https://docs.microsoft.com/en-us/azure/machine-learning/service/how-to-deploy-and-where).

You can manage the deployed models and track multiple runs as you experiment to find the best solution. After it's deployed, your model can return predictions in [real time](https://docs.microsoft.com/en-us/azure/machine-learning/service/how-to-consume-web-service) or [asynchronously](https://docs.microsoft.com/en-us/azure/machine-learning/service/how-to-run-batch-predictions) on large quantities of data. And with advanced [machine learning pipelines](https://docs.microsoft.com/en-us/azure/machine-learning/service/concept-ml-pipelines), you can collaborate on all the steps of data preparation, model training and evaluation, and deployment.

Azure Machine Learning service also includes features that [automate model generation and tuning](https://docs.microsoft.com/en-us/azure/machine-learning/service/tutorial-auto-train-models) to help you create models with ease, efficiency, and accuracy. Automated machine learning is the process of taking training data with a defined target feature, and iterating through combinations of algorithms and feature selections to automatically select the best model for your data based on the training scores. The traditional machine learning model development process is highly resource-intensive, and requires significant domain knowledge and time investment to run and compare the results of dozens of models. Automated machine learning simplifies this process by generating models tuned from the goals and constraints you defined for your experiment, such as the time for the experiment to run or which models to blacklist.

## Scenario overview

In this experience you will learn how the automated machine learning capability in Azure Machine Learning (AML) can be used for the life cycle management of the manufactured vehicles and how AML helps in creation of better vehicle maintenance plans. To accomplish this, you will train a Linear Regression model to predict the number of days until battery failure using Automated Machine Learning in Jupyter Notebooks.

## Exercise 1: Creating a model using automated machine learning

In this exercise, you will create a model that predicts battery failure from time-series data using the visual interface to automated machine learning in an Azure Machine Learning workspace.

### Task 1: Create an automated machine learning experiment using the Portal

1. Navigate to your Azure Machine Learning workspace in the Azure Portal. Select `Overview` in the left navigation bar and then select **Launch studio**. Alternatively, you can navigate directly to the new [Azure Machine Learning studio](https://ml.azure.com/)

   ![Navigate to Azure Machine Learning studio](./media/aml-01.png)

2. Select the Machine learning workspace as part of the sign-in process.

    ![select the machine learning workspace](./media/aml-02.png)

3. Select **Automated ML** in the left navigation bar.

   ![Select Automated ML](./media/002_AutomatedML.png)

4. Select **New automated ML run** to start creating a new experiment.

   ![New automated ML run](./media/003_CreateAutomatedMLRun.png)

5. Select **Create dataset** and choose the **From web files** option from the drop-down.

   ![Create dataset from local file](./media/004_NewDataset_FromURL.png)

6. Fill in the training data URL in the `Web URL` field: `https://quickstartsws9073123377.blob.core.windows.net/azureml-blobstore-0d1c4218-a5f9-418b-bf55-902b65277b85/training-formatted.csv`, make sure the name is set to `training-formatted-dataset`, and select **Next** to load a preview of the parsed training data.

   ![Training data web URL](./media/005_Dataset_BasicInfo.png)

7. In the `Settings and preview` page select **All files have same headers**, and scroll to the right to observe all of the columns in the data.

   ![Reviewing the training data](./media/006_ReviewDataFile.png)

8. Select **Next** to check the schema and then confirm the dataset details by selecting **Next** and then **Create** on the confirmation page.

   ![Reviewing the schema of training data](./media/007_TrainingDataSchema.png)

   ![Confirm dataset creation](./media/007_ConfirmDataset.png)

9. Now you should be able to select the newly created dataset for your experiment. Select the `training-formatted-dataset` dataset and select **Next** to move to the experiment run details page.

   ![Select the dataset](./media/008_SelectDataset.png)

10. You will now configure the Auto ML run basic settings by providing the following values for the experiment name, target column and training compute:

   - Experiment name: **automl-regression**
   - Target column: select **Survival_In_Days**
   - Select training compute target: : select **aml-compute-cpu**

   ![Setup Auto ML experiment basic settings](./media/009_SetupExperiment.png)

11. Select **Next** and select **Regression** in the `Task type and settings` page.

    ![Select Regression task type](./media/010_TaskTypeForExperiment.png)

12. Select **View additional configuration settings** to open the advanced settings section. Provide the following settings:

    - Primary metric: **Normalized root mean squared error**
    - Training job time(hours): **3**
    - Exit criterion > Metric score threshold: **0.09**
    - Validation > Validation type: **k-fold cross validation**
    - Validation > Number of Cross Validations: **5**
    - Concurrency > Max concurrent iterations: **1**

    ![Configuring the Advanced Settings as described](./media/011_TaskConfigurationSettings.png)

    > Note that we are setting a metric score threshold to limit the training time. In practice, for initial experiments, you will typically only set the training job time to allow AutoML to discover the best algorithm to use for your specific data.

13. Select **Save** and then **Finish** to begin the automated machine learning process.

    ![Start Automate ML run](./media/012_CreatingExperiment.png)

14. Wait until the `Run status` becomes **Running** in the `Run Detail page`. Save the  **Run ID: AutoML_xxx** to be used later in Exercise #2 Task #2.

    ![Preparing experiment](./media/012_PreparingExperiment.png)

### Task 2: Review the experiment run results

1. The experiment will run for about _15 minutes_. While it runs and once it completes, you should check the `Models` tab on the `Run Detail` page to observe the model performance for the primary metric for different runs.

   ![Review run details - graph view](./media/aml-03.png)

2. In the models list, notice at the top the iteration with the best **normalized root mean square error** score. Note that the normalized root mean square error measures the error between the predicted value and actual value. In this case, the model with the lowest normalized root mean square error is the best model.

   ![Review run details - table view](./media/022_RunDetails3.png)

   > Note that we have set a metric score threshold to limit the training time. As a result you might see only one algorithm in your models list.

3. Select the **Algorithm name** of the best performing model to view the model details.

   ![Open model details](./media/023_model_Summary.png)

4. Select **View all other metrics** to review all the model performance metrics.

   ![Review run metrics](./media/aml-04.png)

### Task 3: Register the Best Model

1. From the `Model details` page, select **Download** as shown and save the folder with model files on your local disk. Remember to **unzip** the folder.

   ![The Download best model link](./media/032_DeployBestModel.png)

2. You need to register the best model with the Azure Machine Learning model registry so that you can retrieve it later when you want to use it for scoring. Select **Models** in the left navigation pane.

    ![The Models page](./media/033_Models.png)

3. Select **Create** to open the `Register a model` page. Provide the following information and then select **Register**.

    - Name: `battery-life-predictor-model`

    - Description: `Predict the number of days until battery failure.`

    - Model framework: `AutoML`

    - Framework version: `0`

    - Model file or folder: Select **Upload folder**

    - Browse and select the downloaded model folder from the previous step.

   ![Register Model](./media/034_RegisterModel.png)

4. Once the registration process has completed, you will be prompted with the message `Success: Model battery-life-predictor-model registered successfully.` in the notification area at the top. Now you should be able to view this model in the **Models** list.

   ![Viewing the list of models in the Azure Machine Learning workspace](media/035-automl-registered-model.png)

5. If you see your model in the above list, you are now ready to continue on to the next exercise.

## Exercise 2: Understanding the automated ML generated model using model explainability

### Task 1: Setup New Compute Instance

To complete this task, you will use an Azure Compute Instance and Azure Machine Learning.

If you have not already created the `tech-immersion` compute instance in Azure Machine Learning studio follow these steps. If you already have it in your environment, continue with **Task 2**.

1. To get started, sign-in to the Azure Portal, navigate to your Azure Machine Learning workspace and select **Launch the new Azure Machine Learning studio**. Alternatively, you can sign in directly to the [Azure Machine Learning studio](https://ml.azure.com).

2. Navigate to the `Compute` section by selecting the option on the left navigation menu.

3. Under the `Compute Instances` tab, select **New** to create the compute instance. Name it `tech-immersion`, select `Standard_DS3_V2` for VM type and select **Create**. Wait a few minutes until the compute instance is fully provisioned.

    > **Note**: If the `Compute Instance names should be unique within an Azure Region` notification appears, choose a different name that is unique to your environment.

4. Back to the `Compute Instances` tab, select **Refresh** if you are not able to see `tech-immersion` yet. After the compute instance is listed, select the **Jupyter** link.

   ![Open NotebookVM](media/212-OpenNotebookVM.png)
   
5. On the **IMPORTANT NOTE: Always use trusted code** pop-up, Check **Yes, I understand** and click on **Continue**.

      ![Open NotebookVM](media/ai3notebook.png)

### Task 2: Upload the starter notebook

1. Download the notebook on your local disk from the following URL:

   https://github.com/solliancenet/tech-immersion-data-ai/blob/master/lab-files/ai/3/explain-automl-model.ipynb

   Select **Raw** to view the text version of the file and then right-click in the browser and save the content locally as  `explain-automl-model.ipynb`.

2. In the Jupyter Notebook environment configured in **Task1**, navigate to the `Files` tab to view the root folder content. If you see a folder named after your user name, use that to upload notebooks.

3. Select the **Upload** menu and browse for the notebook downloaded in step 1.

   ![Upload notebook](media/05.png 'Upload')

4. Press **Upload** to start uploading the notebook.

   ![The Upload files from Computer dialog](media/upload-notebook-01.png 'Upload files from Computer')

5. In the listing, select the Notebook you just uploaded (`explain-automl-model.ipynb`) to open it.
Please select Kernel **Python 3.6 - Azure ML** if you are prompted with a Kernel not found exception.

6. Follow the instructions within the notebook to complete the experience.

## Exercise 3 (Optional): Train and evaluate a model using Azure Machine Learning

### Task 1: Upload and open the starter notebook

In this exercise, you will use compute resources provided by Azure Machine Learning to remotely train a set of models using Automated Machine Learning, evaluate the performance of each model and pick the best performing model to deploy as a web service. You will perform this lab using an Azure Machine Learning Compute Instance. The model you train here is created using automated machine learning just as you did in exercise 1, except instead of using the visual interface in the Azure Machine Learning studio, you setup the model training using Python.

1. Download the notebook on your local disk from the following URL:

   https://github.com/solliancenet/tech-immersion-data-ai/blob/master/lab-files/ai/3/predict-battery-life-with-AML.ipynb

   Select **Raw** to view the text version of the file and then right-click in the browser and save the content locally as  `predict-battery-life-with-AML.ipynb`.
   
2. Download the yaml on your local disk from the following URL:

   https://github.com/solliancenet/tech-immersion-data-ai/blob/master/lab-files/ai/3/automl_dependencies.yml

   Select **Raw** to view the text version of the file and then right-click in the browser and save the content locally as  `automl_dependencies.yml`.

3. In the Jupyter Notebook environment navigate to the `Files` tab to view the root folder content. If you see a folder named after your user name, use that to upload notebooks.

4. Select the **Upload** menu and browse for the notebook and yaml file downloaded in step 1 and step 2.

   ![Upload notebook](media/05.png 'Upload')

5. Press **Upload** to start uploading the notebook and the yaml file.

   ![The Upload files from Computer dialog](media/06.png 'Upload files from Computer')

6. In the listing, select the Notebook you just uploaded (`predict-battery-life-with-AML.ipynb`) to open it.
Please select Kernel **Python 3.6 - Azure ML** if you are prompted with a `Kernel not found` exception.

7. Follow the instructions within the notebook to complete the experience.

## Wrap-up

Congratulations on completing the Auto ML experience.

To recap, you experienced:

1. How to use automated machine learning from the Azure Machine Learning workspace to simplify the process of getting to a performant model.
2. Using Auto ML to train multiple models by using remote capabilities provided by compute targets.
3. Capturing and querying the telemetry of training runs using an Experiment.
4. Retrieving the best model created from an Auto ML session.
5. Registering the best model with the Model Registry, which enables versioning and makes the model file available for deployment to a web service.
6. Understanding the model using the model interpretability features of the Azure Machine Learning Python SDK.

## Additional resources and more information

To learn more about the Azure Machine Learning service, visit the [documentation](https://docs.microsoft.com/azure/machine-learning/service)
