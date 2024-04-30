#  Monitoring Infrastructure with Prometheus and Node Exporter

## Overview
This project demonstrates how to set up a monitoring infrastructure using Prometheus and Node Exporter on Ubuntu EC2 instances in the AWS cloud. The project involves installing Prometheus as a service on one EC2 instance and Node Exporter on another EC2 instance. Prometheus collects metrics from Node Exporter, providing insights into the health and performance of both the infrastructure itself and the monitored servers.

## Steps:
1. **Launch Two Ubuntu Instances**:
   - Ensure you have two Ubuntu instances provisioned in your AWS environment. These instances will serve as the hosts for Prometheus and Node Exporter.

2. **Install Prometheus and Node Exporter**:
   - Use the provided installation scripts to install Prometheus and Node Exporter on their respective Ubuntu instances.
     - [Install Prometheus Script](https://github.com/DimitryZH/installing-prometheus-server/blob/main/install_prometheus_ubuntu.sh)
     - [Install Node Exporter Script](https://github.com/DimitryZH/prometheus-monitoring/blob/main/install_prometheus_node_exporter.sh)
    
   -   Verify the installations by accessing port 9090 for Prometheus and port 9100 for Node Exporter.
    
![prometheus-server](https://github.com/DimitryZH/prometheus-monitoring/assets/146372946/491ef70e-9589-4084-a1cd-00b026e3835d)
![node-exporter-server-up](https://github.com/DimitryZH/prometheus-monitoring/assets/146372946/6335a1a0-663e-4712-9b46-53a64c4866c5)

3. **Configure Prometheus Server**:
   - Navigate to the Prometheus configuration directory on the Prometheus host.
     ```
     ubuntu@ip-172-31-30-156:~$ cd /etc/prometheus
     ```
   - Update the `prometheus.yml` configuration file according to your monitoring requirements.
```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: "prometheus"
    static_configs:
      - targets: ["localhost:9090"]

  - job_name: "ubuntu-server"
    static_configs:
      - targets:
          - 172.31.31.254:9100
```
The `prometheus.yml` configuration file specifies the global scrape interval for collecting metrics (every 15 seconds) and defines two scrape configurations:
#### **Prometheus Server:**
   - **Job name:** prometheus
   - **Target:** localhost:9090
   - **Goal:** Monitor Prometheus server itself.
####  **Ubuntu Server with Node Exporter:**
   - **Job name:** ubuntu-server
   - **Target:** 172.31.31.254:9100 (replace with the private IP address of your Ubuntu server running Node Exporter)
   - **Goal:** Monitor the Ubuntu server running Node Exporter, collecting metrics exposed on port 9100.


4. **Restart Prometheus Service**:
   - Restart the Prometheus service on the Prometheus host to apply the configuration changes.
     ```
     sudo systemctl restart prometheus
     ```
  ![install_exporter-configure-prometheus](https://github.com/DimitryZH/prometheus-monitoring/assets/146372946/e7132924-f88a-409c-b541-d40f05e2e7b0) 
  
  - Verify that Node Exporter is visible for Prometheus and the job "ubuntu-server" is successfully set up by accessing port 9090 for Prometheus.

5. **Query Metrics from Node Exporter**:
   - Utilize Prometheus's querying capabilities to make desirable queries and retrieve metrics from Node Exporter. Analyze the collected metrics to gain insights into your system's performance and health.



## Results: 

1. **Prometheus Targets:**
   
   ![Prometheus Targets](https://github.com/DimitryZH/prometheus-monitoring/assets/146372946/635b36e7-ea92-4633-954a-f266025eed06)

   - This screenshot shows the Prometheus Targets page, where the monitored endpoints are listed. The targets include "prometheus" (the Prometheus server itself) and "ubuntu-server" (the Ubuntu server running Node Exporter). These targets are up and being successfully scraped by Prometheus.

3. **Graphs. Examples of Metrics:**
   Here are screenshots demonstrating the metrics collected and visualized by Prometheus:
   
   - **node_cpu_seconds_total for last 30 min:**
     
     ![CPU Usage](https://github.com/DimitryZH/prometheus-monitoring/assets/146372946/0172f8a5-1d78-483d-abd0-52f436bc2ec4)

     - This graph displays the `node_cpu_seconds_total` metric over the last 30 minutes. It represents the cumulative CPU time consumed per CPU core in seconds. Monitoring CPU usage over time helps identify trends and anomalies, allowing for better resource management and optimization.

   - **node_network_receive_packets_total for last 15 min:**
     
     ![Network Traffic](https://github.com/DimitryZH/prometheus-monitoring/assets/146372946/a8b8a680-be9c-4f7f-8565-162424828bcc)

     - This graph illustrates the `node_network_receive_packets_total` metric over the last 15 minutes. It indicates the total number of network packets received by the server. Monitoring network traffic helps in understanding network usage patterns, detecting bottlenecks, and optimizing network performance.

## Conclusion:
In this project, we have successfully set up a monitoring infrastructure using Prometheus and Node Exporter on Ubuntu EC2 instances in the AWS cloud. We installed Prometheus as a service on one EC2 instance and Node Exporter on another EC2 instance. Prometheus collects metrics from Node Exporter, providing insights into the health and performance of both the infrastructure itself and the monitored servers. By following these steps, users can effectively monitor their infrastructure and applications using Prometheus and Node Exporter.
