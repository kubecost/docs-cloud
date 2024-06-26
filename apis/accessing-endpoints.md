# Accessing API Endpoints

Kubecost Cloud has select endpoints available for API access. The default API access endpoint will be `api.app.kubecost.com`. Obtaining access involves generating an API key value, which is then used to generate a personal access token. The token, together with your team ID, will then allow you to make queries.


## Access tutorial

### Step 1: Generating an API key value

To get started, visit *Settings* in the Kubecost Cloud UI, then under 'My API Key', select *Generate Key*. A key value will generate in a text box with a lifespan of one month. Save this value, as it will not be retrievable later. Only one key can exist at any given time for a given user. Remove the existing key to generate a new one. 

### Step 2: Generating a personal access token

With your generated API key value, run the following command to generate a personal access token:

```
curl --location --request POST 'https://auth.app.kubecost.com/api/token' \

--header 'X-API-Key: <KEY_VALUE>'
```

The generated token is short-lived and expires after ten minutes. This token can be used in token authentication for API requests to all available Kubecost endpoints.

### Step 3: Generating your team ID value

You will also need to retrieve your team ID value. This is a value associated with your [Kubecost Cloud team](/installation-and-onboarding.md#managing-teams). You can find this ID at the bottom of *Settings*, in the 'Team Info' section.

![Team ID](/images/teaminfo.png)

### Step 4: Using the API endpoints

To verify your token is working, authenticate to the Kubecost Cloud API access route using the personal access token generated in step 2 and the team ID retrieved in Step 3. For example:

```
curl --location 'https://api.app.kubecost.com/query/allocation/query?window=7d&aggregate=cluster&idleByNode=false&idle=false' \


--header 'Authorization: Bearer <TOKEN_VALUE>' \
--header 'Team-Id: <TEAM_ID>'
```

Keep in mind that the `TOKEN_VALUE` must be generated every ten minutes while using the API. 

For a complete list of Kubecost Cloud APIs, see our [API Directory](/apis/api-directory/api-directory.md).

