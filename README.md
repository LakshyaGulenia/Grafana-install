# Grafana-install

For installation click on this link for ubuntu-----
https://grafana.com/docs/grafana/latest/setup-grafana/installation/debian/

after running all cmds type this to start grafana.service-----
sudo systemctl start grafana-server.service 

to check the status-----
sudo systemctl status grafana-server.service 

# Grafana-self monitor on ec2
pre-req install Node-Exporter on ec2 collect scrapes metrics.-----
Navigate to home
cd ~

Download the latest (correct) version — v1.9.1 at the time of writing-----
wget https://github.com/prometheus/node_exporter/releases/download/v1.9.1/node_exporter-1.9.1.linux-amd64.tar.gz

Extract it-----
tar -xvf node_exporter-1.9.1.linux-amd64.tar.gz
cd node_exporter-1.9.1.linux-amd64

Run Node Exporter-----
./node_exporter &
after this its gona show u----
level=info ts=... caller=node_exporter.go:... msg="Listening on" address=:9100
confirm its running----
curl http://localhost:9100/metrics

pre-req install Prometheus to convert Scrapes metrics from Node Exporter.
cd ~
wget https://github.com/prometheus/prometheus/releases/download/v3.4.1/prometheus-3.4.1.linux-amd64.tar.gz
tar -xvf prometheus-3.4.1.linux-amd64.tar.gz
cd prometheus-3.4.1.linux-amd64
after that u need to edit prometheus.yml---
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'ec2-node'
    static_configs:
      - targets: ['localhost:9100']
---like this
then rn prometheus-----
./prometheus --config.file=prometheus.yml &
check if its working-----
http://<your-ec2-ip>:9090
now----
Add Prometheus as Data Source in Grafana
Go to Grafana > connections > Data Sources > Add Data Source

Choose Prometheus

Set URL to: http://localhost:9090

Click Save & Test
Import Dashboard-----
In Grafana, click + > Import

Use Dashboard ID: 1860 
➤ Node Exporter Full Dashboard

Select your Prometheus data source
