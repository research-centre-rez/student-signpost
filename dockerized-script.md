## Dockerized script (draft)

To simplify the script usage there is good practice to cut off the code (packed into docker image) and the data and model (if used). Therefore, for the image we define standardization of these.

Other issue is the run of the image as a root user.

### Accessible outputs

To guarantee access to the user to the script outputs you should define `--user` param in docker run parameters.

In the HowToUse of a project MUST be defined usage of docker container with this parameters:
```bash
docker run --user $(id -u):$(id -g) 
```

### Data mounting
We define standard for mounting configuration, inputs and outputs as well as what should be in the ouputs. For mounting use this docker params:
```
-v /path/to/inputs/directory:/inputs:ro
-v /path/to/output/directory:/outputs
-v /path/to/config/folder:/app/config:ro
-v /path/to/models/folder:/app/models:ro
```

### Dockerfile rules

0) When you are creating the Dockerfile:
   - Do it by dependency after dependency to prevent extensive rebuilds. 
   - Check env after each build in interactive mode `CMD ["/bin/bash"]`. 
   - Once everything is established (all dependencies finally installed and script is runnable) go to the command aggregation (apt install, pip install)
   - Do not forget to run tests before deployment (do a CI pipeline in Github actions where tests will be run)
1) Use docker layers effectively (code should be included last)
2) Use /app folder for deploying the app
3) Copy only files you need for the project run (if you have several scripts in your project create dockerfile for each)

```dockerfile
FROM python:3.11-bookworm

# Install the system requirements
RUN apt-get update && apt-get install ffmpeg libsm6 libxext6  -y

# Use app folder for deployment
WORKDIR /app

COPY docker/rod-growth/requirements.txt /app/requirements.txt
RUN pip install -r /app/requirements.txt

# Copy only files you need

# Configuration of the run. Config folder is mounted
COPY config/rg_config.json /app/config/rg_config.json
# Include the root package
COPY palivo/__init__.py /app/palivo/__init__.py
# Include the main package
COPY palivo/rods_growth /app/palivo/rods_growth
# Add utils of the project
COPY palivo/utils/__init__.py /app/palivo/utils/__init__.py
COPY palivo/utils/config_utils.py /app/palivo/utils/config_utils.py
COPY palivo/utils/logger_tools.py /app/palivo/utils/logger_tools.py
# Include your script
COPY scripts/rods_growth_measurer.py /app/scripts/rods_growth_measurer.py

# Define where is your package deployed to the python
ENV PYTHONPATH="/app"

# This is for testing purposes (run in interactive mode if you would like to check your env)
#CMD ["/bin/bash"]
ENTRYPOINT ["python", "./scripts/rods_growth_measurer.py", "-i", "/data/inputs", "-o", "/data/outputs", "--parallel"]

# in case of app testing use this alternative layer
#ENTRYPOINT ["pytest"]
```