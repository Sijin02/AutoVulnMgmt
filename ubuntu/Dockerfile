# Use an official Ubuntu as a base image
FROM ubuntu

# Set environment variables to non-interactive (for automatic installation)
ENV DEBIAN_FRONTEND=noninteractive

# Update package list and install necessary dependencies
RUN apt update && \
    apt install -y \
    git \
    perl \
    libnet-ssleay-perl \
    libwhisker2-perl \
    python3 \
    python3-pip \
    nmap \
    wapiti  \
    && rm -rf /var/lib/apt/lists/*

# Clone Nikto from GitHub repository
RUN git clone https://github.com/sullo/nikto.git /nikto

# Set the working directory to /nikto
WORKDIR /nikto

# Create a symbolic link for easy access
RUN ln -s /nikto/program/nikto.pl /usr/local/bin/nikto

# Switch to root user if needed
USER root