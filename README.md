# Jenkins Declarative Pipeline Setup

This guide provides step-by-step instructions for setting up a Jenkins declarative pipeline to automate the build and deploy using Docker and Jenkins.

---

## Prerequisites

Ensure you have the following installed and configured on Instance:
- **Docker**
- **Docker Compose**
- **Docker Hub (If used in project)**
- **Jenkins**

Refer to the installation guide: [Java_Jenkins_Docker_Setup_Cloud](https://github.com/dhruv-dosh/Java_Jenkins_Docker_Setup_Cloud)

---

## 1. Create a Jenkins Project

1. Open Jenkins at `http://your_public_ip:8080`
2. Click **New Item**
3. Select **Pipeline**
4. Enter project name 
5. Click **OK**

---

## 2. Configure the Project

### General
- **Description**: Django notes app with declarative pipeline
- **GitHub Project**: Add repository URL

### Build Triggers
- Enable **GitHub hook trigger for GITScm polling**

### Pipeline
- **Definition**: Pipeline script from SCM
- **SCM**: Git
- **Repository URL**: Enter your GitHub repo link
- **Branches to build**: `*/main`
- **Script Path**: `Jenkinsfile`

---

## 3. Add Docker Hub Credentials in Jenkins (If used in your project)

1. Navigate to **Manage Jenkins** → **Manage Credentials**
2. Under **Stores scoped to Jenkins**, select **Global credentials (unrestricted)**
3. Click **Add Credentials**
4. Enter the following details:
   - **Kind**: Username with password
   - **Scope**: Global (Jenkins, nodes, items, all child items, etc.)
   - **ID**: `dockerHub`
   - **Description**: This is DockerHub credentials.
   - **Username**: DockerHub Username 
   - **Password**: DockerHub Password
5. Click **OK**

---

## 4. Allow Port 8000 in Security Group, Firewall, etc

For accessing the Django Notes App, open port **8000**:
- **Type**: Custom TCP
- **Port Range**: 8000
- **Source**: Anywhere-IPv4

---

## 5. Automate Build with Webhooks

1. Install **GitHub Integration Plugin** in Jenkins from **Manage Jenkins → Plugins → Available Plugins**.
2. Select **restart Jenkins when installation is complete and no jobs are running**.
3. Go to **GitHub Repo → Settings → Webhooks → Add Webhook**.
4. Set Payload URL: `http://your_jenkins_master_public_ip:8080/github-webhook/`.
5. Content Type: `application/json`.
6. Select **Just the push event**.
7. Leave secret field empty and other fields as it is.
8. Save and verify webhook with a green tick.
9. In **Jenkins → Project → Configure**:
   - Select **GitHub hook trigger for GITScm polling** under **Triggers**.

Now, any code push to GitHub will trigger an automatic build and deployment via Jenkins. If doesn't work then build manually from jenkins for one time.

#### NOTE: your_public_ip changes when you stop the instance. So update Payload URL accordingly


## 6. Running the Pipeline & Verifying the Deployment

- Push some changes in the code on Github and pipeline will automatically build the app.
- Once the build is complete, the Django Notes App should be accessible at: 
  
  ```
  http://your_public_ip:8000
  ```

---

### You have successfully set up a Jenkins declarative pipeline for a Django Notes App!

## Author
Dhruv Doshi


