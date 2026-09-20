# Workflow Analysis

## 1. What triggers this workflow to run?
The workflow runs when code is pushed to the `main` branch and when a pull
request targeting `main` is opened or updated.

## 2. What are the four main steps this workflow performs?
The `build-and-test` job runs four steps:
1. Checkout code
2. Validate HTML
3. Check links
4. Upload artifact

A separate `deploy` job then runs "Deploy to GitHub Pages" once
`build-and-test` succeeds.

## 3. What does the "Checkout code" step do and why is it necessary?
It uses `actions/checkout@v4` to copy the repository's files onto the runner.
The runner starts empty, so without this step the later steps would have no
code to validate, check, or deploy.

## 4. What is the purpose of the environment configuration?
`environment: github-pages` connects the deploy job to the GitHub Pages
environment so GitHub can track deployments and allow the job to publish the
site. The `url` setting displays the live site address once deployment
finishes.

## 5. How does this automated deployment improve reliability compared to manual deployment?
Every change goes through the same automated checks before release, and the
deploy job only runs if the build-and-test job passes. This reduces human
error, such as forgetting to test or uploading the wrong files, and makes
deployments consistent and repeatable.

## 6. What would happen if you pushed code to a different branch (not main)?
Nothing would run for a plain push, because the push trigger only covers
`main`. A pull request from that branch into `main` would run the
`build-and-test` job, but the deploy job would be skipped because it only
runs on pushes to `main`.