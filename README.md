# Implementing Hierarchical Federation With Prometheus
Implementing Hierarchical Federation With Prometheus

# Implementing Hierarchical Federation with Prometheus Introduction

Prometheus servers are capable of monitoring a large number of applications and components. However, it does not always make sense to monitor everything with a single Prometheus server. Prometheus provides the ability to federate Prometheus servers, allowing high-level Prometheus servers to collect, and even aggregate, metric data from multiple low-level Prometheus servers. In this lab, you will be able to see how this works. You will configure a high-level Prometheus instance to collect data from multiple lower-level Prometheus servers in a single location.

# Solution
Log in to the Federal Prometheus Server via SSH using the provided credentials.


# Configure the Federal Prometheus Server to Scrape Metrics from the Other Two Prometheus Servers

1 . Edit the Prometheus configuration:
```
sudo vi /etc/prometheus/prometheus.yml
```
2. Implement a scrape configuration to pull metrics from the other two Prometheus servers. Under this section:
```
scrape_configs:
    -targets:['localhost:9090']
```
Enter the following:
```
...

 - job_name: 'federate'
   scrape_interval: 15s
   honor_labels: true
   metrics_path: '/federate'
   params:
     'match[]':
       - '{job!~"prometheus"}'
   static_configs:
   - targets:
     - 'prometheus-ds-1:9090'
     - 'prometheus-ds-2:9090'
```
3. Save and quit by pressing Escape followed by :wq




# Start the Federal Prometheus Server and Verify Everything Is Working

1. Enable Prometheus on the Federal Prometheus Server:
```
sudo systemctl enable prometheus
```
2. Start Prometheus:
```
sudo systemctl start prometheus
```
3. Access the Federal Prometheus Server in a browser at
```
http://<Federated Prometheus Server Public IP>:9090. (The public IP can be found in the lab credentials.)
```
4. Run a query to view some metric data:
```
up
```
We should see metric data for instances that are monitored by the other two Prometheus servers. These instances are called limedrop-web-1:9100 and limedrop-web-2:9100.























     
