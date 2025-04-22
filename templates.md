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

There are some templates which can be useful as project scaffold:

- research project: [python-research-template](https://github.com/research-centre-rez/python-research-template)
- model training TODO
- (dockerized) script TODO
- (dockerized) backend API TODO

If your type is changed, please refactor the code accordingly.