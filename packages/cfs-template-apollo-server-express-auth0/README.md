This project was bootstrapped with [Create Full Stack](https://github.com/tiagob/create-full-stack).

## Run

```bash
yarn start
```

Spins up postgres in Docker, the Apollo Express server and all clients.

<!-- remove-pulumi-aws-begin -->

## Deploy

### Development

```bash
cd packages/pulumi-aws
pulumi stack select development
pulumi up
```

This sets up Auth0 which is required for authentication locally.

### Production

```bash
cd packages/pulumi-aws
pulumi stack select production
pulumi up
```

<!-- remove-pulumi-aws-end -->

## Setup

_Assumes MacOS_

### Install Homebrew

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

### Install Docker

```bash
brew cask install docker
```

References

- https://stackoverflow.com/a/43365425/709040
- https://docs.docker.com/get-docker/

<!-- remove-mobile-begin -->

### Install and configure Expo CLI

```bash
yarn global add expo-cli
```

If you're new to Expo, register. Expo is free.

```bash
expo register
```

Or login if you have an account

```bash
expo login
```

References

- https://docs.expo.io/workflow/expo-cli/

<!-- remove-mobile-end -->
<!-- remove-pulumi-aws-begin -->

### Install and configure Pulumi CLI

```bash
brew install pulumi
```

If you're new to Pulumi, register at https://app.pulumi.com/signup. Pulumi is free. Then login.

```bash
pulumi login
```

References

- https://www.pulumi.com/docs/get-started/install/

### Install AWS CLI

```bash
brew install awscli
```

If you're new AWS, register at https://portal.aws.amazon.com/billing/signup#/start. Then, if you don't have an access key, create a new one. From https://console.aws.amazon.com/iam/home#/security_credentials goto Access Keys > Create New Access Key

Configure by adding your access key ID and secret access key (Default region name and output format are not required).

```bash
aws configure
```

References

- https://www.pulumi.com/docs/intro/cloud-providers/aws/setup/#using-the-cli

### Configure Auth0

It's recommended you use separate [Auth0 tenants](https://auth0.com/docs/getting-started/the-basics#account-and-tenants) for development and production. However if you prefer to use the same tenant, set the `auth0:domain`, `auth0:clientId` and `auth0:clientSecret` to the same value for the development and production stacks.

#### Development

In your development Auth0 tenant create a Machine to Machine Application

- Applications > CREATE APPLICATION > Machine to Machine Applications
- Give it a name (ex. "pulumi")
- Select the "Auth0 Management API" under "Select an API..." dropdown
- Select "All" scopes

Use the created Machine to Machine Application to set the pulumi configuration

```bash
cd packages/pulumi-aws
pulumi stack select development
pulumi config set auth0:domain XXXXXXXXXXXXXX
pulumi config set auth0:clientId YYYYYYYYYYYYYY --secret
pulumi config set auth0:clientSecret ZZZZZZZZZZZZZZ --secret
```

References

- https://www.pulumi.com/docs/intro/cloud-providers/auth0/setup/#configuring-credentials

#### Production

In your development Auth0 tenant create a Machine to Machine Application

- Applications > CREATE APPLICATION > Machine to Machine Applications
- Give it a name (ex. "pulumi")
- Select the "Auth0 Management API" under "Select an API..." dropdown
- Select "All" scopes

Use the created Machine to Machine Application to set the pulumi configuration

```bash
cd packages/pulumi-aws
pulumi stack select production
pulumi config set auth0:domain XXXXXXXXXXXXXX
pulumi config set auth0:clientId YYYYYYYYYYYYYY --secret
pulumi config set auth0:clientSecret ZZZZZZZZZZZZZZ --secret
```

References

- https://www.pulumi.com/docs/intro/cloud-providers/auth0/setup/#configuring-credentials

### Deploy development on Pulumi

```bash
cd packages/pulumi-aws
pulumi stack select development
pulumi up
```

This sets up Auth0 which is required for authentication locally.

<!-- remove-pulumi-aws-end -->
<!-- remove-manual-config-begin -->

### Configure Auth0

#### Server

From the Auth0 console https://manage.auth0.com/dashboard/

- Create an Auth0 API for the Apollo Server
  - APIs > CREATE API
  - Record the identifier/audience (ex. "server").

In `packages/server/.env.development` fill in the fields

```
AUTH0_AUDIENCE=
AUTH0_DOMAIN=
```

<!-- remove-web-begin -->

#### Web

From the Auth0 console https://manage.auth0.com/dashboard/

- Create a Native Application for the React Native mobile app
  - Applications > CREATE APPLICATION > Native
  - Set "Allowed Callback URLs" to "https://auth.expo.io/@[YOUR EXPO USERNAME]/[YOUR EXPO APP SLUG]"
  - Get YOUR EXPO USERNAME by running `expo whoami`
  - Get YOUR EXPO SLUG from `packages/mobile/app.json` `"slug"`

In `packages/web/.env.development` fill in the fields

```
REACT_APP_AUTH0_CLIENT_ID=
REACT_APP_AUTH0_AUDIENCE=
REACT_APP_AUTH0_DOMAIN=
```

<!-- remove-web-end -->
<!-- remove-mobile-begin -->

#### Mobile

From the Auth0 console https://manage.auth0.com/dashboard/

- Create a Single Page Application for the React website
  - Applications > CREATE APPLICATION > Single Page Web Applications
  - Set "Allowed Callback URLs", "Allowed Logout URLs", and "Allowed Web Origins" to "http://localhost:3000"

In `packages/mobile/.env.development` fill in the fields

```
AUTH0_CLIENT_ID=
AUTH0_AUDIENCE=
AUTH0_DOMAIN=
```

<!-- remove-mobile-end -->
<!-- remove-manual-config-end -->