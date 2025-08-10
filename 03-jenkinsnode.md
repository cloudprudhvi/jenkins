## What is a Jenkins Node?
- **Master (Controller):** The main Jenkins server that manages jobs and resources.
- **Node (Agent/Slave):** A machine (physical or virtual) that runs build jobs, controlled by the master.
- **Why use nodes?**
  - Distribute builds across multiple machines.
  - Run jobs on different operating systems or environments.
  - Scale Jenkins for larger teams or projects.

---

## Types of Jenkins Nodes
- **Built-in Node:** The Jenkins master itself (not recommended for heavy builds).
- **Permanent Agent:** A node always available to Jenkins.
- **Cloud/On-demand Agent:** Dynamically provisioned (e.g., via Docker, Kubernetes, or cloud plugins).

---

## Prerequisites
- Jenkins master is installed and running.
- The agent machine can connect to the master (network/firewall rules allow communication).
- Java is installed on the agent (usually required for the agent.jar method).

---

## Step 1: Add a New Node in Jenkins
1. Go to **Manage Jenkins** > **Manage Nodes and Clouds**.
2. Click **New Node**.
3. Enter a name (e.g., `linux-agent-1`).
4. Select **Permanent Agent** and click **OK**.

---

## Step 2: Configure Node Details
- **Name:** Unique identifier (e.g., `linux-agent-1`).
- **Description:** (Optional) Purpose of the node.
- **# of Executors:** Number of jobs this node can run in parallel (usually 1-2 for small VMs).
- **Remote root directory:** Directory on the agent where Jenkins stores files (e.g., `/home/jenkins`).
- **Labels:** Tags to group/select nodes (e.g., `linux`, `docker`).
- **Usage:**
  - **Use this node as much as possible** (default)
  - **Only build jobs with label expressions matching this node**
- **Launch method:** How Jenkins connects to the agent (see below).

---

## Step 3: Choose a Launch Method
### 1. Launch agent via SSH
- **Recommended for Linux agents.**
- Jenkins connects to the agent using SSH and starts the agent process.
- **Fields:**
  - Host (IP or DNS)
  - Credentials (username/password or SSH key)
  - Host Key Verification Strategy
- **Example:**
  - Host: `192.168.1.100`
  - Credentials: `jenkins-ssh-key`

### 2. Launch agent via Java Web Start (agent.jar)
- **Manual method.**
- Download `agent.jar` from Jenkins master and run it on the agent:
  ```sh
  java -jar agent.jar -jnlpUrl http://<jenkins-master>:8080/computer/<node-name>/slave-agent.jnlp -secret <secret-key>
  ```
---

## Step 3: Verify Node Connection
- After setup, the node should show as **online** in the Jenkins UI.
- You can run a simple job with a label matching the node to test it.

---