# Connect-Live-Objects-to-Splunk
Follow and analyze your IoT data with Splunk. Get and use Splunk to understand and value your Live Objects IoT data (free version available!)



Splunk to value your Live Objects IoT data

    “Splunk’s core offering collects and analyzes high volumes of machine-generated data. It was developed in response to the demand for comprehensible and actionable data reporting for executives outside a company’s IT department.”
    Wikipedia

Get and easily install Splunk

Splunk Free is for individual use. Splunk Entreprise & Splunk Cloud give you multi users and distributed capabilities.

To download free version & see the compared functionalities go to https://www.splunk.com/en_us/software/features-comparison-chart.html
then connect HEC Live Objects / Splunk
Configuration of HEC in Splunk

First, connect you with a browser on your Splunk Web instance at :

https://[DNS name]:443/

for instance: https://splunk.francecentral.cloudapp.azure.com:443/

Go to « Settings / Data Inputs / http Event Collector / Add new »

Then add a new HEC token (http Event Collector) with (in the following example, no sourcetype has been used and data are pushed into “main” index)

Click on « Global Settings » button:

    Click on “Enabled” to activate “All Tokens”
    Select the index you want (for instance “main”)
    If you have no valid SSL certificate, then uncheck « Enable SSL » (the Splunk self-signed certificate does not work with Live Objects’ HTTP push)
    Change the value of the field « HTTP Port Number », and choose one compatible with Live Objects’ HTTP Push feature. For instance « 8443 »

After clicking on “Save”, get the “Token Value” that will be used in the next step.

It is possible to test the service with curl:

curl -k http://[DNS name]:8443/services/collector/event -H "authorization: Splunk [Token Value]" -d '{"event": "hello world"}'

For instance :

curl -k http://splunk.francecentral.cloudapp.azure.com:8443/services/collector/event -H "authorization: Splunk d487f324-fbf6-4eb3-807e-cd962db26f89" -d '{"event": "hello world"}' )

If successful you should get this:

{"text":"Success","code":0}

Link Live Objects to it

On Live Objects web interface, go to « Data / Routing », then click on « + Add a routing rule » button: throw next steps, give the routing the name you want, but do not change the default values.

Then choose « + HTTP Push »

Set the values of the following fields:

    URL : http://[DNS name]:8443/services/collector/event
    HTTP Headers : “authorization” and “Splunk [Token Value]”
    Message body : choose “A Mustache formatted message” and set the value : {“event”: “{{value}}”}

then value you data

That’s all, then extract the value from your data

