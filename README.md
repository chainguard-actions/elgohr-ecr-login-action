# ECR-Login-Action

[![Actions Status](https://github.com/elgohr/ecr-login-action/actions/workflows/release.yml/badge.svg?branch=main)](https://github.com/elgohr/ecr-login-action/actions)

This Action for Docker logs into [AWS ECR](https://aws.amazon.com/de/ecr/) and gets the timely bound credentials for Docker.

## Usage

## Example pipeline

```yaml
name: Publish Docker
on: [ push ]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    - name: Login to ECR
      id: ecr
      uses: elgohr/ecr-login-action@v3
      with:
        access_key: ${{ secrets.AWS_ACCESS_KEY }}
        secret_access_key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        region: ${{ secrets.AWS_REGION }}
    - name: Publish to Registry
      uses: elgohr/Publish-Docker-Github-Action@v5
      with:
        name: myDocker/repository
        username: ${{ steps.ecr.outputs.username }}
        password: ${{ steps.ecr.outputs.password }}
        registry: ${{ steps.ecr.outputs.registry }}
```

## Mandatory arguments

`access_key` is the AWS access key  
`secret_access_key` is the AWS secret access key  
`region` the region you would like to login to  

## Outputs
`username` the username for logging in  
`password` the password for logging in  
`registry` the registry to log in to  
`docker_name` the name of the docker image for building and pushing

## Privacy

This Action contacts Chainguard's licensing server to verify authorization. Connection metadata (IP address, GitHub repository identifier, timestamp, and any metadata encoded in the auth token) is transmitted to Chainguard, Inc. even if authorization is denied in accordance with our [Privacy Notice](https://www.chainguard.dev/legal/privacy-notice)
