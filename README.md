# Dockerize and deploy machine learning model as REST API using Flask
A simple Flask application that can serve predictions machine learning model. Reads a pickled sklearn model into memory when the Flask app is started and returns predictions through the /predict endpoint. You can also use the /train endpoint to train/retrain the model.

# Steps for deploying ML model

1. Install Flask and Docker
2. Serialise your scikit-learn model (this can be done using Pickle, or JobLib)
3. [optional] add column names list to scikit object ex: rf.columns = ['Age', 'Sex', 'Embarked', 'Survived']
4. Create a separate flask_api.py file which will build the web service using Flask
    5. To run python flask_api.py <port>
    6. Go to http address to check if its working
5. Create a dockerfile which does the below items
    6. Install ubuntu, python and git
    7. Clone code repo from git or move local python code to /app in container 
    8. Set WORKDIR to /app
    9. Install packages in requirements.xt
    10. Expose the port for flask enpoint
    11. Define ENTRYPOINT as python main.py 9999
6. Build  docker image
7. Run docker container 
8. Make a http POST call with some data, and receive the prediction back using postman or python requests library.
9. Push the docker container to docker registry / ship to production

1. ### Install PIP requirements
    FYI: The code requries Python 3.6+ to run 
    ```
    pip install -r requirements.txt
    ```
2. ### Running API

    ```
    python main.py <port>
    ```

3. ### Endpoints
    ### /predict (POST)
    Returns an array of predictions given a JSON object representing independent variables. Here's a sample input:
    ```
    [
        {"Age": 14, "Sex": "male", "Embarked": "S"},
        {"Age": 68, "Sex": "female", "Embarked": "C"},
        {"Age": 45, "Sex": "male", "Embarked": "C"},
        {"Age": 32, "Sex": "female", "Embarked": "S"}
    ]
    ```
    
    and sample output:
    ```
    {"prediction": [0, 1, 1, 0]}
    ```
        
    ### /train (GET)
    Trains the model. This is currently hard-coded to be a random forest model that is run on a subset of columns of the titanic dataset.
    
    ### /wipe (GET)
    Removes the trained model.


# Docker commands
1. Build docker image from Dockerfile

    ```docker build -t <app name> .```
2. Run the docker container after build

    ```docker run ml_app -p 9999 # -p to make the port externally avaiable for browsers```

3. Show all running containers
    
    ```docker ps```

    a. Kill and remove running container
    
     ```docker rm <containerid> -f ```

4. Open bash in a running docker container (optional)

    ```docker exec -ti <containerid> bash```


Appendix
- http://docs.python-requests.org/en/latest/user/quickstart/#more-complicated-post-requests
- https://www.ibm.com/developerworks/webservices/library/ws-restful/
- https://blog.hyperiondev.com/index.php/2018/02/01/deploy-machine-learning-model-flask-api/
- https://medium.com/@amirziai/a-flask-api-for-serving-scikit-learn-models-c8bcdaa41daa