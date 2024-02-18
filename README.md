### Jenkins Pipeline for Simple Todo Python based application using SonarQube, Argo CD, Helm and Kubernetes

A simple todo app project 

![Screenshot 2024-02-18 115419](https://github.com/Keerthanachinnu/jenkins-project/blob/main/images/Screenshot%202024-02-18%20115419.png)

## CICD Architecture [GitHub -> Jenkins -> k8s Manifests -> Argo CD -> k8s cluster]

![Screenshot 2024-02-18 115514](https://github.com/Keerthanachinnu/jenkins-project/blob/main/images/Screenshot%202024-02-18%20115514.png)

Here are the step-by-step details to set up an end-to-end Jenkins pipeline for a Python application using SonarQube, Argo CD, Helm, and Kubernetes:

Prerequisites:

   -  Python application code hosted on a Git repository
   -  Jenkins server
   -  Kubernetes cluster
   -  Helm package manager
   -  Argo CD

### Jenkins Pipeline Stages

![Screenshot 2024-02-18 114611](https://github.com/Keerthanachinnu/jenkins-project/blob/main/images/Screenshot%202024-02-18%20114611.png)

### Execute locally and access the application
Make sure to install the dependencies of the project through the requirements.txt file.
```bash
pip install -r requirements.txt
```

Once you have installed django and other packages, go to the cloned repo directory and run the following command
```bash
python manage.py makemigrations
```

This will create all the migrations file (database migrations) required to run this App.Now, to apply this migrations run the following command
```bash
python manage.py migrate
```

### options
Project it self has the user creation form but still in order to use the admin you need to create a super user.you can use the createsuperuser option to make a super user.
```bash
python manage.py createsuperuser
```

And lastly let's make the App run. We just need to start the server now and then we can start using our simple todo App. Start the server by following command
```bash
python manage.py runserver
```

Once the server is up and running, head over to http://127.0.0.1:8000 for the App.