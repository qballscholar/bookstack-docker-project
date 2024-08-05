# BookStack with MariaDB monitored by Prometheus and metrics displayed by Grafana

This project is an educational project introducing beginners to a more intermediate approach of learning Docker and multi-containers, in an environment including the graphical monitoring of resources and database implementation.

In this project, the complete environment will be using Docker containers that includes Grafana and Prometheus for metrics and dashboards, BookStack application as the front-end app, and MariaDB as the back-end database. Docker Compose is to be used to provision the services. The project includes a comprehensive `docker-compose.yml` file that sets up all of these components and a `prometheus.yml` file to set up where Prometheus is to acquire metrics.

## Prerequisite

Ensure Docker is installed on your machine. This project will be installed on a Red Hat / CentOS Streaming 9 Linux machine.

> **Note:** If installing Docker on RHEL, it is recommended to disable Podman (if installed) to avoid any conflicts. It is recommended to install on the latest version of Centos Streaming 9 as Centos Streaming 8 & Centos 9 have deprecated repositories and may complicate the installing process.

### First, you must stop any running containers IF installed:

```bash
podman stop -a
```

Second, stop and disable the Podman socket service:

```bash
sudo systemctl stop podman.socket
sudo systemctl disable podman.socket
```

If absolutely necessary, remove Podman Package:

```bash
sudo dnf remove podman
```

And remove related directories and files:

```bash
sudo rm -rf /etc/containers /var/lib/containers
```

### Steps to install Docker if not previously installed on RHEL/CentOS machine (note: use dnf if unable to use yum):

1. Install required packages:

```bash
sudo yum update -y
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

### Steps to Start, Enable, and Verify Docker Installation

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

> **Note:** The specific steps may vary slightly depending on your RHEL version (e.g., RHEL 8 or RHEL 9). Make sure to follow the instructions provided in the official Docker documentation for your specific RHEL version.

## Now Install Docker Compose

```bash
sudo yum install docker-compose-plugin
```

## Then validate its installation

```bash
docker compose version
```

## Yum update Linux

```bash
sudo yum update -y
```

## Steps to Install and Run the Complete Environment

Open a terminal, navigate to the directory containing the `docker-compose.yml` file and `prometheus.yml` file, and run:

```bash
docker compose up -d
```

## Access the Services

Ensure BookStack is properly installed and configured on your local machine.

- **BookStack**: Open your browser and go to `http://localhost:8080`.
  - If BookStack is set up correctly, you should see a login page.
  - For the first login, use the default credentials:
    - Username: admin@admin.com
    - Password: password
  - Refer to the application source website for further documentation and personal customization.

- **Grafana**: Open your browser and go to `http://localhost:3000`. Log in with the default credentials (admin/admin).
  - Refer to the application source website for further documentation and personal customization.

- **Prometheus**: Open your browser and go to `http://localhost:9090`.
  - Refer to the source website for further documentation and personal customization.

## Configure Grafana

1. Log in to Grafana.
2. Add Prometheus as a data source by navigating to Connections > Data Sources > Add data source > Prometheus.
3. Set the URL to `http://prometheus:9090` and click Save & Test.

This setup will provide a fully functional environment with Grafana and Prometheus for monitoring, BookStack as the front-end application, and MariaDB as the back-end database.

4. Log into Grafana, then go to "Dashboards" in the main menu and click "New" to select "Import" from the dropdown menu.
5. In the field labeled 'Import via grafana.com', enter `7991` for the '2MySQL Simple Dashboard' to monitor SQL and click "Load."
6. Select the appropriate data source for the dashboard from the dropdown menu.
7. Click "Import" and the dashboard will be imported and opened then click "Save" to store it.
8. Now go back and repeat the process in steps 5, 6, & 7. You will also import `1860` for the 'Node Exporter Full' dashboard for Prometheus.

> **Note:** If the dashboard doesn't display data correctly, you may need to adjust the data source. Edit the dashboard and check each panel's data source settings. Update the data source to match your Grafana setup.

### BookStack application troubleshooting tips:

If the application does not properly display from missing graphics, a timeout on URL `http://localhost:8080`, or immediate redirection to `http://localhost:8080/login`, do the following commands on the CLI to enter the active container and adjust the environment:

1. Enter the BookStack container:

```bash
docker exec -it bookstack /bin/bash
```

2. Inside the container, edit the '.env' file ensuring the 'APP_URL' in the '.env' file matches the one in the `docker-compose.yml` template which is `APP_URL=http://localhost:8080` and save:

```bash
vi /config/www/.env
```

3. Update the URLs in the Database:

```bash
docker exec -it bookstack php /app/www/artisan bookstack:update-url http://localhost:8080/login http://localhost:8080
```

4. Clear the application cache:

```bash
docker exec -it bookstack php /app/www/artisan cache:clear
docker exec -it bookstack php /app/www/artisan view:clear
docker exec -it bookstack php /app/www/artisan route:clear
```

5. Exit the container:

```bash
exit
```

6. Restart the BookStack multi-container environment:

```bash
docker compose down
docker compose up -d
```

7. Open your browser and go to `http://localhost:8080`.

# bookstack-docker-project-mariadb
