**Prerequisite**: get an Auth Token before creating an annotation, see Create mPulse Auth Token

mPulse allows you to apply annotations or notes against Apps when events happen on the site(s) that you may want to highlight on the dashboards. For example, if there is a site outage or a big sales event that shows up as an anomaly on the dashboards, you may apply an annotation/note that summarises what the event was about, perhaps a link to a Jira ticket or Confluence page detailing the event.

Some events like Property Manager activations are annotated automatically.

Annotations could be built into code deploy pipelines, or they can be created manually.

To get a specific existing annotations

- First create an mPulse Auth token, see Create mPulse Auth Token. Ideally save the Auth token as an env variable, e.g. **X_MPULSE_TOKEN** to be used in the API request below

- Determine the id of an existing Annotation

- Create and execute an API request as below.

- The response should be a 200 with a JSON payload with a JSON payload listing all the annotations for the matching criteria

Note the JSON payload for each request

## curl version
```shell
curl -X GET \
     -H "X-Auth-Token: ${X_MPULSE_TOKEN}" \
     https://mpulse.soasta.com/concerto/mpulse/api/annotations/v1/19211111 
```

## httpie version
```
http GET https://mpulse.soasta.com/concerto/mpulse/api/annotations/v1/19211111 X-Auth-Token:${X_MPULSE_TOKEN} 
```

Example response
```
{
    "domains": [
        {
            "id": 12335,
            "name": "www.example.co.uk_pm"
        },
        {
            "id": 99999,
            "name": "www.test-site.com_pm"
        }
    ],
    "end": 1631160000000,
    "id": 1921111,
    "lastModified": 1631260867126,
    "source": "REST API",
    "start": 1631142300000,
    "text": "Application Upgrade and Migrate - CHG0112345",
    "title": "Application update",
    "type": "USER_ENTERED",
    "user": "john.smith@example.com"
}
```

## Advanced response processing

Taking the response and turn into a table

```
http GET https://mpulse.soasta.com/concerto/mpulse/api/annotations/v1/19211111 X-Auth-Token:${X_MPULSE_TOKEN}  \
    | jq -r '. | [ {id: .id, title: .title, text: .text, start: (.start / 1000 | strftime("%Y-%m-%d %H:%M:%S UTC")), end: (.end / 1000 | strftime("%Y-%m-%d %H:%M:%S UTC")), lastModified:(.lastModified / 1000 | strftime("%Y-%m-%d %H:%M:%S UTC")), source: .source, type: .type, user: .user, domain: .domains[]}]'

```
Uses jq to post-process the response, converting the Epoch time values into ISO dates, then formats into Tab separated values, finally uses the column function to turn into a table.

