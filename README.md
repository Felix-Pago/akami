# Akami
Akami is the service responsible to dispatch messages coming from customers, its the central point of the Chat Platform

![e140dc8da24f4d3fb024ffcf39c6feb4](https://user-images.githubusercontent.com/89485796/177841937-52c9e4d5-9007-42e0-945d-2aea96f2c8b8.jpeg)

## How to install Akami

In order to run Akami you will need to install Python 3.9, after this run the following commands:

```sh
python3 -m venv venv/
source venv/bin/activate
pip install -r requirements.txt
```

This will create a virtual environment where all the service dependencies are going to be installed.

## How to run Akami

You can run Akami by issuing the following command:

```sh
python3 main.py
```

You will need to pass several arguments or environment variables to the `main.py` script like:

- `--env` or `ENV`: Possible application environments are: `test`, `development`, `sandbox` and `production`.
- `--config_path` or `CONFIG_PATH`: Relative file path where all the configuration files are stored.
- `--google_application_credentials` or `GOOGLE_APPLICATION_CREDENTIALS`: Relative file path to the GCP .json credentials, you don't need to provide this argument or environment variable in you are deploying the application in GCP.
- `--reload` or `RELOAD`: If you set this argument of environment variable with any value reload flag for FastAPI will be set to True, do not pass this if you want reload flag to be False.

## How to deploy Akami

The deployment of this project is performed on Google Cloud Run (https://cloud.google.com/run) which is a managed compute platform that enables you to run containers that are invocable via requests or events. Cloud Run is serverless: it abstracts away all infrastructure management, so you can focus on what matters most — building great applications.

The deployment is made manually by using Google Cloud Build (https://cloud.google.com/build) on the GCP console. Cloud build is a CI/CD tool that will help us to automatically deploy our application to CLoud Run.

You can find the pipeline instructions that are executed during the deployment at the root folder of this project. In the file called "cloudbuild.yaml". You can also check the DOCKERFILE to see the steps that are performed to build the container (which uses python3.9 and runs a server instance by using Unicorn).

The pipeline build instructions involves the following steps:

- Build the docker image
- Push the Docker image to the container registry (located at Google Container Registry)
- Deploy container image to Cloud Run (Specifying the machine requirements, Environment variables and secrets needed)
- Assign the traffic to the new image created
