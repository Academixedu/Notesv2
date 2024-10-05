### Step-by-Step Instructions to Install Docker on EC2 and Connect to a Container

---

### Step 1: Install Docker on the EC2 Instance

1. **Update the package database**:
   Run the following command to make sure your system packages are up to date:
   ```bash
   sudo su     (to get into sudo mode)
   sudo yum update -y
   ```

2. **Install Docker**:
   Use the following command to install Docker:
   ```bash
   sudo yum install docker
   ```

3. **Start the Docker service**:
   After installing Docker, start the Docker service:
   ```bash
   sudo service docker start
   ```

4. **Add the `ec2-user` to the Docker group**:
   This step allows you to run Docker commands without needing `sudo` every time:
   ```bash
   sudo usermod -a -G docker ec2-user
   ```

5. **Restart your session**:
   Log out of the instance and log back in, or run the following command to apply the group changes:
   ```bash
   newgrp docker
   ```

6. **Verify Docker Installation**:
   To confirm that Docker is installed and running, check the Docker version:
   ```bash
   docker --version
   ```
   You should see the Docker version information.

---

### Step 2: Run a Test Docker Container

Now that Docker is installed, let’s run a test container to ensure everything is working.

1. **Pull a Docker image**:
   We’ll use the official **hello-world** image as a simple test:
   ```bash
   docker pull hello-world
   ```

2. **Run the Docker container**:
   Execute the following command to run the container:
   ```bash
   docker run hello-world
   ```

   This should output a message confirming that Docker is working correctly.

---

### Step 3: Install and Run a Web Server in a Docker Container

Let’s run a more useful container, such as **nginx**, which is a web server.

1. **Pull the NGINX Docker image**:
   ```bash
   docker pull nginx
   ```

2. **Run the NGINX container**:
   ```bash
   docker run -d -p 80:80 --name nginx-server nginx
   ```
   - `-d`: Runs the container in detached mode (in the background).
   - `-p 80:80`: Maps port 80 on the EC2 instance to port 80 on the container, allowing you to access the web server.
   - `--name nginx-server`: Gives the container a name (`nginx-server`).

3. **Verify that the container is running**:
   Run the following command to see your running containers:
   ```bash
   docker ps
   ```

---

### Step 4: Connect to the Docker Container (NGINX Web Server)

1. **Access the web server**:
   Open your web browser and navigate to the public IP address of your EC2 instance. You can find this IP in the EC2 dashboard under the "Public IP" column.
   - **URL**: `http://your-ec2-public-ip`
   
   If everything is set up correctly, you should see the NGINX welcome page.

---

### Step 5: Access and Interact with the Container

If you want to connect directly to the running Docker container and interact with it:

1. **Access the NGINX container's shell**:
   Run the following command to open a shell inside the running container:
   ```bash
   docker exec -it nginx-server /bin/bash
   ```

2. **Explore the container**:
   Once inside, you can run regular Linux commands to explore the file system:
   ```bash
   ls
   ```
   When you're done, type `exit` to leave the container shell.

---

### Step 6: Manage Your Docker Containers

Here are some common Docker management commands:

1. **Stop the container**:
   ```bash
   docker stop nginx-server
   ```

2. **Start the container**:
   ```bash
   docker start nginx-server
   ```

3. **Remove the container**:
   ```bash
   docker rm nginx-server
   ```

4. **List all containers (including stopped ones)**:
   ```bash
   docker ps -a
   ```

---

### Conclusion
You have successfully installed Docker on your EC2 instance, run an NGINX web server inside a Docker container, and accessed the web server. You can now experiment with running more complex applications inside containers or even build your own custom containers.

Let me know if you'd like to try anything more with Docker or if you need further help!
