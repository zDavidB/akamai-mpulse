**Prerequisite**: get an Auth Token before creating an annotation, see Create mPulse Auth Token

mPulse allows you to apply annotations or notes against Apps when events happen on the site(s) that you may want to highlight on the dashboards. For example, if there is a site outage or a big sales event that shows up as an anomaly on the dashboards, you may apply an annotation/note that summarises what the event was about, perhaps a link to a Jira ticket or Confluence page detailing the event.

Some events like Property Manager activations are annotated automatically.

Annotations could be built into code deploy pipelines, or they can be created manually.

To get a list of the existing annotations

- First create an mPulse Auth token, see Create mPulse Auth Token. Ideally save the Auth token as an env variable, e.g. X_MPULSE_TOKEN to be used in the API request below

- Determine which Domain Id for the App or Apps  you need to list the annotations, the same annotation may be applied to multiple apps in one request. See the App domains page for the domain ids required.

- Next, determine the (Unix) Epoch time value in milliseconds for the start of the annotations and optionally (ideally) the end of the annotations, e.g. '2021-09-20T10:55:37+01:00" == 1632131737000

- Create and execute an API request as below.

- The response should be a 200 with a JSON payload with a JSON payload listing all the annotations for the matching criteria

Note the JSON payload for each request

## curl version
```shell
curl -X GET \
     -H "X-Auth-Token: ${X_MPULSE_TOKEN}" \
     "https://mpulse.soasta.com/concerto/mpulse/api/annotations/v1?date-start=1473720858000&domain=12345"
```

## httpie version
```
http GET https://mpulse.soasta.com/concerto/mpulse/api/annotations/v1 date-start==1473720858000 domain==12345,525252 X-Auth-Token:${X_MPULSE_TOKEN}
```

Example response
```
{
  "annotations": [
    {
      "id": 1258378,
      "title": "mPulse Service Incident",
      "text": "mPulse data may be missing during this period (20:00 to 22:15 UTC) due to a service incident. More info can be found here: https://community.akamai.com/customers/s/feed/0D54R00007DkF2aSAF",
      "start": 1595966400000,
      "end": 1595974500000,
      "source": "REST API",
      "type": "MP_SYSTEM_EVENT",
      "lastModified": 1596462474790,
      "domains": []
    }
  ]
}
```

## Advanced response processing

Taking the response and turn into a table

```
http GET https://mpulse.soasta.com/concerto/mpulse/api/annotations/v1 date-start==1627994989000 X-Auth-Token:${X_MPULSE_TOKEN}  | jq -r '(["Id          ","Title", "Start    ","End","Source   ","Domain Name     ", "Domain Id "] | (., map(length*"-"))), (.annotations[] | [.id, .title, .user //"-", (.start / 1000 | strftime("%Y-%m-%d %H:%M:%S UTC")), (.end / 1000 | strftime("%Y-%m-%d %H:%M:%S UTC")), .source, .domains[].name, .domains[].id]) | @tsv' | column -t -s "$(printf '\t')"

```
Uses jq to post-process the response, converting the Epoch time values into ISO dates, then formats into Tab separated values, finally uses the column function to turn into a table.

