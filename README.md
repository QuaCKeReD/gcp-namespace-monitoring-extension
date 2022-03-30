# Google Cloud Monitor (Custom Extension) - WIP

Custom extension to get GCP service metrics using Google Cloud Monitoring API (python library)
- Works for any metric/service (namespace_list.txt file attached for reference)
- Single google project (Quick change to work with multiple projects in a subscription)
- Tested in demo environment

# Install
Install python library (Python 3+)
```
pip3 install --upgrade google-cloud-monitoring
 
pip3 install pyyaml==5.4.1
```

# Config
1. Install as custom extension (Bundle all files into GCPMonitor folder and copy to machineagent monitors folder)
2. Update env variable (GOOGLE_APPLICATION_CREDENTIALS) in gcp_monitor.sh to point to location of access key file.
3. Configure config.yml (example below)
   1. Sample namespace list attached (Add namespace in the format given in example file)
   2. (Optional, but recommended) - Identify label for namespace configured above - (One time activity to refine default config.yml file. Once it is configured for common services, we don't need to do this again and can simply comment services which we don't want to monitor. Please share the updated config.yml file if testing on new services.)
      1. Uncomment following line in python script - 
         1. `#print(result.resource.labels, result.metric.labels, result.resource.type, result.metric.type, result.points[0].value.double_value, result.points[0].value.int64_value)`
      2. Execute ./gcp_monitor.sh file, which will print label names for metric along with Custom metric path 
      3. Choose the labels you are interested in to be part of metric path, modify config.yml file and test to see changes in Custom metric path.
         1. Example;
            1. `{'zone': 'australia-southeast1-a', 'project_id': 'gcp-appdapjcgcpproj-nprd-424242', 'instance_id': '5656961996410000000'} {'instance_name': 'intersystems-iris-health-community-ed-1-vm'} gce_instance compute.googleapis.com/instance/cpu/guest_visible_vcpus 2.0`
            2. `name=Custom Metrics|GCP|gcp-appdapjcgcpproj-nprd-424242|compute.googleapis.com|instance|intersystems-iris-health-community-ed-1-vm|cpu|guest_visible_vcpus,value=2`
            3. `MetricPath logic - METRIC_PREFIX|${projectName}|${namespace(Slashes replaced with pipe)}|${selectedLabelsIfExist}|${metricNameAndValue}`

Ref: https://confluence.corp.appdynamics.com/pages/viewpage.action?spaceKey=~mukund.babbar@appdynamics.com&title=Google+Cloud+Monitor+%28Custom+Extension%29+-+WIP
