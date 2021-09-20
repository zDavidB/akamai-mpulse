First obtain an API key from the mPulse application.

The API key is not provided via the Akamai IAM console, the mPulse API key is created automatically for each application in mPulse and most Users in the mPulse console.

The User API keys are the preferred use (IMHO, better tracking and audit).

To access mPulse you must first log into the Akamai Control Center (ACC), when you do and click the mPulse application from the nav menu, an mPulse user id is also created for you and an API key added to your mPulse profile.

We have (had) created a number of service ids that do not have ACC login ids so these may be used in orchestration / pipelines. 


With your API Token, you must first create an Authentication token to performa any other mPulse API actions, to obtain this run the following API request

### curl version

```bash
curl -X PUT -H "Content-type: application/json" \
    --data-binary '{"apiToken":"$MPULSE_API_KEY"}' \
    "https://mpulse.soasta.com/concerto/services/rest/RepositoryService/v1/Tokens"
```

### httpie version

```bash
http PUT https://mpulse.soasta.com/concerto/services/rest/RepositoryService/v1/Tokens apiToken=${MPULSE_API_KEY}
```

Replace **$MPULSE_API_KEY** with your API key for mPulse, or use an env variable to prevent the key from being saved to history if running manually.

The response in either case should be a JSON payload with a single object named "token"

e.g.
```json
{
    "token": "73d9de14-1c12345-99c9-1234-exampletoken"
}
```

Save this Token value, e.g. "73d9de14-1c12345-99c9-1234-exampletoken"

Ideally save this to an env variable


Simple Bash script function (note uses the HTTPie app, adapt if you prefer using curl), also uses jq to extract the token value from the reponse. Adding this into your bash/zsh profile setup allows you to call the function as **get_mpulse_token** and have the auth token saved to an env variable. The function relies on your API key (e.g. **MPULSE_API_KEY**) also being exported as an env variable
```
function get_mpulse_token {
 
    export X_MPULSE_TOKEN="$(http PUT https://mpulse.soasta.com/concerto/services/rest/RepositoryService/v1/Tokens apiToken=$MPULSE_API_KEY | jq -r .token)"
 
}
```
