# A Practical CI/CD Framework for Machine Learning at Massive Scale

https://fosdem.org/2020/schedule/event/a_practical_cicd_framework_for_machine_learning_at_massive_scale/

## Why is prod ML so hard?

* specialised hardware (GPU, etc.)
* compiles computation graphs (ETL pipelines, training pipelines, live healing, retries, etc.)
* compliance -- everything needs to reproducible at any point in time, data, learnt params, code, etc.

## Seldon Core

Streamlines ML deployment in K8s.

Takes python model code or artefact and turn into prod REST endpoint.

## Jenkins X

Serverless CICD in Kubernetes.

Not actually related to Jenkins, no Java, pure Go. Just related in name (and potentially team).

## Jenkinx X MLOPs CI

See diagram on the talk slides.

TODO: put slides

## The Delivery (CD) Part

When PR finally gets merged into master (after passing CI part), the model code gets published into **Helm**.

Jenkins X automatically pushes the config from Helm to the staging environment (push as in git push -- it's a git repo).

Then after passing staging, perform **canary** or **shadow** deployments before performing a final push to the prod repo.

Canary -- small % of prod instances use new code (one instance?)
Shadow -- 50/50 deployments to fair A/B test comparison (e.g. for accuracy)

## MLOps Framework Benefits

See slides.
