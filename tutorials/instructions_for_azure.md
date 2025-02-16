# 🚀 Installing on Azure

## 🎯 Goal

By the end of this tutorial, you should have a JupyterHub with admin users and a user environment with installed packages running on [Microsoft Azure](https://azure.microsoft.com).

This tutorial guides you step-by-step to manually deploy your own JupyterHub on Azure cloud.

## ✅ Prerequisites

- A Microsoft Azure account.
- You can start with a free account, which includes $150 worth of Azure credits. [Sign up here](https://azure.microsoft.com/en-us/free//?wt.mc_id=TLJH-github-taallard).

## 🏗️ Step 1: Installing The Littlest JupyterHub

We start by creating a Virtual Machine (VM) to run The Littlest JupyterHub (TLJH).

1. 🔗 Go to [Azure portal](https://portal.azure.com/) and log in with your Azure account.

2. 📂 In the left panel, navigate to **Virtual Machines** and click **+ Add** to create a new Virtual Machine.

3. 🖥️ Select **Create VM from Marketplace** and choose **Ubuntu Server 22.04 LTS**.

4. 🛠️ Customize your Virtual Machine:
   - **Subscription**: Choose your Azure plan.
   - **Resource Group**: Create a new one or use an existing group.
   - **Name**: Provide a descriptive name for your VM.
   - **Region**: Choose a location close to your users.
   - **Availability options**: Select "No infrastructure redundancy required."
   - **Authentication type**: Set to **Password**.
   - **Username**: Choose a root user name.
   - **Password**: Set a strong password.

5. 💾 Set up your VM’s resources:
   - **RAM**: Ensure enough memory for your users.
   - **Storage**: Choose between **SSD** (faster) or **HDD** (cheaper).
   - **Attach a data disk** if needed.

6. 🌐 Networking configuration:
   - **Public IP**: Keep default settings.
   - **Security Group**: Choose "Basic" and allow **HTTP, HTTPS, and SSH** traffic.

7. 🏗️ Enable system management options:
   - **Boot diagnostics**: Enable it.
   - **OS guest diagnostics**: Disable it.
   - **Auto-shutdown**: Disable it.
   - **Backup**: Disable it.

8. 🔧 Advanced settings:
   - **Extensions**: Ensure no extensions are listed.
   - **Cloud init**: Use the following script to install TLJH automatically:

   ```bash
   #!/bin/bash
   curl -L https://tljh.jupyter.org/bootstrap.py \
     | sudo python3 - \
       --admin <admin-user-name>
   ```
   
   Replace `<admin-user-name>` with your chosen admin username.

9. ✅ Review your VM configuration and confirm the creation.

10. ⏳ Wait for the VM setup to complete. This takes about 5-10 minutes.

11. 🌍 Copy your **Public IP address** from the VM details and access it in a browser.

12. 🔑 Log in with your admin username and password to access JupyterHub.

13. 🎉 Congratulations! Your JupyterHub is now running!

## 👥 Step 2: Adding more users

To add new users:

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