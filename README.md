# relay

A shim to get webhook POSTs from Docker Hub and post them to [Slack](http://slack.com).


## Setup

### Put your project up on Docker Hub

First, make a [Dockerfile](http://docs.docker.com/reference/builder/) and add it to the root of your repository.
You can test it locally with

    sudo docker build .

after [installing Docker](https://docs.docker.com/installation/).

Then push your project to [Docker Hub](https://hub.docker.com/).


### Make a new Incoming Webhook in Slack's Integrations

![](http://i.imgur.com/y2FWZme.png)

Copy the webhook URL.

![](http://i.imgur.com/FPVIDeq.png)


### Webserver-side configuration

Set up dependencies:

    sudo apt-get install python-requests python-twisted

Set shell variables for your webhook URL and a port (which can be any integer between 1024 and 65535; we have 8080 here as an example):

    export slack_url='https://hooks.slack.com/services/PUT/YOURS/HERE'
    export relay_port=8080

Clone this repo.
Make a file `channel_selector.json` in the repo directory that maps the repo path on Docker Hub to a channel in your Slack chat.
For example, to map <https://registry.hub.docker.com/u/psathyrella/ham/> and [my fork of ham](https://registry.hub.docker.com/u/psathyrella/ham/) to the bcell channel, the file is:

    {
        "psathyrella/ham":"#bcell",
        "matsen/ham":"#bcell"
    }


### Run

After running

    python relay.py

visit <http://your-webserver-goes-here.com:8080/relay> and you should get the message "Relay is running."

Now, when your build pushes to Docker Hub you should get a nice notification in Slack.
