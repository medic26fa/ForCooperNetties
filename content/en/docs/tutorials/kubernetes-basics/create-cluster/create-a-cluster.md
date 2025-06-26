---
title: "Module 1 - Create a Kubernetes Cluster"
weight: 1
--- 

## Objectives
- Learn what a Kubernetes cluster is.
- Use minikube to create Kubernetes cluster on your computer.
- Use basic kubectl commands to inspect your cluster.


A Kubernetes cluster is a set of machines that run your containerized applications. In this tutorial, you’ll create a local cluster for development using _minikube_, Kubernetes' lightweight solution for creating a VM on your local machine and for deploying a simple cluster containing only one node. In addition, you'll use kubectl - the command-line tool for interacting with your cluster.

### Before You Begin

If you haven't already, first install minikube:

<script language="javascript"> const architectures = [ "Linux/x86-64", "Linux/ARM64", "Linux/ppc64", "Linux/S390x", "Linux/ARMv7", "macOS/x86-64", "macOS/ARM64", "Windows/x86-64" ]; console.time("timerReleaseFetch"); fetch('https://api.github.com/repos/kubernetes/minikube/releases') .then((response) => response.json()) .then((releases) => { console.timeEnd("timerReleaseFetch"); if (releases && releases.length > 0 && releases[0] && releases[0].tag_name) { const isBetaMostRecent = releases[0].tag_name.includes("-beta"); if (isBetaMostRecent) { for (architecture of architectures) { const betaElement = document.querySelector(`button[data-quiz-id="/${architecture}/Beta"]`); if (betaElement) { betaElement.classList.remove("hide"); } } } } }) .catch(console.error); </script>
{{% card %}}

Click on the buttons that describe your target platform. For other architectures, see the release page for a complete list of minikube binaries.

{{% quiz_row base="" name="Operating system" %}} {{% quiz_button option="Linux" %}} {{% quiz_button option="macOS" %}} {{% quiz_button option="Windows" %}} {{% /quiz_row %}}

{{% quiz_row base="/Linux" name="Architecture" %}} {{% quiz_button option="x86-64" %}} {{% quiz_button option="ARM64" %}} {{% quiz_button option="ARMv7" %}} {{% quiz_button option="ppc64" %}} {{% quiz_button option="S390x" %}} {{% /quiz_row %}}

{{% quiz_row base="/Linux/x86-64" name="Release type" %}} {{% quiz_button option="Stable" %}} {{% quiz_button option="Beta" hide="true" %}} {{% /quiz_row %}}

{{% quiz_row base="/Linux/x86-64/Stable" name="Installer type" %}} {{% quiz_button option="Binary download" %}} {{% quiz_button option="Debian package" %}} {{% quiz_button option="RPM package" %}} {{% /quiz_row %}}

{{% quiz_row base="/Linux/x86-64/Beta" name="Installer type" %}} {{% quiz_button option="Binary download" %}} {{% quiz_button option="Debian package" %}} {{% quiz_button option="RPM package" %}} {{% /quiz_row %}}

{{% quiz_row base="/Linux/ARM64" name="Release type" %}} {{% quiz_button option="Stable" %}} {{% quiz_button option="Beta" hide="true" %}} {{% /quiz_row %}}

{{% quiz_row base="/Linux/ARM64/Stable" name="Installer type" %}} {{% quiz_button option="Binary download" %}} {{% quiz_button option="Debian package" %}} {{% quiz_button option="RPM package" %}} {{% /quiz_row %}}

{{% quiz_row base="/Linux/ARM64/Beta" name="Installer type" %}} {{% quiz_button option="Binary download" %}} {{% quiz_button option="Debian package" %}} {{% quiz_button option="RPM package" %}} {{% /quiz_row %}}

{{% quiz_row base="/Linux/ppc64" name="Release type" %}} {{% quiz_button option="Stable" %}} {{% quiz_button option="Beta" hide="true" %}} {{% /quiz_row %}}

{{% quiz_row base="/Linux/ppc64/Stable" name="Installer type" %}} {{% quiz_button option="Binary download" %}} {{% quiz_button option="Debian package" %}} {{% quiz_button option="RPM package" %}} {{% /quiz_row %}}

{{% quiz_row base="/Linux/ppc64/Beta" name="Installer type" %}} {{% quiz_button option="Binary download" %}} {{% quiz_button option="Debian package" %}} {{% quiz_button option="RPM package" %}} {{% /quiz_row %}}

{{% quiz_row base="/Linux/S390x" name="Release type" %}} {{% quiz_button option="Stable" %}} {{% quiz_button option="Beta" hide="true" %}} {{% /quiz_row %}}

{{% quiz_row base="/Linux/S390x/Stable" name="Installer type" %}} {{% quiz_button option="Binary download" %}} {{% quiz_button option="Debian package" %}} {{% quiz_button option="RPM package" %}} {{% /quiz_row %}}

{{% quiz_row base="/Linux/S390x/Beta" name="Installer type" %}} {{% quiz_button option="Binary download" %}} {{% quiz_button option="Debian package" %}} {{% quiz_button option="RPM package" %}} {{% /quiz_row %}}

{{% quiz_row base="/Linux/ARMv7" name="Release type" %}} {{% quiz_button option="Stable" %}} {{% quiz_button option="Beta" hide="true" %}} {{% /quiz_row %}}

{{% quiz_row base="/Linux/ARMv7/Stable" name="Installer type" %}} {{% quiz_button option="Binary download" %}} {{% quiz_button option="Debian package" %}} {{% quiz_button option="RPM package" %}} {{% /quiz_row %}}

{{% quiz_row base="/Linux/ARMv7/Beta" name="Installer type" %}} {{% quiz_button option="Binary download" %}} {{% quiz_button option="Debian package" %}} {{% quiz_button option="RPM package" %}} {{% /quiz_row %}}

{{% quiz_row base="/macOS" name="Architecture" %}} {{% quiz_button option="x86-64" %}} {{% quiz_button option="ARM64" %}} {{% /quiz_row %}}

{{% quiz_row base="/macOS/x86-64" name="Release type" %}} {{% quiz_button option="Stable" %}} {{% quiz_button option="Beta" hide="true" %}} {{% /quiz_row %}}

{{% quiz_row base="/macOS/x86-64/Stable" name="Installer type" %}} {{% quiz_button option="Binary download" %}} {{% quiz_button option="Homebrew" %}} {{% /quiz_row %}}

{{% quiz_row base="/macOS/x86-64/Beta" name="Installer type" %}} {{% quiz_button option="Binary download" %}} {{% /quiz_row %}}

{{% quiz_row base="/macOS/ARM64" name="Release type" %}} {{% quiz_button option="Stable" %}} {{% quiz_button option="Beta" hide="true" %}} {{% /quiz_row %}}

{{% quiz_row base="/macOS/ARM64/Stable" name="Installer type" %}} {{% quiz_button option="Binary download" %}} {{% quiz_button option="Homebrew" %}} {{% /quiz_row %}}

{{% quiz_row base="/macOS/ARM64/Beta" name="Installer type" %}} {{% quiz_button option="Binary download" %}} {{% /quiz_row %}}

{{% quiz_row base="/Windows" name="Architecture" %}} {{% quiz_button option="x86-64" %}} {{% /quiz_row %}}

{{% quiz_row base="/Windows/x86-64" name="Release type" %}} {{% quiz_button option="Stable" %}} {{% quiz_button option="Beta" hide="true" %}} {{% /quiz_row %}}

{{% quiz_row base="/Windows/x86-64/Stable" name="Installer type" %}} {{% quiz_button option=".exe download" %}} {{% quiz_button option="Windows Package Manager" %}} {{% quiz_button option="Chocolatey" %}} {{% /quiz_row %}}

{{% quiz_row base="/Windows/x86-64/Beta" name="Installer type" %}} {{% quiz_button option=".exe download" %}} {{% /quiz_row %}}

{{% quiz_instruction id="/Linux/x86-64/Stable/Binary download" %}}

curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64
{{% /quiz_instruction %}}

{{% quiz_instruction id="/Linux/x86-64/Beta/Binary download" %}}

r=https://api.github.com/repos/kubernetes/minikube/releases
curl -LO $(curl -s $r | grep -o 'http.*download/v.*beta.*/minikube-linux-amd64' | head -n1)
sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64
{{% /quiz_instruction %}}

{{% quiz_instruction id="/Linux/x86-64/Stable/Debian package" %}}

curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb
sudo dpkg -i minikube_latest_amd64.deb
{{% /quiz_instruction %}}

{{% quiz_instruction id="/Linux/x86-64/Beta/Debian package" %}}

r=https://api.github.com/repos/kubernetes/minikube/releases
u=$(curl -s $r | grep -o 'http.*download/v.*beta.*/minikube_.*_amd64.deb' | head -n1)
curl -L $u > minikube_beta_amd64.deb && sudo dpkg -i minikube_beta_amd64.deb
{{% /quiz_instruction %}}

{{% quiz_instruction id="/Linux/x86-64/Stable/RPM package" %}}

curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-latest.x86_64.rpm
sudo rpm -Uvh minikube-latest.x86_64.rpm
{{% /quiz_instruction %}}

{{% quiz_instruction id="/Linux/x86-64/Beta/RPM package" %}}

r=https://api.github.com/repos/kubernetes/minikube/releases
u=$(curl -s $r | grep -o 'http.*download/v.*beta.*/minikube-.*.x86_64.rpm' | head -n1)
curl -L $u > minikube-beta.x86_64.rpm && sudo rpm -Uvh minikube-beta.x86_64.rpm
{{% /quiz_instruction %}}

{{% quiz_instruction id="/Linux/ARM64/Stable/Binary download" %}}

curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-linux-arm64
sudo install minikube-linux-arm64 /usr/local/bin/minikube && rm minikube-linux-arm64
{{% /quiz_instruction %}}

{{% quiz_instruction id="/Linux/ARM64/Beta/Binary download" %}}

r=https://api.github.com/repos/kubernetes/minikube/releases
curl -LO $(curl -s $r | grep -o 'http.*download/v.*beta.*/minikube-linux-arm64' | head -n1)
sudo install minikube-linux-arm64 /usr/local/bin/minikube && rm minikube-linux-arm64
{{% /quiz_instruction %}}

{{% quiz_instruction id="/Linux/ARM64/Stable/Debian package" %}}

curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_arm64.deb
sudo dpkg -i minikube_latest_arm64.deb
{{% /quiz_instruction %}}

{{% quiz_instruction id="/Linux/ARM64/Beta/Debian package" %}}

r=https://api.github.com/repos/kubernetes/minikube/releases
u=$(curl -s $r | grep -o 'http.*download/v.*beta.*/minikube_.*_arm64.deb' | head -n1)
curl -L $u > minikube_beta_arm64.deb && sudo dpkg -i minikube_beta_arm64.deb
{{% /quiz_instruction %}}

{{% quiz_instruction id="/Linux/ARM64/Stable/RPM package" %}}

curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-latest.aarch64.rpm
sudo rpm -Uvh minikube-latest.aarch64.rpm
{{% /quiz_instruction %}}

{{% quiz_instruction id="/Linux/ARM64/Beta/RPM package" %}}

r=https://api.github.com/repos/kubernetes/minikube/releases
u=$(curl -s $r | grep -o 'http.*download/v.*beta.*/minikube-.*.aarch64.rpm' | head -n1)
curl -L $u > minikube-beta.aarch64.rpm && sudo rpm -Uvh minikube-beta.aarch64.rpm
{{% /quiz_instruction %}}

{{% quiz_instruction id="/Linux/ppc64/Stable/Binary download" %}}

curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-linux-ppc64le
sudo install minikube-linux-ppc64le /usr/local/bin/minikube && rm minikube-linux-ppc64le
{{% /quiz_instruction %}}

{{% quiz_instruction id="/Linux/ppc64/Beta/Binary download" %}}

r=https://api.github.com/repos/kubernetes/minikube/releases
curl -LO $(curl -s $r | grep -o 'http.*download/v.*beta.*/minikube-linux-ppc64le' | head -n1)
sudo install minikube-linux-ppc64le /usr/local/bin/minikube && rm minikube-linux-ppc64le
{{% /quiz_instruction %}}

{{% quiz_instruction id="/Linux/ppc64/Stable/Debian package" %}}

curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_ppc64le.deb
sudo dpkg -i minikube_latest_ppc64le.deb
{{% /quiz_instruction %}}

{{% quiz_instruction id="/Linux/ppc64/Beta/Debian package" %}}

r=https://api.github.com/repos/kubernetes/minikube/releases
u=$(curl -s $r | grep -o 'http.*download/v.*beta.*/minikube_.*_ppc64le.deb' | head -n1)
curl -L $u > minikube_beta_ppc64le.deb && sudo dpkg -i minikube_beta_ppc64le.deb
{{% /quiz_instruction %}}

{{% quiz_instruction id="/Linux/ppc64/Stable/RPM package" %}}

curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-latest.ppc64el.rpm
sudo rpm -Uvh minikube-latest.ppc64el.rpm
{{% /quiz_instruction %}}

{{% quiz_instruction id="/Linux/ppc64/Beta/RPM package" %}}

r=https://api.github.com/repos/kubernetes/minikube/releases
u=$(curl -s $r | grep -o 'http.*download/v.*beta.*/minikube-.*.ppc64el.rpm' | head -n1)
curl -L $u > minikube-beta.ppc64el.rpm && sudo rpm -Uvh minikube-beta.ppc64el.rpm
{{% /quiz_instruction %}}

{{% quiz_instruction id="/Linux/S390x/Stable/Binary download" %}}

curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-linux-s390x
sudo install minikube-linux-s390x /usr/local/bin/minikube && rm minikube-linux-s390x
{{% /quiz_instruction %}}

{{% quiz_instruction id="/Linux/S390x/Beta/Binary download" %}}

r=https://api.github.com/repos/kubernetes/minikube/releases
curl -LO $(curl -s $r | grep -o 'http.*download/v.*beta.*/minikube-linux-s390x' | head -n1)
sudo install minikube-linux-s390x /usr/local/bin/minikube && rm minikube-linux-s390x
{{% /quiz_instruction %}}

{{% quiz_instruction id="/Linux/S390x/Stable/Debian package" %}}

curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_s390x.deb
sudo dpkg -i minikube_latest_s390x.deb
{{% /quiz_instruction %}}

{{% quiz_instruction id="/Linux/S390x/Beta/Debian package" %}}

r=https://api.github.com/repos/kubernetes/minikube/releases
u=$(curl -s $r | grep -o 'http.*download/v.*beta.*/minikube_.*_s390x.deb' | head -n1)
curl -L $u > minikube_beta_s390x.deb && sudo dpkg -i minikube_beta_s390x.deb
{{% /quiz_instruction %}}

{{% quiz_instruction id="/Linux/S390x/Stable/RPM package" %}}

curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-latest.s390x.rpm
sudo rpm -Uvh minikube-latest.s390x.rpm
{{% /quiz_instruction %}}

{{% quiz_instruction id="/Linux/S390x/Beta/RPM package" %}}

r=https://api.github.com/repos/kubernetes/minikube/releases
u=$(curl -s $r | grep -o 'http.*download/v.*beta.*/minikube-.*.s390x.rpm' | head -n1)
curl -L $u > minikube-beta.s390x.rpm && sudo rpm -Uvh minikube-beta.s390x.rpm
{{% /quiz_instruction %}}

{{% quiz_instruction id="/Linux/ARMv7/Stable/Binary download" %}}

curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-linux-arm
sudo install minikube-linux-arm /usr/local/bin/minikube && rm minikube-linux-arm
{{% /quiz_instruction %}}

{{% quiz_instruction id="/Linux/ARMv7/Beta/Binary download" %}}

r=https://api.github.com/repos/kubernetes/minikube/releases
curl -LO $(curl -s $r | grep -o 'http.*download/v.*beta.*/minikube-linux-arm' | head -n1)
sudo install minikube-linux-arm /usr/local/bin/minikube && rm minikube-linux-arm
{{% /quiz_instruction %}}

{{% quiz_instruction id="/Linux/ARMv7/Stable/Debian package" %}}

curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_armhf.deb
sudo dpkg -i minikube_latest_armhf.deb
{{% /quiz_instruction %}}

{{% quiz_instruction id="/Linux/ARMv7/Beta/Debian package" %}}

r=https://api.github.com/repos/kubernetes/minikube/releases
u=$(curl -s $r | grep -o 'http.*download/v.*beta.*/minikube_.*_armhf.deb' | head -n1)
curl -L $u > minikube_beta_armhf.deb && sudo dpkg -i minikube_beta_armhf.deb
{{% /quiz_instruction %}}

{{% quiz_instruction id="/Linux/ARMv7/Stable/RPM package" %}}

curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-latest.armv7hl.rpm
sudo rpm -Uvh minikube-latest.armv7hl.rpm
{{% /quiz_instruction %}}

{{% quiz_instruction id="/Linux/ARMv7/Beta/RPM package" %}}

r=https://api.github.com/repos/kubernetes/minikube/releases
u=$(curl -s $r | grep -o 'http.*download/v.*beta.*/minikube-.*.armv7hl.rpm' | head -n1)
curl -L $u > minikube-beta.armv7hl.rpm && sudo rpm -Uvh minikube-beta.armv7hl.rpm
{{% /quiz_instruction %}}

{{% quiz_instruction id="/macOS/x86-64/Stable/Homebrew" %}} If the Homebrew Package Manager is installed:

brew install minikube
If which minikube fails after installation via brew, you may have to remove the old minikube links and link the newly installed binary:

brew unlink minikube
brew link minikube
{{% /quiz_instruction %}}

{{% quiz_instruction id="/macOS/x86-64/Stable/Binary download" %}}

curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-darwin-amd64
sudo install minikube-darwin-amd64 /usr/local/bin/minikube
{{% /quiz_instruction %}}

{{% quiz_instruction id="/macOS/x86-64/Beta/Binary download" %}}

r=https://api.github.com/repos/kubernetes/minikube/releases
curl -LO $(curl -s $r | grep -o 'http.*download/v.*beta.*/minikube-darwin-amd64' | head -n1)
sudo install minikube-darwin-amd64 /usr/local/bin/minikube
{{% /quiz_instruction %}}

{{% quiz_instruction id="/macOS/ARM64/Stable/Binary download" %}}

curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-darwin-arm64
sudo install minikube-darwin-arm64 /usr/local/bin/minikube
{{% /quiz_instruction %}}

{{% quiz_instruction id="/macOS/ARM64/Stable/Homebrew" %}} If the Homebrew Package Manager is installed:

brew install minikube
If which minikube fails after installation via brew, you may have to remove the old minikube links and link the newly installed binary:

brew unlink minikube
brew link minikube
{{% /quiz_instruction %}}

{{% quiz_instruction id="/macOS/ARM64/Beta/Binary download" %}}

r=https://api.github.com/repos/kubernetes/minikube/releases
curl -LO $(curl -s $r | grep -o 'http.*download/v.*beta.*/minikube-darwin-arm64' | head -n1)
sudo install minikube-darwin-arm64 /usr/local/bin/minikube
{{% /quiz_instruction %}}

{{% quiz_instruction id="/Windows/x86-64/Stable/Windows Package Manager" %}} If the Windows Package Manager is installed, use the following command to install minikube:

winget install Kubernetes.minikube
{{% /quiz_instruction %}}

{{% quiz_instruction id="/Windows/x86-64/Stable/Chocolatey" %}} If the Chocolatey Package Manager is installed, use the following command:

choco install minikube
{{% /quiz_instruction %}}

{{% quiz_instruction id="/Windows/x86-64/Stable/.exe download" %}}

Download and run the installer for the latest release.

Or if using `PowerShell`, use this command: ```powershell New-Item -Path 'c:\' -Name 'minikube' -ItemType Directory -Force $ProgressPreference = 'SilentlyContinue'; Invoke-WebRequest -OutFile 'c:\minikube\minikube.exe' -Uri 'https://github.com/kubernetes/minikube/releases/latest/download/minikube-windows-amd64.exe' -UseBasicParsing ```
Add the minikube.exe binary to your PATH.

_Make sure to run PowerShell as Administrator._ ```powershell $oldPath = [Environment]::GetEnvironmentVariable('Path', [EnvironmentVariableTarget]::Machine) if ($oldPath.Split(';') -inotcontains 'C:\minikube'){ [Environment]::SetEnvironmentVariable('Path', $('{0};C:\minikube' -f $oldPath), [EnvironmentVariableTarget]::Machine) } ``` If you used a terminal (like powershell) for the installation, please close the terminal and reopen it before running minikube. {{% /quiz_instruction %}}
{{% quiz_instruction id="/Windows/x86-64/Beta/.exe download" %}}

Download and run the installer for the latest beta release.

Or if using `PowerShell`, use this command: ```powershell New-Item -Path 'c:\' -Name 'minikube' -ItemType Directory -Force $response = Invoke-WebRequest -Uri 'https://api.github.com/repos/kubernetes/minikube/releases' -UseBasicParsing $json = $response.Content | ConvertFrom-Json $item = ($json | ?{ $_.prerelease -eq $true })[0].assets | ?{ $_.name -eq 'minikube-windows-amd64.exe' } Invoke-WebRequest -Uri $item.browser_download_url -OutFile 'c:\minikube\minikube.exe' -UseBasicParsing ```
Add the minikube.exe binary to your PATH.

_Make sure to run PowerShell as Administrator._ ```powershell $oldPath = [Environment]::GetEnvironmentVariable('Path', [EnvironmentVariableTarget]::Machine) if ($oldPath.Split(';') -inotcontains 'C:\minikube'){ [Environment]::SetEnvironmentVariable('Path', $('{0};C:\minikube' -f $oldPath), [EnvironmentVariableTarget]::Machine) } ``` _If you used a CLI to perform the installation, you will need to close that CLI and open a new one before proceeding._ <script type="text/javascript"> fetch("https://api.github.com/repos/kubernetes/minikube/releases") .then(response => response.text()) .then(data => { const u = data.match(/http.*download\/v.*beta.*\/minikube-installer\.exe/)[0] document.getElementById("latest-beta-download-link").href = u; }) .catch((error) => { const el = document.getElementById("latest-beta-download-link"); el.innerHTML = "latest beta from the release page"; el.href = "https://github.com/kubernetes/minikube/releases"; }); </script> {{% /quiz_instruction %}}
{{% /card %}}

To check if minikube is properly installed, run the *minikube version* command:

```shell
minikube version
```

## Step 1 - Start the Cluster


Once minikube is installed, create a cluster by running the *minikube start* command:

```shell
minikube start
```

The _minikube start_ command:

1. **Chooses and launches a driver** — minikube picks a backend to host your cluster, such as Docker or hyperkit.

  {{< note >}} If minikube couldn't auto-detect a supported driver when running the command, ensure an appropriate driver is installed and try again.   You can also specify a driver in the command (for example: `minikube start --driver=docker`).

2. **Downloads Kubernetes components** — minikube downloads either the version of Kubernetes you requested or the latest stable version by default.

3. **Creates a single-node cluster** — This node acts as both the [control plane and the worker](https://kubernetes.io/docs/tutorials/kubernetes-basics/create-cluster/cluster-intro/).

4. **Configures kubectl** — So you can run commands and manage your cluster from the terminal.

You should now have a running Kubernetes cluster in your terminal! minikube started a virtual environment for you, and a Kubernetes cluster is now running in that environment. 

### Optional: Try the minikube Dashboard!

Prefer a GUI? You can launch a visual interface for your cluster using:

```bash
minikube dashboard
```

## Step 2 - Inspect Your Cluster

Let's use kubectl to find out some details about the cluster you just created! Run the *kubectl version* command:

```shell
kubectl version
```
If kubectl is configured, you should see both the version of the client and the server. The client version is the _kubectl version_; the server version is the _Kubernetes version_ installed on the control plane. You should also see details about the build.


Next, let's view the cluster details. Run the *kubectl cluster-info* command:

```shell
kubectl cluster-info
```

The `kubectl cluster-info` command gives you a quick summary of your Kubernetes control plane components. It will tell you if your cluster is up and  where it's running. 


Finally, to view the nodes in the cluster, run the *kubectl get nodes* command:

```shell
kubectl get nodes
```

The `kubectl get nodes` command shows you all the nodes in your Kubernetes cluster. Recall that a node is a machine (virtual or physical) that is part of your Kubernetes cluster. In a minikube cluster, you'll usually see just one node. This command is helpful for checking if nodes are ready to run workloads and when debugging issues.



🎉 **You Did It!**

You just launched a real Kubernetes cluster on your machine, connected it to your command line, and explored it with kubectl.

## What's Next

Congratulations! In this tutorial, you:

- Created a single-node Kubernetes cluster on your local machine. <br/>
- Installed the Kubernetes control plane and worker (in this case as the single node). <br/>
- Configured kubectl to communicate with the newly created cluster. <br/>
- Set up a fully functioning local environment where you can deploy apps, experiment, and manage everything with Kubernetes commands.

In the next tutorial, you'll be able to deploy your first pod - the basic unit of work in Kubernetes.

{{% button link="/docs/tutorials/kubernetes_101/module2" %}}Next: Deploy Your First Pod{{% /button %}}
