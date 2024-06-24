Your README.md file looks good overall, but I'll make some minor adjustments to improve the formatting for GitHub. Here's the corrected version:

```markdown
# BookStack with MySQL monitored by Prometheus and Grafana

This project is an educational lab introducing beginners to a more intermediate approach of learning Docker, and containers in general, with an environment including monitoring of resources and database implementation.

In this project, the complete environment will be using Docker containers that includes Grafana and Prometheus for metrics and dashboards, BookStack application as the front-end, and MySQL as the application back-end. Docker Compose is to be used to manage the services. The project includes a comprehensive docker-compose.yml file that sets up all these components and a prometheus.yml file.

## Prerequisite

Ensure Docker is installed on your machine. This project will be installed on a Red Hat / CentOS Linux machine.

### Steps to install Docker if not installed on RHEL/CentOS machine:

1. Install required packages:
   ```bash
   sudo yum install -y yum-utils device-mapper-persistent-data lvm2
   ```

2. Add the official Docker repository:
   ```bash
   sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
   ```

3. Install the latest Docker Engine:
   ```bash
   sudo yum install docker-ce docker-ce-cli containerd.io
   ```

### Steps to Start, Enable, and Verify Docker Installation:

```bash
sudo systemctl start docker
sudo systemctl enable docker
sudo docker run hello-world
```

Optional: To add user to Docker Group and run commands without 'sudo', add your user to the 'docker' group:

```bash
sudo usermod -aG docker $USER
```
Then log out and log back in for the changes to take effect.

Note: The specific steps may vary slightly depending on your RHEL version (e.g., RHEL 8 or RHEL 9). Make sure to follow the instructions provided in the official Docker documentation for your specific RHEL version.

## Steps to Install and Run the Complete Environment

Open a terminal, navigate to the directory containing the docker-compose.yml file and prometheus.yml file, and run:

```bash
docker-compose up -d
```

## Access the Services

Ensure BookStack is properly installed and configured on your local machine.

- **BookStack**: Open your browser and go to http://localhost:8080.
  - If BookStack is set up correctly, you should see a login page.
  - For the first login, use the default credentials:  
    Username: admin@admin.com
    Password: password

  - refer to application source website for further documentation and personal customization: https://www.bookstackapp.com/
  

- **Grafana**: Open your browser and go to http://localhost:3000. Log in with the default credentials (admin/admin).

  - refer to application source website for further documentation and personal customization: https://grafana.com/


- **Prometheus**: Open your browser and go to http://localhost:9090.

  - refer to source website for further documentation and personal customization: https://prometheus.io/
  
## Configure Grafana

1. Log in to Grafana.
2. Add Prometheus as a data source by navigating to Connections > Data Sources > Add data source > Prometheus.
3. Set the URL to http://prometheus:9090 and click Save & Test.

This setup will provide a fully functional environment with Grafana and Prometheus for monitoring, BookStack as the front-end application, and MySQL as the back-end database.

4. Go to Dashboard and click the 'New' blue button on the right side of the screen and you will see a dropdown menu. 
5. Select import and in the field entry labeled 'Find and import dashboard URL or ID" enter 7991 for the '2MySQL Simple Dashboard' to monitor SQL. 
6. Now go back and repeat the process then import 1860 for 'Node Exporter Full' dashboard for Prometheus.# bookstack-docker-project
