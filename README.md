# Robust WordPress and MySQL Architecture with Docker

In this guide, we'll walk through creating a WordPress application connected to a MySQL database using Docker containers. Follow the steps below to set up and launch your WordPress and MySQL architecture seamlessly.

---

## Architecture Overview

The architecture consists of:
1. **MySQL Container**: Serves as the database backend to store and manage WordPress data.
2. **WordPress Container**: Hosts the WordPress application and connects to the MySQL database.
3. **Persistent Volume**: Ensures that database data is not lost when the MySQL container is removed.

**Diagram:**

---

![image](https://github.com/user-attachments/assets/3420fa09-9abe-4af6-8963-84ea4cca8c4b)

---

## Use Cases

1. **Personal Blogging**: Quickly set up a personal blog or website with WordPress.
2. **Content Management**: Manage content for small businesses or projects.
3. **Development and Testing**: Ideal for testing WordPress plugins or themes in a local or cloud environment.

---

## Step 1: Create a Directory for Persistent Data

To ensure MySQL data persists even if the container is removed, create a directory on your host machine to store the database files.

```bash
mkdir /mydatabases
```
![WhatsApp Image 2025-01-02 at 11 05 47_bcadb6e3](https://github.com/user-attachments/assets/efee74e4-acf4-498a-8f74-79edd048f37b)
![WhatsApp Image 2025-01-02 at 11 08 53_748a1ee7](https://github.com/user-attachments/assets/441e810d-dff1-4afa-b41f-d001612413ba)


**Explanation:**
- The `mkdir /mydatabases` command creates a directory named `/mydatabases` on your local system. This directory will be mounted as a volume in the MySQL container to store database data persistently.

---

## Step 2: Launch the MySQL Container

Run the following command to start a MySQL container:

```bash
docker run -dit --name mysqldb \
  -e MYSQL_ROOT_PASSWORD=redhat \
  -e MYSQL_USER=ritesh \
  -e MYSQL_PASSWORD=redhat \
  -e MYSQL_DATABASE=mydb \
  -v /mydatabases:/var/lib/mysql \
  mysql:latest
```
![WhatsApp Image 2025-01-02 at 11 26 59_ec893232](https://github.com/user-attachments/assets/6d71b7be-13e9-448d-8f8a-2108fb79eb7c)

**Explanation:**
- `--name mysqldb`: Assign a container name, `mysqldb`.
- `-e MYSQL_ROOT_PASSWORD=redhat`: Set the root password for MySQL.
- `-e MYSQL_USER=ritesh`: Create a user named `ritesh`.
- `-e MYSQL_PASSWORD=redhat`: Set the password for the user `ritesh`.
- `-e MYSQL_DATABASE=mydb`: Create a database named `mydb`.
- `-v /mydatabases:/var/lib/mysql`: Map the `/mydatabases` directory on the host to `/var/lib/mysql` inside the container for data persistence.
- `mysql:latest`: Specifies the latest MySQL image from Docker Hub.

**Key Point**: Use environment variables to configure MySQL securely and efficiently.

---

## Step 3: Launch the WordPress Container

Run the following command to start a WordPress container linked to the MySQL container:

```bash
docker run -dit --name wordpress_server \
  -e WORDPRESS_DB_HOST=mysqldb \
  -e WORDPRESS_DB_USER=ritesh \
  -e WORDPRESS_DB_PASSWORD=redhat \
  -e WORDPRESS_DB_NAME=mydb \
  --link mysqldb \
  -p 1234:80 \
  wordpress:latest
```
![WhatsApp Image 2025-01-02 at 13 26 07_970edeaf](https://github.com/user-attachments/assets/8e664407-1c36-4d22-8fa2-a003f0c56d53)

**Explanation:**
- `--name wordpress_server`: Assign a container name, `wordpress_server`.
- `-e WORDPRESS_DB_HOST=mysqldb`: Specifies the MySQL container as the database host.
- `-e WORDPRESS_DB_USER=ritesh`: Set the database user to `ritesh`.
- `-e WORDPRESS_DB_PASSWORD=redhat`: Set the password for the database user.
- `-e WORDPRESS_DB_NAME=mydb`: Connect WordPress to the `mydb` database.
- `--link mysqldb`: Link the WordPress container to the MySQL container for communication.
- `-p 1234:80`: Map port 1234 on the host to port 80 in the container for accessing WordPress.
- `wordpress:latest`: Specifies the latest WordPress image from Docker Hub.

**Key Point**: Port `1234` on the host allows external access to your WordPress site.

---

## Step 4: Access the WordPress Application

After starting both containers, you can access your WordPress site by opening a web browser and navigating to:

```
http://<your-ec2-instance-ip>:1234
```

You will see the WordPress setup page, where you can configure your site and connect it to the MySQL database.
![WhatsApp Image 2025-01-02 at 11 31 15_5ef7369d](https://github.com/user-attachments/assets/7c0562e2-52ba-4b2f-9f6a-f655445ab69c) ![WhatsApp Image 2025-01-02 at 11 32 10_30fd6285](https://github.com/user-attachments/assets/8a4acbc2-9e83-41c8-ad27-cee19edd3cdf) ![WhatsApp Image 2025-01-02 at 11 34 12_32d0c1dc](https://github.com/user-attachments/assets/73c1fb06-525e-4ef9-9e2b-f6fd9ed41256) ![WhatsApp Image 2025-01-02 at 11 35 20_a4090977](https://github.com/user-attachments/assets/3eee0bcf-7bcd-40e0-8aa0-880dd7abe1de)

**My Post:**
![WhatsApp Image 2025-01-02 at 11 40 17_9cfbc843](https://github.com/user-attachments/assets/9daedf29-a70c-47bd-8bd0-4fdd0cc547e4)

**Explanation:**
- Replace `<your-ec2-instance-ip>` with the public IP address of your server or localhost if running locally.
- Use the credentials and database information provided during the container setup to complete the WordPress installation.

---
By following these steps, you have successfully set up a robust WordPress and MySQL architecture using Docker. 

### Benefits of This Setup
- **Scalability**: Easily add more containers or services.
- **Portability**: Run the same setup on any Docker-supported platform.
- **Data Security**: Persistent volumes ensure data safety.

**Happy blogging!**
