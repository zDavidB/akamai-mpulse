**Prerequisite**: get an Auth Token before creating an annotation, see Create mPulse Auth Token

mPulse allows you to apply annotations or notes against Apps when events happen on the site(s) that you may want to highlight on the dashboards. For example, if there is a site outage or a big sales event that shows up as an anomaly on the dashboards, you may apply an annotation/note that summarises what the event was about, perhaps a link to a Jira ticket or Confluence page detailing the event.

Some events like Property Manager activations are annotated automatically.

Annotations could be built into code deploy pipelines, or they can be created manually.

- Existing Annotations may be updated via the API request

- First create an mPulse Auth token, see Create mPulse Auth Token. Ideally save the Auth token as an env variable, e.g. **X_MPULSE_TOKEN** to be used in the API request below

- Determine which App or Apps need the annotation, the same annotation may be applied to multiple apps in one request. See the App domains page for the domain ids required.

- From a previous annotation request or from the List annotations request, find the Annotation Id of an existing request

- Optionally, determine the (Unix) Epoch time value in microseconds for the start of the event and optionally (ideally) the end of the event, e.g. `2021-09-20T10:55:37+01:00` == `1632131737000`

- Create and execute an API request as below.

- The response should be a 200 with a JSON payload with a single object of "id" and the id value for the annotation.

## curl version
```
curl -X PUT \
     -H "X-Auth-Token: ${X_MPULSE_TOKEN}" \
     -H "Content-type: application/json" \
     --data-binary '{"title":"Application Upgrade"}' \
     https://mpulse.soasta.com/concerto/mpulse/api/annotations/v1/19211111
```


## httpie version
```
http --raw '{"title":"Sterling Upgrade"}' PUT https://mpulse.soasta.com/concerto/mpulse/api/annotations/v1/19211111 X-Auth-Token:${X_MPULSE_TOKEN}

```

The body of the request (`data-binary` or `raw`) may contain any of the fields used in the Create Annotation request

