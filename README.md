# cloud-tech-assignment

# 🚀 AAP + Prometheus + Grafana Setup on RHEL (Cloud VM)

This project demonstrates the setup of **Red Hat Ansible Automation Platform (AAP)**, **Prometheus**, and **Grafana** on a cloud-based RHEL VM. It includes configuration files and a walkthrough presentation covering the integration of metrics monitoring and visualization.

---

## 📁 Repository Structure

```bash
.
├── prometheus.yml           # Prometheus scrape configuration
├── inventory-growth         # Ansible inventory or playbook file (modify as needed)
├── Technical Assignment.pptx  # Final demo presentation (implementation summary)
└── README.md                # This file
```
## Step-by-Step Execution Guide
1. **Cloud Setup**
  - Sign up with a cloud provider (AWS or GCP) and provision a RHEL VM.
  - Ensure:
    - Outbound internet access is allowed.
    - Inbound rules open relevant ports (e.g., 22, 443, 9090 for Prometheus, 3000 for Grafana).

2. **Red Hat Developer Subscription**
   - Create a Red Hat Developer Account.
   - Subscribe to the Red Hat Enterprise Linux Individual Developer Subscription for free access.
   - Download -automation-platform-containerized-setup-2.5-14.tar.gz in a remote computer

3. **Basic Configuration Before Installation**
   - SSH into your RHEL VM.
   - Create a new user who will be the owner for AAP
   - Create an installation folder in the VM as that user
   - scp the tar file to this folder and extract its contents.

4. **AAP Installation**
   - Modify the extracted inventory-growth file as per the sample provided in this repo(adjust as required)
   - Run the install script
     ``` bash
      ansible-playbook -i inventory-growth ansible.containerized_installer.install
     ```
  - Log into AAP in the browser by providing the VM's IP address or hostname and activate the subscription
    
5. **Grafana Installation**
   - Run as a container
     ``` bash
      podman network create monitoring
      podman run -d --network monitoring --name=grafana -p 3000:3000 grafana/grafana
     ```
   - Log into Grafana in the browser by providing the VM's IP address or hostname and port 3000

6. **Prometheus Installation**
   - Create the prometheus job config as per the sample provided in this repo
   - Run as a container
     ``` bash
     podman run -d -network monitoring --name=prometheus -p 9090:9090 -v /home/aap/prometheus/prometheus.yml:/etc/prometheus/prometheus.yml:ro,z prom/prometheus
     ```
   - Log into Grafana in the browser and connect the Datasource to the server http://prometheus:9090
     
7. **Node_Exporter Installation**
   - Run as a container
     ``` bash
     podman run -d --network monitoring --name=node_exporter -p 9100:9100 prom/node-exporter
     ```
   - Ensure that the entries in prometheus job config file referes to this container for scraping
     
8. **Create Grafana Dashboard**
   - Log into Grafana and create new dashboard using template ID 1860 sourced from grafana.com
     
