# Devops-Tools-Installation-guide

We are Installing a lot of Devops tools from different websites everyday. Here we are trying to reduce the pain of searching them on on lot of websites.
Let’s combine all of them in a single place and make installations hassle free.

Prerequisites
This tutorial requires an Ubuntu 20.04 system configured with a non-root user with sudo privileges.

 # 1. Installing Golang  

Golang is one of the leading languages that DevOps professionals adopt, and this is due to the characteristics the language has. Golang is an open source programming language developed by Google to make software building easy, efficient, and reliable. Go is designed to make coding more user-friendly with the built-in garbage-collection, fast compilation, and high performance even when building a large-scale project or complex software.

The prominent players in the DevOps community adopt Go not because it’s so unique but because it makes the work easier while increasing performance

As of this writing, the latest release is go1.16.7. To install Go on an Ubuntu server (or any Linux server, for that matter), copy the URL of the file ending with linux-amd64.tar.gz.

Now that you have your link ready, first confirm that you’re in the home directory:

``` 
$ cd ~ 
```

Then use curl to retrieve the tarball, making sure to replace the highlighted URL with the one you just copied. The -O flag ensures that this outputs to a file, and the L flag instructs HTTPS redirects, since this link was taken from the Go website and will redirect here before the file downloads:

```
 $ curl -OL https://golang.org/dl/go1.16.7.linux-amd64.tar.gz
```

To verify the integrity of the file you downloaded, run the sha256sum command and pass it to the filename as an argument:
```
$ sha256sum go1.16.7.linux-amd64.tar.gz
```
This will return the tarball’s SHA256 checksum:
```
Outputgo1.16.7.linux-amd64.tar.gz
7fe7a73f55ba3e2285da36f8b085e5c0159e9564ef5f63ee0ed6b818ade8ef04  go1.16.7.linux-amd64.tar.gz
```
If the checksum matches the one listed on the downloads page, you’ve done this step correctly.

Next, use tar to extract the tarball. This command includes the -C flag which instructs tar to change to the given directory before performing any other operations. This means that the extracted files will be written to the /usr/local/ directory, the recommended location for installing Go… The x flag tells tarto extract, v tells it we want verbose output (a listing of the files being extracted), and f tells it we’ll specify a filename:
```
$ sudo tar -C /usr/local -xvf go1.16.7.linux-amd64.tar.gz
```
Although /usr/local/go is the recommended location for installing Go, some users may prefer or require different paths.

Setting Go Paths
In this step, you will set paths in your environment.

First, set Go’s root value, which tells Go where to look for its files. You can do this by editing the .profilefile, which contains a list of commands that the system runs every time you log in.

Use your preferred editor to open .profile, which is stored in your user’s home directory. Here, we’ll use nano:
```
 $ sudo nano ~/.profile
```
Then, add the following information to the end of your file:
sudo nano ~/.profile
```
#Add the following lines at bottom
export PATH=$PATH:/usr/local/go/bin
```

After you’ve added this information to your profile, save and close the file. If you used nano, do so by pressing CTRL+X, then Y, and then ENTER.

Next, refresh your profile by running the following command:
```
$ source ~/.profile
```
After, check if you can execute go commands by running go version:
```
$ go version
```
This command will output the release number of whatever version of Go is installed on your system:
```
Outputgo version go1.16.7 linux/amd64
```
This output confirms that you are now running Go on your server.
# 2. Docker Engine  

Docker Engine is an open source containerization technology for building and containerizing your applications. Docker Engine acts as a client-server application with:

A server with a long-running daemon process dockerd.
APIs which specify interfaces that programs can use to talk to and instruct the Docker daemon.
A command line interface (CLI) client docker.
The CLI uses Docker APIs to control or interact with the Docker daemon through scripting or direct CLI commands. Many other Docker applications use the underlying API and CLI. The daemon creates and manage Docker objects, such as images, containers, networks, and volumes.

For more details, see Docker Architecture.

Uninstall old versions
Older versions of Docker were called docker, docker.io, or docker-engine. If these are installed, uninstall them:
```
$ sudo apt-get remove docker docker-engine docker.io containerd runc
```
Set up the repository
Update the apt package index and install packages to allow apt to use a repository over HTTPS:
```
$ sudo apt-get update $ sudo apt-get install \ ca-certificates \ curl \ gnupg \ lsb-release
```
Add Docker’s official GPG key:
```
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg 
```
Use the following command to set up the stable repository. To add the nightly or test repository, add the word nightly or test (or both) after the word stable in the commands below. Learn about nightly and test channels.
```
$ echo \ "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \ $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
Install Docker Engine
Update the apt package index, and install the latest version of Docker Engine and containerd, or go to the next step to install a specific version:
```
$ sudo apt-get update 
$ sudo apt-get install docker-ce docker-ce-cli containerd.io
```
# 3. Docker Compose
Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application’s services. Then, with a single command, you create and start all the services from your configuration. To learn more about all the features of Compose, see the list of features.

Compose works in all environments: production, staging, development, testing, as well as CI workflows. You can learn more about each case in Common Use Cases.
Run this command to download the current stable release of Docker Compose:
```
$ sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose 
```
To install a different version of Compose, substitute 1.29.2 with the version of Compose you want to use. For instructions on how to install Compose 2.2.3 on Linux, see Install Compose 2.0.0 on LinuxIf you have problems installing with curl, see Alternative Install Options tab above.

Apply executable permissions to the binary:
```
$ sudo chmod +x /usr/local/bin/docker-compose
```
> Note:  
>If the command docker-compose fails after installation, check your path. You can also create a symbolic link to /usr/bin or any other directory in your path.  



Test the installation.
```
$ docker-compose --version
```
# 4. kubectl
The Kubernetes command-line tool, kubectl, allows you to run commands against Kubernetes clusters. You can use kubectl to deploy applications, inspect and manage cluster resources, and view logs. For more information including a complete list of kubectl operations, see the kubectl reference documentation.

kubectl is installable on a variety of Linux platforms, macOS and Windows

Download the latest release with the command:
```
$ curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
```
Validate the binary (optional)
Download the kubectl checksum file:
```
curl -LO "https://dl.k8s.io/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
```
Install kubectl
```
$ sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```
 >Note:  
>If you do not have root access on the target system, you can still install kubectl to the ~/.local/bin directory:
```
chmod +x kubectl
mkdir -p ~/.local/bin
mv ./kubectl ~/.local/bin/kubectl # and then append (or prepend) ~/.local/bin to $PATH
```
Test to ensure the version you installed is up-to-date:
```
$ kubectl version --client
```
# 5. MicroK8s
MicroK8s is a CNCF certified upstream Kubernetes deployment that runs entirely on your workstation or edge device. Being a snap it runs all Kubernetes services natively (i.e. no virtual machines) while packing the entire set of libraries and binaries needed. Installation is limited by how fast you can download a couple of hundred megabytes and the removal of MicroK8s leaves nothing behind.

Install via Snap :
```
$ sudo snap install microk8s --classic
```
Check the status while Kubernetes starts
```
$ microk8s status --wait-ready
```
Turn on the services you want
```
$ microk8s enable dashboard dns registry istio
```
Start using Kubernetes
```
$ microk8s kubectl get all --all-namespaces
```
# 6. minikube
minikube is local Kubernetes, focusing on making it easy to learn and develop for Kubernetes.

All you need is Docker (or similarly compatible) container or a Virtual Machine environment, and Kubernetes is a single command away: minikube start

Installation:
```
$ curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64

$ sudo install minikube-linux-amd64 /usr/local/bin/minikube
```
Start your cluster
```
$ minikube start
```
# 7. Kompose
Kompose is a conversion tool for Docker Compose to container orchestrators such as Kubernetes (or OpenShift).

A deb package is released for compose. Download latest package in the assets in github releases.
```
$ wget https://github.com/kubernetes/kompose/releases/download/v1.26.1/kompose_1.26.1_amd64.deb # Replace 1.26.1 with latest tag

$ sudo apt install ./kompose_1.26.1_amd64.deb

```
# 8. Helm
Helm is a Kubernetes deployment tool for automating creation, packaging, configuration, and deployment of applications and services to Kubernetes clusters. 

Helm now has an installer script that will automatically grab the latest version of Helm and install it locally.

You can fetch that script, and then execute it locally. It’s well documented so that you can read through it and understand what it is doing before you run it.
```
$ curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
$ chmod 700 get_helm.sh
$ ./get_helm.sh
```
# 9. Ansible
Ansible is an open-source software provisioning, configuration management, and application-deployment tool enabling infrastructure as code. It runs on many Unix-like systems, and can configure both Unix-like systems as well as Microsoft Windows.

Install
Ubuntu builds are available in a PPA here.

To configure the PPA on your machine and install Ansible run these commands:
```
$ sudo apt update
$ sudo apt install software-properties-common
$ sudo add-apt-repository --yes --update ppa:ansible/ansible
$ sudo apt install ansible
```
# 10. Terraform
Terraform is an open-source infrastructure as code software tool created by HashiCorp. Users define and provide data center infrastructure using a declarative configuration language known as HashiCorp Configuration Language, or optionally JSON.

To install Terraform, find the appropriate package for your system and download it as a zip archive.

After downloading Terraform, unzip the package. Terraform runs as a single binary named terraform. Any other files in the package can be safely removed and Terraform will still function.

Finally, make sure that the terraform binary is available on your PATH. This process will differ depending on your operating system.

Print a colon-separated list of locations in your PATH.
```
$ echo $PATH
```
Move the Terraform binary to one of the listed locations. This command assumes that the binary is currently in your downloads folder and that your PATH includes /usr/local/bin, but you can customize it if your locations are different.
```
$ mv ~/Downloads/terraform /usr/local/bin/
```
For more detail about adding binaries to your path, see this Stack Overflow article.

Verify the installation
Verify that the installation worked by opening a new terminal session and listing Terraform’s available subcommands.
```
$ terraform -help
Usage: terraform [-version] [-help] <command> [args]
```
The available commands for execution are listed below.
The most common, useful commands are shown first, followed by
less common or more advanced commands. If you're just getting
started with Terraform, stick with the common commands. For the
other commands, please read the help and docs before usage.
##...
Add any subcommand to terraform -help to learn more about what it does and available options.
```
$ terraform -help plan
```
# 11. AWS Command Line Interface
The AWS Command Line Interface (CLI) is a unified tool to manage your AWS services. With just one tool to download and configure, you can control multiple AWS services from the command line and automate them through scripts.

The AWS CLI v2 offers several new features including improved installers, new configuration options such as AWS Single Sign-On (SSO), and various interactive features. 

Install or update the AWS CLI
```
$ curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" unzip awscliv2.zip sudo ./aws/install
```
# 12. gcloud CLI
The Google Cloud CLI is a set of tools to create and manage Google Cloud resources. You can use these tools to perform many common platform tasks from the command line or through scripts and other automation.

download the Linux 64-bit archive file, at the command line, run:
```
$ curl -O https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-sdk-379.0.0-linux-x86_64.tar.gz
```
Extract the contents of the file to any location on your file system (preferably your Home directory). To replace an existing installation, remove the existing google-cloud-sdk directory and then extract the archive to the same location.
```
$ tar -xf google-cloud-sdk-379.0.0-linux-x86.tar.gz
```
Use the install script to add the gcloud CLI tools to your PATH. You can also opt-in to command-completion for your shell and usage statistics collection.

Run the script (from the root of the folder you extracted to) using the following command:
```
/google-cloud-sdk/install.sh
```
