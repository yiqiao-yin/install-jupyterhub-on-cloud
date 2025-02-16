# 🚀 Installing on Amazon Web Services

## 🎯 Goal

To have a JupyterHub with admin users and a user environment with conda / pip packages.

## ✅ Prerequisites

1. An Amazon Web Services account.
   
   If asked to choose a default region, choose the one closest to the majority of your users.

## 🏗️ Step 1: Installing The Littlest JupyterHub

Let's create the server on which we can run JupyterHub.

1. 🔗 Go to Amazon Web Services and sign in to the console.

2. 🖥️ In the AWS Management Console, select **EC2** under **Compute**.

3. 📄 From the **EC2 Management Console**, choose **Instances** under the **INSTANCES** sub-heading.

4. 🚀 Click **Launch Instance**.

5. 🛠️ On **Step 1: Choose an Amazon Machine Image (AMI)**, select **Ubuntu Server 22.04 LTS**.

6. 💾 **Step 2: Choose an Instance Type** – Select a server with at least **2GB of RAM**, such as **t3.small**.

7. 🔍 **Step 3: Configure Instance Details** – Add the following script under **User data**:
   
   ```bash
   #!/bin/bash
   curl -L https://tljh.jupyter.org/bootstrap.py \
     | sudo python3 - \
       --admin <admin-user-name>
   ```

   ### Additional options after `--admin`
   - To set a password for the admin user during setup:
     
     ```bash
     --admin <admin-user-name> --user-password <password>
     ```
   
   - To add multiple admin users:
     
     ```bash
     --admin user1 user2 user3
     ```
   
   - To pre-configure additional users:
     
     ```bash
     --user user4 user5 user6
     ```
   
   - To specify a different installation directory:
     
     ```bash
     --prefix /opt/tljh
     ```
   
   - To disable user environment installation (useful for custom setups):
     
     ```bash
     --plugin disable-user-env
     ```

8. 💽 **Step 4: Add Storage** – Adjust disk size as needed.

9. 🏷️ **Step 5: Add Tags** – Give your server a meaningful name.

10. 🔐 **Step 6: Configure Security Group** – Allow **HTTP, HTTPS, and SSH** traffic.

11. ✅ **Review and Launch** – Click the **Launch** button.

12. 🔑 Choose or create an **SSH key pair**.

13. 🎬 Click **Launch instances**.

14. 🔄 Wait for the server to start.

15. 🌍 Find the **Public IP** of your server.

16. ⏳ Wait **~10 minutes** for JupyterHub to install.

17. 🔎 Check installation by accessing the **Public IP** in your browser.

18. 🎉 Once installed, you should see the **JupyterHub login page**.

19. 🔑 Log in with the **admin username** and set a strong password.

20. 🏆 Congratulations! Your JupyterHub is now running!

## 👥 Step 2: Adding more users

To add users after installation, use the following command:

```bash
sudo tljh-config add-user newuser
```

To add an admin user:

```bash
sudo tljh-config add-admin newadmin
```

To remove a user:

```bash
sudo tljh-config remove-user username
```

## 📦 Step 3: Install conda / pip packages for all users

To install a package for all users:

```bash
sudo -E pip install package-name
```

For conda packages:

```bash
sudo -E conda install -c conda-forge package-name
```

To ensure changes take effect, restart JupyterHub:

```bash
sudo systemctl restart jupyterhub
```
