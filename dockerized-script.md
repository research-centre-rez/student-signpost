## Dockerized script (draft)

To simplify the script usage there is good practice to split code, data and models (if used). Therefore, for a docker image we define standardization of these. Code is packed inside the image (`COPY` in `Dockerfile`), models and data are mounted via `docker run -v`. Both, the data and the model should be mounted read-only (`:ro`). Mount extra folder for the outputs (read+write).

Other issue is the run of the image as a root user.

### Accessible outputs

To guarantee access to the user to the script outputs you should define `--user` param in docker run parameters.

In the HowToUse of a project MUST be defined usage of docker container with this parameters:
```bash
docker run --user $(id -u):$(id -g) 
```

### Data mounting
We define standard for mounting configuration, inputs and outputs as well as what should be in the ouputs. For mounting use these docker params:
```
-v /path/to/inputs/directory:/inputs:ro
-v /path/to/output/directory:/outputs
-v /path/to/config/folder:/app/config:ro
-v /path/to/models/folder:/app/models:ro
```

### Dockerfile rules

0) When you are creating the Dockerfile:
   - Add dependencies one by one to prevent extensive rebuilds. Docker creates layers which means that everything successfully built in previous build is cached and cannot be builded again (especially downloading, building and installation of packages is very time consuming, do it gradually) 
   - Check validity of conainer environment after each build (use interactive mode: put `CMD ["/bin/bash"]` at the end of `Dockerfile` and `-it` in `docker run` command) 
   - Once everything is established (all dependencies finally installed and script is runnable) go to the command aggregation (apt install, pip install as in following example)
   - Do not forget to run tests before deployment do a CI pipeline in Github actions where tests will be run
     - PyTest - Take a look on these first: 
       - pytest outside docker container - https://github.com/marketplace/actions/run-pytest
       - pytest inside docker container - https://stackoverflow.com/questions/78075525/run-pytest-within-docker-container-while-running-github-actions-workflow)
     - Test of the docker build - https://github.com/marketplace/actions/build-and-push-docker-images
1) Use docker layers effectively. Layers (i.e. commands in `Dockerfile`) are reused up to the first layer where change in the output appears. This means that if you put your `COPY` before `apt install` or `pip install` then every change in your code requires reinstallation of packages. 
   - the code `COPY` should be included last (because it is often changed)
   - system packages should be at the beginning of the Docker file (relatively stable layer) 
2) Use `/app` folder for deploying the app
3) If you have multiple scripts inside one docker image use `ENTRYPOINT python` and give comprehensive documentaion to a user (README.md).  
4) If you need to expose some port use different [template](templates.md) (dockerized backend API)

### Example

```dockerfile
FROM python:3.11-bookworm

# Install the system requirements
RUN apt-get update && apt-get install ffmpeg libsm6 libxext6  -y

# Use app folder for deployment
WORKDIR /app

COPY docker/rod-growth/requirements.txt /app/requirements.txt
RUN pip install -r /app/requirements.txt

# If you package wasn't installed in previous step (i.e. it is only script not a package)
# NOTE: use .gitignore for all ignored files. Keep in mind, that the docker image should be as small as possible
COPY . . 

# Define where is your package deployed to the python
ENV PYTHONPATH="/app"

# This is for testing purposes (run in interactive mode if you would like to check your env)
#CMD ["/bin/bash"]
ENTRYPOINT ["python", "./scripts/rods_growth_measurer.py", "-i", "/data/inputs", "-o", "/data/outputs", "--parallel"]

# in case of app testing use this alternative layer
#ENTRYPOINT ["pytest"]
```