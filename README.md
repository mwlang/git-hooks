# git hooks

This is just a collection of git hooks I use for my own project management and are meant to be installed on the git server.  Once set up, will keep both pivotal tracker and slack channel informed of changes as developers commit their work to the repos.

With these scripts in place, any time a commit is made and pushed to your git server repo:
* A message is sent to slack with the commit message and who committed the changes.  
* A comment with the commit message is added to all stories referenced in the commit message.
* If the commit was to the integration branch, then the story is marked as accepted, otherwise, it's marked as delivered.

## To Install

### Install the chain script
1. copy chain/hook-chain to the server under the repo's `hooks/hook-chain` and make it executable
2. sym-link it to hooks/post-receive

### Install the pivotal tracker script

1. Copy pivotal-tracker/post-receive-pivotal to the repo's `hooks/post-receive.001-pivotal`
2. Make it executable `chmod +x hooks/post-receive.001-pivotal`

And configure...

Get your API-token from under your Profile's menu (near end) and project-id from under your project's settings menu.

    git config hooks.pivotal-tracker.api-token asdflasf930sd0230ds0ds20dgh0h03
    git config hooks.pivotal-tracker.project-id 12345
    git config hooks.pivotal-tracker.integration-branch develop

### Install the slack script

1. Copy slack/post-receive-slack to the repo's `hooks/post-receive.002-slack`
2. Make it executable `chmod +x hooks/post-receive.002-slack`

Add an Incoming WebHooks integration in your Slack by going to e.g.

    https://my.slack.com/services/new/incoming-webhook

Make note of the webhook URL.

    git config hooks.slack.webhook-url 'https://hooks.slack.com/services/...'
    
More options for the slack hook here:  https://github.com/chriseldredge/git-slack-hook

NOTE: If you're wishing to update a private channel, leave user and channel info unconfigured on the git server side and configure on the Webhook side within Slack.

### When all Done

You should see something like this:

~~~
 ...
 -rwxr-xr-x. 1 git  git   280  hook-chain
 lrwxrwxrwx. 1 root root   10  post-receive -> hook-chain
 -rwxr-xr-x. 1 git  git  8763  post-receive.001-pivotal
 -rwxr-xr-x. 1 git  git  1745  post-receive.002-slack
 ...
~~~
 
## CREDITS

* slack post-receive:           https://github.com/chriseldredge/git-slack-hook
* pivotal-tracker post-receive: https://gist.github.com/Bokenator/42aeb02e0aefe55d5a9a
* hook-chain:                   http://stackoverflow.com/questions/8730514/chaining-git-hooks