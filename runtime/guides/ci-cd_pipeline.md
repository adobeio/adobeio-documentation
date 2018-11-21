# CI/CD Pipeline

When it comes to creating a CI/CD pipeline for managing your actions, Adobe I/O Runtime offers a number of tools that can help you build a pipeline that suites your needs. 

Namespaces can be used to separate your different environments: dev, qe, stage or prod. You can create as many namespaces as you want/need. This enables you to deploy the different versions of your actions to different environments, as you promote a given version all the way up to production.

The CLI tools, `wsk` and `wskdeploy`, can help you automate deployments: manage action dependencies and create manifest files that describe your packages/actions. You can read more about using `wskdeploy` [here](creating_actions.md). 

When it comes to exposing your actions as REST APIs, using Swagger definition files helps you to manage the API life cycle: create/edit/delete. You can read more about this [here](creating_rest_apis.md).