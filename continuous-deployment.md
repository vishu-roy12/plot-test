# Continuous Delivery

## Tools to use ##

We can use ArgoCD tool for continuous deployment. ArgoCD is an open source tool, well maintained and easy to deploy tool in kubernetes cluster which can be used for continuous deployment of kubernetes manifest in the cluster. It easily integrated with multiple deployment orchestration tool like Helm and Kustomize. It has got a good UI where you can view and manage the resources easily without even connecting to cluster from your CLI. You can give restricted access to Devs and other teams too and they can manage the tool too and check the status of theor application pods/containers.
There are other tools in the market which we can use in combination with CI/CD like Jenkins, github-actions, codefresh etc., but they lack the UI interface and better log extracts.

## CI/CD process & strategy for application.

We can use github-actions or anyother thirdparty tool for Continuous integration or building our manifests, in kubernetes ecosystem docker images. In this repo i have used github-actions to create the docker image and then push it to the repo, there are several other steps too which can be added into the CI process.

- Continous building of docker images, CI pipeline should be created to build docker images based on release/hot-fix/development branch, it depends on the company architecture. Once the new release is pushed and PR is raised the CI pipeline will trigger to eventually create the docker image including more testing and linting steps.
- We can add many steps in the CI pipeline like unit test and integration test to catch the issue beforehand.
- SOme kind of static analysis tool like sonarqube can be added too to check the quality of the code and code coverage and other security vulnerabilities (CVE).
- Once these stages are passed artifacts can be generated like docker image, we can use image scanning tool too to scan the final prepared image and then push it to the repository.
- After successfully creating the docker image it can be deployed to the lower environment first by using ArgoCD. Once its deployed on lower environment, it can be handed over to QA team for thorough inspection and testing to catch bugs before getting deployed to production environment.
- Once all the tests are passed, prod deployment can be triggered where a new pr is openend with correct image in the deployment file. Once the PR is approved the banch can be merged to main/master. Since we are using ArgoCD so the application on argocd will go out of sync and someone from the Devops/SRE team can sync the app to update the new release image. I don't prefer enaling auto sync on prod environment since deployment on prod needs to be monitored and properly schedule with proper updates to all the stake holders.
- Once the prod deployment is done, the application needs to be monitored and check for any anomaly post deployment.

## Strategy to follow ##

- Create a feature branch
- Create a CI pipeline which triggers when PR is raised on the feature branch.
- Include all the tests and lints in the pipeline. Include OPA policies too for checking kubernetes manifest files.
- Once the docker image is build use ArgoCD cli options to update the image in the manifest to continuous deployment on lower environments.
- Once the deploment os successfull and all the tests are run, trigger the prod deployment pipleine which will raise a PR to change the image to the kubernetes manifests.
- Once the PR is approved th branch can be merged.
- Once the branch is merged, it will make the application on ArgoCD as out of sync.
- Manually sync the production manifest with new image.
- Once the deployment is successfull, monitor the application for any anomaly.