 # Writing Exercise 

 ## Setup
 1. [Create a Linux VM](https://learn.hashicorp.com/tutorials/vagrant/getting-started-index?in=vagrant/getting-started).
 1. Perform one-step install of Datadog Agent.  In a terminal, run the following command:

 `DD_AGENT_MAJOR_VERSION=7 DD_API_KEY=my_key_value DD_SITE="datadoghq.com" bash -c "$(curl -L https://s3.amazonaws.com/dd-agent/scripts/install_script.sh)"`

 1. Access Datadog via browser: 
   `https://app.datadoghq.com/account/settings`
    and get familiar with the UI.

 # Exercise 1: Collecting Metrics

 ## Add tags in the Agent config file

 1. Edit the configuration file for the Datadog Agent `/etc/datadog-agent/datadog.yaml`.
 1. Uncomment the `tags` line.
 1. Add `jon:odonnell` under the `team:infra` tag. 
 1. Save the file.
 1. Restart the Agent.
   1. `sudo service datadog-agent stop`
   1. `sudo service datadog-agent start`
 1. On the Datadog page in your browser, click **Infrastructure** > **Host Map**.  
 1. Click the **vagrant** tile in the UI. The new tag `jon:odonnell` is visible under **Tags** > **Datadog**.

 ![Host Map showing custom tags](/img/inf_map.png)

  Note: Tag `test:123`, added via the **Edit Tags** button, appears under the **User** field.

 ## Install a database and set up the respective Datadog integration

 1. [Install MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/) on VM.
 1. In your browser, click **Integrations** > **Search**, then type `MongoDB`. 
 1. Click the **+Install** button on the MongoDB tile, then click the **Configuration** tab.
 1. Configure the MongoDB integration:
   1. Open `/etc/datadog-agent/conf.d/mongo.d# nano conf.yaml.example` and uncomment fields to configure the file as follows.

 ```
 init_config:

 instances:
   - hosts:
      - localhost:27017
    username: datadog
    password: MyPassword
    database: admin
    options:
      authSource: admin
 ```
 1. Save this file as `conf.yaml`.
 1. Restart the Agent.
 1. Navigate to your browser and click **Integrations** > **Integrations** to  Verify integration is installed and working.

 ![Integrations page showing MongoDB](/img/mongo_int_success.png)

 1. Click **Dashboards** > **Dashboard List** > **MongoDB - Overview** and see initial MongoDB metrics.

 ![Dashboard displaying intital MongoDB data](/img/mongo_dashboard.png)

 Note: Link to my [MongoDB dashboard](https://app.datadoghq.com/dash/integration/13/mongodb---overview?from_ts=1647372118753&to_ts=1647372418753&live=true).

 ## Create a custom Agent check that submits a metric named "my_metric" with a random value between 0 and 1000.

  1. `cd /etc/datadog-agent/conf.d`.
  1. Create new configuration file, using sample content found docs for reference.

 ``` 
 init_config:

 instances:
   - min_collection_interval: 30

 ```

  1. Save the file as `custom_check.yaml`.  
  1. `cd  ../checks.d`.
  1. Create a new check file, using sample content found docs for reference. Following is a snippet from my check file:
 ```
  class CustomCheck(AgentCheck):
    def check(self, instance):
        self.gauge('my_metric', random.randint(0,1000), tags=['jon:odonnell'] + self.instance.get('tags', []))
 ```
  1. Save file as `custom_check.py`.
  1. Restart the Agent.

 ## Adjust your check's collection interval to submit the metric once every 45 seconds.
 1. Open `/etc/datadog-agent/conf.d/custom_check.yaml`.  Change the value of `min_collection_interval` to `45`.  
 1. Save the file.
 1. Restart the Agent.

 ## Bonus Question: Can you change the collection interval without modifying the Python check file you created?

 Change the interval by editing the value of `min_collection_interval` in `custom_check.yaml`, or via the UI:
 1. Click **Metrics** > **Summary**.
 1. On the resulting Metrics Explorer page, type `my_metric` in the **Metric** field.
 1. Click `my_metric` in the list.  
 1. Click the **Edit** button.
 1. Change the Value in the **Interval** field.

 ![Edit metric interval](/img/change_metric_interval_ui.png)

 1. Click **Save**.

 # Exercise 2: Visualizing Data
 ## Use the Datadog API to create a Dashboard

 cURL command used to create dashboard:

 ```
 curl  -X POST -H "Content-type: application/json" -d '{
      "graphs" : [{
          "title": "New Test dashboard displaying my_metric",
          "definition": {
              "events": [],
              "requests": [
                  {"q": "avg:mongodb.stats.datasize{host:vagrant}"},
                  {"q": "anomalies(avg:mongodb.uptime{host:vagrant},\"basic\",2)"}
              ]
          },
          "viz": "timeseries"
      }],
      "title" : "Writing Test Dashboard", 
      "description" : "This is the dashboard I finally kind of figured out :)", 
      "read_only": "True" 
 }' "https://api.datadoghq.com/api/v1/dash?api_key=abc123&application_key=abc123"
 ```

 ## Access the dashboard from your dashboard list
 1. In your browser, click **Dashboards** > **Dashboard List**.

 1. Click **Writing Test Dashboard** to display the [dashboard](https://app.datadoghq.com/dashboard/zrx-iix-388/writing-test-dashboard?from_ts=1646936739648&to_ts=1647541539648&live=true).

 1. Expand the dropdown menu in the top right corner of the page that says **1h Past 1 Hour**.  Select **5m Past 5 Minutes** to change the timeframe of the dashboard.

 1. Click the graph on the dashboard to expand the options menu.
 1. Click **Send Snapshot**.  
 1. Add a comment explaining the issue, and select the team member you want to notify.  
 1. Click **Submit**.
 
 ![Snapshot email](/img/snapshot_email.png)

 ### Bonus Question: What does the Anomaly graph display?

 A gray band showing the expected  range based on past data.  If data charts outside of this gray band, that would indicate a possible issue.  This seems like a nice feature for being able to quickly spot abnormal data.

 # Exercise 3: Blog Post

 ## Monitor [OpenVPN](https://openvpn.net/) with Datadog!

 With more workers than ever connecting to their office networks via VPN, knowing how many users are connected and how much bandwith they are utilizing has never been more important. A new [integration](https://github.com/byronwolfman/dd-openvpn) developed by Byron Wolfman lets you track key VPN data with Datadog. Use this integration so Datadog can alert you before facing frustrating VPN latency or network overload.

 A second community member, Dennis Webb, has developed an [OpenVPN license tracking](https://github.com/denniswebb/datadog-openvpn) integration. You can easily track how many licenses are used and how many are available with this slick integration.

 ### Configure the Datadog OpenVPN Active User/Bandwith Check
 1.	Open `/etc/openvpn/openvpn.conf` and add the line:
 `management localhost 7505`.
 1.	Save the file and restart OpenVPN. 
 1.	In a terminal, run `telnet localhost 7505` to verify the OpenVPN Management interface is running. (You should see ``>INFO:OpenVPN Management Interface Version 1 -- type 'help' for more info’`) 
 1.	Move `openvpn.yaml` to `/etc/dd-agent/conf.d/openvpn.yaml`.
 1.	Move `openvpn.py` to `/etc/dd-agent/checks.d/openvpn.py`.
 1.	Restart the Datadog Agent.
   1. `sudo service datadog-agent stop`
   1. `sudo service datadog-agent start`

 ### Configure the License Tracking Integration
 Run [`install.sh`](https://github.com/denniswebb/datadog-openvpn/blob/master/install.sh) and you are all set! Now you can track OpenVPN license usage in Datadog!
 
 #### Help us out OpenVPN users!
 These integrations could be extended to monitor additional OpenVPN metrics. Have one in mind? Check out our [Agent Integrations](https://datadoghq.dev/integrations-core/) instructions and consider filing a PR to help expand the coverage of these tools! 
 
 We've only tested them on Linux, but let us know if you've had success using them on another platform!