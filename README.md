If you are applying to be a Techical Writer at [Datadog](https://www.datadoghq.com/), you are in the right spot.

<img src="https://repository-images.githubusercontent.com/2967233/246a3700-b83c-11e9-9960-8b03925fc6f7" width="500" height="500" alt="Logo">

## Instructions

* Read the [prerequisites](https://github.com/jeremy-lq/hiring-engineers/blob/tech-writer/README.md#prerequisites) and [references](https://github.com/jeremy-lq/hiring-engineers/blob/tech-writer/README.md#references) before the exercises.
* Fork this repo.
* Complete your responses in the `answers.md` file of your forked repo.
* Feel free to add links to your dashboard and screenshots inline with your responses.
* Commit as much code as you need to support your responses.
* Create a pull request and submit the PR link in Greenhouse.

If you have any questions, create an issue in this repository and contact Kaylyn Sigler.

## Prerequisites

You can utilize [any OS/host](https://app.datadoghq.com/account/settings#agent) to complete this exercise. However, we recommend one of the following approaches:

* Spin up a fresh Linux VM via Vagrant or another tool so you don’t run into any OS or dependency issues. To set up a Vagrant Ubuntu LTS VM, see the [instructions](https://github.com/jeremy-lq/hiring-engineers/blob/tech-writer/README.md#vagrant).
* Utilize a containerized approach with Docker for Linux and our dockerized Datadog Agent image.

When you are ready, sign up for a [two-week trial](https://app.datadoghq.com/signup) and select “Datadog Recruiting Candidate” in the **Company** field. If you have already signed up for a trial, but it is running out, let us know by responding to the "Take Home Test from Datadog" email and we can extend it.
The Datadog Agent should start reporting metrics from your local machine. 

## Exercise 1: Collecting Metrics

* Add tags in the Agent config file. Include a screenshot of your host and its tags on the **Host Map** page in Datadog.
* Install a database on your machine (such as MongoDB, MySQL, or PostgreSQL) and set up the respective Datadog integration for that database.
* Create a custom Agent check that submits a metric named "my_metric" with a random value between 0 and 1000.
* Adjust your check's collection interval to submit the metric once every 45 seconds.

**Bonus Question**: Can you change the collection interval without modifying the Python check file you created?

## Exercise 2: Visualizing Data

Utilize the Datadog API to create a Dashboard that contains:

* Your custom metric (`my_metric`) scoped over your host.
* Any metric from the Integration on your Database with the anomaly function applied.

Please include the script you used to create your Dashboard in your submission.

After you create your Dashboard, access the Dashboard from your Dashboard List.

* Set the Dashboards's timeframe to the past 5 minutes.
* Take a snapshot of this graph and use the @ notation to send it to yourself.

**Bonus Question**: What does the Anomaly graph display?

## Final Exercise: Blog Post

The Datadog community has documented a substantial number of [high-quality integrations and libraries](https://docs.datadoghq.com/developers/libraries/). Select one as a topic.

Write a blog post that announces your topic and explains the benefits it offers our users/community. Thank the contributor for their efforts and include images, code samples, and diagrams where applicable.

This blog post should demonstrate your ability to write about a new topic and dive into its configuration, usage, and best practices. 

## References

### Getting Started in Datadog
* [Getting Started](https://docs.datadoghq.com/getting_started/)
* [Graphing](http://docs.datadoghq.com/graphing/)
* [Monitoring](https://docs.datadoghq.com/monitors/)

### The Datadog Agent and Metrics
* [Guide to the Agent](http://docs.datadoghq.com/agent/)
* [Datadog Docker-image Repo](https://hub.docker.com/r/datadog/docker-dd-agent/)
* [Writing an Agent Check](https://docs.datadoghq.com/developers/write_agent_check/)
* [Datadog API](https://docs.datadoghq.com/api/)

### APM
* [Datadog Tracing](https://docs.datadoghq.com/tracing)
* [Introduction to Flask](http://flask.pocoo.org/docs/0.12/quickstart/)

### Vagrant
 * [Setting Up Vagrant](https://www.vagrantup.com/intro/getting-started/)

### Questions
* [Datadog Help Center](https://help.datadoghq.com/hc/en-us)
