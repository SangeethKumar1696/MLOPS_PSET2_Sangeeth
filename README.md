MLOPS_PSET2_Sangeeth
==============================

PSET2 MLOPS

Project Organization
------------

LICENSE
Makefile             <- Used to install the dependencies
README.md            <- Top level Readme file
assets
   |-- .gitignore
   |-- metrics.json  <-  Gives the metrics of the executed model i.e the value of rmse, mae and r2
data
   |-- .gitignore
   |-- external
   |   |-- .gitkeep
   |-- processed
   |   |-- .gitkeep
   |-- raw.dvc        <- Used to track the changes in the data by git
dvc.lock              <- Used by the DVC to detect when the stages or their dependencies have changed
dvc.yaml              <- Used to define the pipelines that runs when the dvc repro command is used after the initial run
params.yaml           <- Used to defining the parameters required for training the model and testing purposes
requirements.txt      <- Required for installing the necessary packages to run the ML model
src
   |-- data
   |   |-- .gitkeep
   |   |-- config.py  <- Used to configure the dataset paths that are required in the next step
   |   |-- create_dataset.py <- This is used to split the given dataset wine-quality.csv into train and test dataset and this is further used for training and testing the model.
   |-- features
   |   |-- .gitkeep
   |   |-- config.py  <- Used to configure the paths that are required in the next step
   |   |-- create_features.py <- Extracts the features and the labels required for the training step
   |-- models
   |   |-- .1.gitkeep
   |   |-- .gitkeep
   |   |-- config.py   <- Used to configure the paths that are required in the next step 
   |   |-- evaluate_model.py <- Used to evaluate the model by the given parameters in the params.yaml file
   |   |-- train_model.py   <- Used to train the model based on the parameters specified in the params.yaml file and saves the model.pkl file

--------

Steps to be followed
---------------------

Step 1: Create Git repo create DagsHub repo: https://dagshub.com
Step 2: install DVC dvc init configure dvc: 
        dvc remote add origin https://dagshub.com/SangeethKumar1696/MLOPS_PSET2_Sangeeth.dvc 
        dvc remote modify origin --local auth basic 
        dvc remote modify origin --local user SangeethKumar1696  
        dvc remote modify origin --local password $DAGSHUB_TOKEN
        dvc pull -r origin
        dvc add data/raw
        dvc push -r origin    
Step 3: DVC run once all the necessary files for training and testing of models are created
        dvc run -f -n prepare -d src/data/create_dataset.py -o assets/data python src/data/create_dataset.py
        dvc run -f -n featurize -d src/features/create_features.py -d assets/data -o assets/features python src/features/create_features.py
        dvc run -f -n evaluate -d src/models/evaluate_model.py -d assets/features -d assets/models -p model_type -M assets/metrics.json python src/models/evaluate_model.py
        
        Running the aboce steps creates up the dvc.yaml file with the updated stages/pipelines
        From the next time, "dvc repro" command would be sufficient when the model parameters/datasets get updated
Step 4 : To commit the updates to the git.
        git add
        git commit
        git push
Step5   : ML flow based tracking
        mlflow.set_tracking_uri("https://dagshub.com/SangeethKumar1696/MLOPS_PSET2_Sangeeth.mlflow") tracking_uri = mlflow.get_tracking_uri() print("Current tracking uri: {}".format(tracking_uri))




<p><small>Project based on the <a target="_blank" href="https<-//drivendata.github.io/cookiecutter-data-science/">cookiecutter data science project template</a>. #cookiecutterdatascience</small></p>
