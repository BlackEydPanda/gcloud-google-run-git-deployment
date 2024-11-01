### Google Run Function Deployment testground
This REPO was created to test Google Run Function Deployment.
In order to see the deployment check the workflows file for the python deployment. Could it be cleaner? Possibly, but mission accomplished so who cares, its a test ground luv.
GCP project has been since removed so pushing into main will not redeploy anymore. Based on Kyle Venn's answer on StackOverflow: https://stackoverflow.com/questions/77337407/deploy-gen2-google-cloud-functions-with-github-actions


### Discovered issues and what to look for to fix them
#### Permmissions
With the permissions listed in the stackoverflow post, there might be an issue where GCloud throws a 403 with an error message along the lines of `The caller does not have permission`. This is a bit of an inconsistent behaviour because on this Repos initial creation the `Cloud Functions Developer` and the `Service Account User` seemed to be enough permissions to allow deployment.
However, if you run into the permissions issue mentioned above, the following 2 permissions seem to fix the issue:
* `Cloud Functions Admin` -> I believe this is needed to allow the GitHub Action to alter the function surrounding permissions and such (eg. region and resources and whatnot)
* `Viewer` -> This I believe is needed to allow the GH Actions to access the list of functions. 

It is worth noting that whenever you update the permissions, the actual change in GCP is made with a delay, I would recommend waiting about a minute before retrying workflow.

#### Function already exists
This is an issue that can arise whenever you have created a function manually through the GCP Console Interface and then attempt to redeploy changes to that specific function. In that case GCP sees it as a conflict and will not allow you to update it. As of right now, I have not found a fix that will not give too much permissions to GH Actions, so the workaround right now is to simply delete the existing function or simply rename the one defined by your GH Actions.
