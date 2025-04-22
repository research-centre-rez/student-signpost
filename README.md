# Signpost

This project contains a bunch of readmes which are here to help you properly setup your project. If you don't know were to start, how to group your files or which routines can help you to create readable, maintainable code here are some recommendations which we use across CVŘ projects.

## Quick guide

1. First of all, you should choose from the project type ... see [templates.md](templates.md).
2. If you never work in team of developers please see [versioning.md](versioning.md), there are instructions how to wrap features and bugfixes into branches and how to use git the same way as we use in CVŘ projects.

There are other requirements for a good code, but we (still) do not have strict policy for every project (especially due to their various nature). But it is good to think about [linting](codestyle.md) and [testing](#project-testing).

Finally, this repo is work in progress (WIP), it is good to discuss the rules here, suggest improvements and participate on their creation.

## Project testing

Writing of tests is a pain but sometimes with sweet reward. It is hard to say when tests are useful or necessary and where are a must. Try to identify these objectives:

1. What is the purpose of the code?
    - Research?
    - Proof of concept?
    - Production code which purpose is to repeatedly process data?
2. How many developers will participate?
    - one man show or a team work?
3. What is the target size of the code?
    - three data classes one script with 4 parameters?
    - 20 API endpoints 40 data classes hundreds of possible run configurations

Tests should warn you when you break something in part of the code which was not changed. But, there is also useful another point of view. Let's say that:
- **unit test** shows how to use a function
- **integration test** shows how input and output data should look like
- **end to end test** validates, that user API is ergonomic

Prepare an infrastructure in your repository for automatic testing with `pytest`. Use for this purpose e.g. GitHub Actions https://docs.github.com/en/actions/use-cases-and-examples/building-and-testing/building-and-testing-python. 

Without the infrastructure you will always skip writing a single test. With prepared infrastructure you decision will be more relevant.

Finally, try to write a test before the function itself - design inputs, write expected output and in between put the non-existent function call. Then write desired function. Once you do it, you will be able to think about testing different way..