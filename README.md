**Security Tools Environment with Docker Compose**

This project provides a Docker Compose setup to deploy a comprehensive security testing environment. It includes tools such as Jenkins for CI/CD pipelines, ZAP Proxy, OpenVAS, DVWA (Damn Vulnerable Web Application), and a custom Ubuntu container with pre-installed security tools like Nikto, Nmap, and Wapiti. Additionally, DefectDojo is included for managing security findings.

## Features

- **Jenkins**: Automates the execution of security tools in pipelines. Preconfigured with essential plugins and tools like Maven, Node.js, Docker, and integrates with DefectDojo for managing findings.
- **ZAP (Zed Attack Proxy)**: A tool for finding vulnerabilities in web applications.
- **OpenVAS**: An open-source vulnerability scanner for comprehensive system analysis.
- **DVWA**: A deliberately vulnerable web application to practice security testing.
- **Custom Ubuntu Environment**: Includes security tools like Nikto, Nmap, and Wapiti for manual and automated vulnerability assessment.
- **DefectDojo**: A security vulnerability management tool that integrates with Jenkins and tracks findings.
- **Isolated Docker Network**: Ensures secure communication between containers.

## Prerequisites

Before running the project, ensure the following are installed on your system:

- [Docker](https://www.docker.com/get-started)
- Git (to clone this repository)

## Setup Instructions

Follow these steps to get the project up and running:


**Clone the Repository**:<br>
   https://github.com/Sijin02/AutoVulnMgmt.git<br>
   cd AutoVulnMgmt
   
**Build and Start the Containers: Use Docker Compose to build and start the containers:**<br>
docker-compose up --build

**Run Pre-Pipeline Setup on Jenkins Container:**<br>
Before running any pipeline, ensure Jenkins has the correct Docker permissions by running the following commands inside the Jenkins container:

docker exec -u root -it jenkins-container /bin/sh<br>
chown root:docker /var/run/docker.sock<br>
chmod 660 /var/run/docker.sock<br>
usermod -aG docker jenkins<br>
exit<br>


**Run DefectDojo as a Separate Container:**<br>
Pull the DefectDojo image and run it separately. You will then add it to the same Docker network as Jenkins and the other tools.

docker pull defectdojo/defectdojo<br>
docker run -d --name defectdojo-container --network pfs_network -p 5000:5000 defectdojo/defectdojo<br>

**Access the Services:**
Jenkins: http://localhost:8010<br>
ZAP: http://localhost:8080<br>
DVWA: http://localhost<br>
OpenVAS: https://localhost:400<br>
DefectDojo: http://localhost:5000<br>


Run a Security Pipeline in Jenkins:<br>
Create a Jenkins pipeline that targets a URL (e.g., DVWA or any other web application).<br>
The pipeline will automatically run security tools such as ZAP, OpenVAS, and Nikto and send results to DefectDojo for vulnerability tracking.<br>

Stop the Environment: To stop and remove the containers, run:<br>
docker-compose down<br>
docker stop defectdojo-container<br>
docker rm defectdojo-container<br>
