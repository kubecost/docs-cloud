# Accessing API Endpoints

Kubecost Cloud has select endpoints available for API access. The default API access endpoint will be `api.app.kubecost.com`. Obtaining access involves generating an API key value, which is then used to generate a personal access token, which is used to retrieve your team ID value. The token and team ID value will then allow you to make queries.

## Access tutorial

### Step 1: Generating an API key value

To get started, visit *Settings* in the Kubecost Cloud UI, then under 'My API Key', select *Generate Key*. A key value will generate in a text box with a lifespan of one month. Save this value, as it will never be retrievable later. Kubecost will not save this value. You can remove the key value, and consequently access to it, by selecting *Remove Key*. This will not provide a warning after being selected, so be careful to not permanently delete your key.

### Step 2: Generating a personal web token

With your generated API key value, run the following command to generate a personal access token:

```
curl --location --request POST 'https://app.kubecost.com/api/token' \
--header 'X-API-Key: <KEY_VALUE>'
```

The generated token is short-lived and expires after ten minutes. This token can be used in token authentication for API requests to all available Kubecost endpoints.

### Step 3: Generating your team ID value

You will also need to retrieve your team ID value. This is a value associated with your [Kubecost Cloud team](/installation-and-onboarding.md#managing-teams). Run the following command with your generated token:

```
curl --location 'https://auth.stage.kceng.dev/api/user' \
--header 'Authorization: Bearer <TOKEN_VALUE>'
```

### Step 4: Authenticating

Finally, you need to authenticate your token. Use the following command:

```
curl --location 'https://api.stage.kceng.dev/query/allocation/query?window=7d&aggregate=cluster&idleByNode=false&idle=false' \
--header 'Authorization: Bearer <TOKEN_VALUE>' \
--header 'Team-Id: <TEAM_ID>'
```

For a complete list of Kubecost Cloud APIs, see our [API Directory](/apis/api-directory.md).

