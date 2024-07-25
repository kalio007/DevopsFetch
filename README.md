

# DevOps Fetch Tool

The DevOps Fetch Tool (`devopsfetch.sh`) is a comprehensive system information gathering script that provides insights into various aspects of a server's operation including active ports, Docker containers, Nginx configurations, user logins, and system activity logs. It is designed to run both as a standalone script and as a systemd service for continuous monitoring.

## Features

- **Ports**: List all active ports or information about a specific port.
- **Docker**: Display all Docker containers and images or details about a specific container.
- **Nginx**: Show all Nginx domain configurations or details for a specific domain.
- **Users**: List all users and their last login times or details for a specific user.
- **Time Range**: Display system logs for a specified time range.
- **Continuous Monitoring**: Continuous monitoring mode that logs updated system information every 5 minutes.

## Requirements

- Linux-based system (Tested on Ubuntu 20.04)
- Docker
- Nginx
- net-tools
- systemd (for setting up as a service)

## Installation

1. **Clone the repository**:
   ```
   git clone https://github.com/Mobey-eth/Hng_stage_5-midway-task.git
   cd devopsfetch
   ```

2. **Make the script executable**:
   ```
   sudo chmod +x devopsfetch.sh
   ```

3. **Move the script to a suitable directory** (optional):
   ```
   sudo mv devopsfetch.sh /usr/local/bin/
   ```

4. **Ensure log file is setup**:
   ```
   sudo touch /var/log/devopsfetch.log
   sudo chmod 644 /var/log/devopsfetch.log
   ```

5. **Install Dependencies**:
   ```
   sudo apt-get update
   sudo apt-get install net-tools docker.io nginx
   ```

## Systemd Service Setup

To enable continuous monitoring, you can set up `devopsfetch.sh` as a systemd service.

1. **Create systemd service file**:
   ```
   sudo nano /etc/systemd/system/devopsfetch.service
   ```

   Insert the following configuration:

   ```ini
   [Unit]
   Description=DevOps Fetch Service
   After=network.target

   [Service]
   Type=simple
   ExecStart=/usr/local/bin/devopsfetch.sh -c
   Restart=on-failure
   User=root
   WorkingDirectory=/usr/local/bin
   StandardOutput=append:/var/log/devopsfetch.log
   StandardError=append:/var/log/devopsfetch.log

   [Install]
   WantedBy=multi-user.target
   ```

2. **Reload systemd to recognize the new service**:
   ```
   sudo systemctl daemon-reload
   ```

3. **Enable and start the service**:
   ```
   sudo systemctl enable devopsfetch.service
   sudo systemctl start devopsfetch.service
   ```

## Testing



## Manual Testing

### **Testing Individual Functions**

#### Ports Functionality:

- **Test listing all active ports:** This will display all services currently listening on ports.
  ```bash
  sudo /usr/local/bin/devopsfetch.sh -p
  ```
- **Test a specific port:** Use this to check detailed information for a known active port, such as port 80 if a web server is running.
  ```bash
  sudo /usr/local/bin/devopsfetch.sh -p 80
  ```

#### Docker Functionality:

- **Preparation:** Ensure Docker is running and has some containers for testing. If none, start a simple one:
  ```bash
  sudo docker run -d --name test-nginx nginx
  ```
- **Test listing all Docker images and containers:**
  ```bash
  sudo /usr/local/bin/devopsfetch.sh -d
  ```
- **Test detailed information for a specific container:** This will show detailed Docker container configuration and state.
  ```bash
  sudo /usr/local/bin/devopsfetch.sh -d test-nginx
  ```

#### Nginx Configuration:

- **Test displaying all Nginx configurations:** This requires that Nginx is installed and configured with at least one site.
  ```bash
  sudo /usr/local/bin/devopsfetch.sh -n
  ```
- **Test for a specific Nginx domain configuration:** Assuming a domain like `example.com` is configured, this command will display the Nginx configuration for that domain.
  ```bash
  sudo /usr/local/bin/devopsfetch.sh -n example.com
  ```

#### User Login Information:

- **Test listing all users and their last login times:**
  ```bash
  sudo /usr/local/bin/devopsfetch.sh -u
  ```
- **Test specific user login details:** Replace `username` with a real user's name to get their last login details.
  ```bash
  sudo /usr/local/bin/devopsfetch.sh -u username
  ```

#### Time Range Logs:

- **Test displaying logs for a specific time range:** This could be for logs from "today" or "1 hour ago".
  ```bash
  sudo /usr/local/bin/devopsfetch.sh -t "1 hour ago"
  ```

### **Continuous Monitoring Mode**

- **Start the script in continuous monitoring mode:** This mode refreshes and logs data every 5 minutes.
  ```bash
  sudo /usr/local/bin/devopsfetch.sh -c
  ```
- **Monitor the output:** Let it run for a period (like 15 or 30 minutes), then check the log file to verify that it is updating.
  ```bash
  sudo tail -f /var/log/devopsfetch.log
  ```
- **Stop monitoring:** Use Ctrl+C to stop monitoring the log output.

### **Review Log Files**

- **Check the log file:** To ensure all outputs are logged correctly and to understand the information being recorded.
  ```bash
  sudo less /var/log/devopsfetch.log
  ```



### Systemd Service Testing

- **Check service status**:
  ```
  sudo systemctl status devopsfetch.service
  ```

- **View service logs**:
  ```
  sudo tail -f /var/log/devopsfetch.log
  ```

- **Restart the service** if needed:
  ```
  sudo systemctl restart devopsfetch.service
  ```

## Conclusion

The DevOps Fetch Tool is designed to provide system administrators and DevOps engineers with a powerful tool for monitoring and logging system status. By integrating it as a systemd service, it provides continuous, automated monitoring with minimal manual intervention.
