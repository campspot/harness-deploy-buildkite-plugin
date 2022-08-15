# Harness Deploy Buildkite Plugin

[![GitHub Release](https://img.shields.io/github/release/campspot/harness-deploy-buildkite-plugin.svg)](https://github.com/campspot/harness-deploy-buildkite-plugin/releases)

A [Buildkite plugin](https://buildkite.com/docs/agent/v3/plugins) to kick off [Harness](https://harness.io/) pipelines using the Harness GraphQL API.

## Setup
You will need a Harness API key provisioned, as well as input names and their values that are required to start the Harness pipeline. 
The [Harness docs](https://docs.harness.io/article/s3leksekny-trigger-workflow-or-a-pipeline-using-api)
have more information on how to discover and enumerate required execution and service inputs for a given pipeline. 

## Example

The following Buildkite pipeline will start the `production-app` Harness pipeline in the `app` Harness application.
Execution inputs and service inputs will depoend on your Harness pipeline setup, but included are some examples.

```yaml
  - name: "Start Harness deploy"
    command: 'echo "Starting harness deploy for ''\$ENV'' and commit hash ''\$APP_COMMIT_HASH''"'
    plugins:
      - campspot/harness-deploy#v1.0.0:
          application: "app"
          pipeline: "production-app"
          harness-api-key: ${API_KEY}
          harness-account-id: ${ACCOUNT_ID}
          inputs:
            - name: "env"
              value: "app-${ENV}"
            - name: "commit_hash"
              value: "$APP_COMMIT_HASH"
            - name: "infra_one"
              value: "app-${ENV}-infra-one"
            - name: "infra_two"
              value: "app-${ENV}-infra-two"
          services:
            - name: "app-service-one-name"
              artifact: "app-service-one-artifact-name"
              build: "app-service-one-artifact-build-number"
            - name: "app-service-two-name"
              artifact: "app-service-two-artifact-name"
              build: "app-service-two-artifact-build-number"
```

## Configuration

- `application` (required, string)

  Name of the Harness application containing the pipeline you wish to kickoff

- `pipeline` (required, string)

  Name of the Harness pipeline you wish to kickoff

- `harness-api-key` (required, string)

  Your Harness API key used to access the Harness GraphQL API

- `harness-account-id` (required, string)

  Your Harness account ID

- `wait-for-deploy` (boolean)
  
  Defaults to true. Whether to wait for the Harness deploy to pass/fail

- `inputs` (array)

  Execution inputs that your Harness pipeline requires. Name and value depend on your harness pipeline setup

  - `name` (string)

  The name of the execution input
  - `value` (string)

  The value of the execution input

- `services` (array)

  Service inputs that your Harness pipeline requires. Values depend on your harness pipeline setup. See [Harness docs](https://docs.harness.io/article/s3leksekny-trigger-workflow-or-a-pipeline-using-api) for more information

    - `name` (string)

  The name of the service

    - `artifact` (string)

  The name of the artifact

    - `build` (string)

  The build number of the artifact


## License

MIT (see [LICENSE](LICENSE))