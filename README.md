# osquery-splunk-dashboards
This is a collection of osquery configs and splunk dashboards that can be used to gain an understanding of your osquery deployment and serve as an investigative tool for host based detection. I will not be releasing these as a Splunk app.  Its XML, just create a new dashboard, go to source and then copy/pasta these in.   Unless you are doing something wild this should "just work." 

## Requirements  
- [Sankey Diagram](https://splunkbase.splunk.com/app/3112/)  - Not strictly required.  Use another visualization if your company wont let you install apps. 
- gtfo.csv as a lookup table in splunk


## Config  
Contains the configs running on AWS EC2 instances across a cloud footprint.  They are almost entirely stock and based off the great [Palantir OSQ configs.](https://github.com/palantir/osquery-configuration)  I have also dropped in an example logrotate configuration file since this is not handled by osquery and you will fill disk with long running instances. 
I chose to decorate all events with AWS metadata to ensure proper attribution to hosts since this is designed to function without a control plane. By leveraging AWS SSM, immutable infrastructure practices, and a golden base image there is little need for a control plane currently.  I can roll instances with new configs when necessary (shoutout Terraform and Packer) and connect directly to hosts for any ad-hoc investigations. 

## Dashboards   
### osq_fim.xml
The FIM dashboard is based off the [Splunk App for OSquery](https://splunkbase.splunk.com/app/3278/).  Dropping an instance id will bring up any file related changes that have happened on the host over the time period chosen.  This is an extremely valuable dashboard for understanding any changes happening to critical files on cloud infra which should not be changing. 

### osq_host_explorer.xml
The host explorer is meant to be a quick reflection of the current state of a host. Dropping an instance id into the explorer will give you tons of information about the host, spanning installed packages, network info, process related info, cron, /etc/hosts, interfaces, and many more. This is typically one of the first things I use to investigate a host when alerts fire.  Because computers gotta compute `process_events`  will generate a ton of noise.  One approach we have implemented is using the [GTFOBins](https://gtfobins.github.io/) site as a mechanism for filtering on riskier binaries for analysis of rare commands being executed. There's likely some improvements to be made here, but overall, this dashboard is very effective for starting searches to dig deeper on your hosts. 

### osq_ops.xml
The operational dashboard is meant to show your osquery deployment at a high level and help ops teams understand if there are issues with your hosts or logging pipeline as well as surface up data that could be worth filtering out to reduce noise in your environment.  

