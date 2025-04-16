In general there is [PEP 8](https://peps.python.org/pep-0008/). It is highly recommended to be PEP-8 compatible wherever is possible. If you want, use a linter to verify this. 

# Code structure guidelines

First of all try to identify type of your project (the structure vary accordingly)

1. Research or fast prototyping 
    - i.e. mostly notebooks, generated charts and reports
    - focus is on algorithms and approaches
2. Model training, testing and deployment 
    - testing of various architectures, hyper-parameter search, precision/recall/F1
    - focus on models and their evaluation
3. Script with CLI (Dockerized) 
    - data & metadata processing with defined output format
    - focus on the data, repeatability and reproducibility
4. Backend API (Dockerized)
    - focus on the API and code quality
5. If you don’t know ask your supervisor.

If your type is changed, please refactor the code accordingly. Look in our [templates](templates.md) which you can use as project scaffold.

## Non-code components

There are other components except the code which **require versioning** as well as the code, because every referenced data processing should be replicable somewhere in the future. These components typically are:

- 3rd party packages
    - python ecosystem uses for this purpose `requirements.txt`, `pyproject.toml`, `environment.yml`, `Pipfile`, `python_version` & `pyenv` and many others
    - it is important to record
        - which version of python was used for development/testing/deployment
        - which architecture was used (arm, x86_64, …)
        - which packages and their version were used
    - preferred variant is `pyproject.toml` + app dockerization
    - please spend some time studying
        - https://setuptools.pypa.io/en/latest/userguide/
        - https://docs.conda.io/en/latest/
        - https://python-poetry.org/
        - https://github.com/astral-sh/uv
        - https://github.com/pyenv/pyenv
        - https://pip.pypa.io/en/stable/
        - https://virtualenv.pypa.io/en/latest/
- environmental variables
    - for this purpose we use `dotenv` package
- command line instructions (run parameters)
    - as a part of `argparse` routine there is easy to dump the CLI parameters into `logger` or `MLFlow`
- configurations (json and yaml files with stored param values)
    - are part of the `git` repo
- data - images, metadata, derived outputs
    - there are versioning tools for data like [DVC]([https://dvc.org](https://dvc.org/)) or DataChain
- models - trained neural networks
    - for this purpose is recommended to use [MLFlow]([https://mlflow.org](https://mlflow.org/)) or [WanDB](https://wandb.ai/site/)

## Folders structure

Keep your repo clean. Here is suggested high level repository structure:

```bash
.run / bin              # build scripts, generators, CLI confgurations etc.
config                  # configuration files belongs here
docker                  # Dockerfile, docker-compose.yaml
documentation           # e.g. to the HW used in project, protocols
notebooks               # 
openapi                 # API definitions
src / {{package_name}}  # python code
tests                   # pytest tests
.env.example            # default environment setup with possible variants for various machines
pyproject.toml          # 3rd party dependencies declarations
README.md               # explains what is in the repo and how to use it
```
