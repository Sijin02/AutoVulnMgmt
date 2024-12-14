# Start from the official Jenkins LTS image
FROM jenkins/jenkins:lts
# Switch to root user to install software packages
USER root
# Install Maven for managing Java projects
RUN apt-get update && \
    apt-get install -y maven && \
    apt-get clean
# Install Node.js and npm
RUN apt-get install -y curl && \
    curl -fsSL https://deb.nodesource.com/setup_18.x | bash - && \
    apt-get install -y nodejs && \
    apt-get clean
# Install Docker so that Jenkins can interact with Docker containers
RUN apt-get install -y docker.io 
# Verify that Node.js, npm, and Docker are installed correctly
RUN node -v && npm -v && docker --version

# Install essential Jenkins plugins for building and working with Docker
RUN jenkins-plugin-cli --plugins \
    workflow-aggregator:2.6 \
    docker-workflow:1.26 \
    defectdojo:1.0.0 

RUN apt-get install -y jq

# Switch back to the Jenkins user for normal operation
USER jenkins
