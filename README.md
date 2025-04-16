
## Project testing

Writing of tests is a pain but sometimes with sweet reward. It is hard to say when tests are useful or necessary and where are must. Try to identify these objectives:

1. What is the purpose of the code?
    - Research?
    - Proof of concept?
    - Production code which purpose is to repeatedly process data?
2. How many developers will participate?
    - one man show or a team work?
3. What is the target size of the code?
    - three data classes one script with 4 parameters?
    - 20 API endpoints 40 data classes hundreds of possible run configurations

…

Prepare an infrastructure in your repository for automatic testing with `pytest`. Use for this purpose e.g. GitHub Actions https://docs.github.com/en/actions/use-cases-and-examples/building-and-testing/building-and-testing-python. 

Without the infrastructure you will always skip writing a single test. With prepared infrastructure you decision will be more relevant.