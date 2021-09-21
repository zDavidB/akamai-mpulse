Ought to put details in the Collection docs, but didn't.

Get the API key/token from mPulse and add it to the Collection Variables > apiToken var 
(**NOTE;** if you add it to the INITIAL VALUE column and share the collection everyone will see your token, 
put the token in the CURRENT VALUE field and it stays local to you)

After that, run the **Authorisation** request to get the Auth Token. This is saved to the Collection Variables for you.

Then update the **Annotate** > Pre-request Script tab.
- set `start_date` of the event - note the time format is in UTC/GMT
- set `end_date` of the event - note the time format is in UTC/GMT
- set `title` of the event - short and snappy
- set `text` of the event - not too much detail, maybe a link to a Jira ticket or wiki page with more detail
- set `domain` the event occured on - get the domain id from either your predefined list, or the mPulse console (view the App panel source and search for "domainId") or list the existing annotations.
The properties are saved to Collection variables and then used in the Body of the request.
There are various tests in the request to check the annotation is successful.

The annotation id is saved to a Collection Variable, so you can execute the **Get An Annotation** request right 
away and view the results (note the human formatted date time of the event is written to console.

You can also **Update An Annotation**, this uses the annotation id saved in the Collection Variables, 
change it as required for the update, then amend the properties at the start of the Pre-request Script tab as per the **Annotate** request.
