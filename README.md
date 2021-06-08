# Ansible Kubernetes Workshop

Introduction to deploying on Kubernetes using Ansible.

## Preparing for the Workshop

### Local Setup

Before taking part in the workshop, please make sure your local system has the following applications installed:

- [Ansible 2.9+](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
- [Kubectl](https://kubernetes.io/docs/tasks/tools/)
- [Helm 3](https://helm.sh/docs/intro/install/)

You will also need a copy of this repository:

- [Download a zip](https://github.com/JeffreyDD/ansible-kubernetes-workshop/archive/refs/heads/complete.zip)
- ```
  git clone https://github.com/JeffreyDD/ansible-kubernetes-workshop.git
  cd ansible-kubernetes-workshop
  git checkout complete
  ```


Not required, but recommended for an optimal experience:
- [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
- [Visual Studio Code](https://code.visualstudio.com/download)
- [K9s](https://github.com/derailed/k9s#installation)

#### Windows Notes

While Ansible supports managing Windows nodes, it can not run on Windows. If you want to to participate in the workshop from a windows machine, you'll need to use [WSL2](https://docs.microsoft.com/en-us/windows/wsl/install-win10). 

### Cluster Setup

Besides having the above mentioned tools installed, you'll also need access to a Kubernetes cluster, with the following features configured and working:

- Cluster networking
- Persistent volumes
- Ingress controller

For most users, running this locally for the workshop is the way to go. There are many ways of running Kubernetes locally, but the following methods have been verified to work for most users' environments.

- **Minikube**

  *Recommended for novice users*
  
  Minikube runs Kubernetes in a prebuilt virtual machine, completely isolated from your local environment. 
  It supports macOS, Linux and Windows, and works with many virtualization providers, of which VirtualBox is easiest to setup for most users.

  - [Install Minikube](https://minikube.sigs.k8s.io/docs/start/)
  - [Deploy Ingress Controller](https://kubernetes.io/docs/tasks/access-application-cluster/ingress-minikube/)

- **Docker Desktop**
  
  *Recommended for users already running Docker Desktop*  
  
  Docker Desktop also offers a Kubernetes integration which is very easy to use. You will need to deploy
  
  - [Install Docker Desktop](https://docs.docker.com/get-docker/) (Windows users should use the WSL2 implementation as mentioned above)
  - [Enable Kubernetes integration](https://docs.docker.com/desktop/kubernetes/)
  - Deploy Ingress Controller
    - [MacOS](https://kubernetes.github.io/ingress-nginx/deploy/#docker-for-mac)
      *These instructions should work just fine on windows as well, though they have not been tested*

#### Configuring `/etc/hosts`

As we'll be using hostname-based ingresses during the workshop, you'll need to update your `hosts` file.

> For most **Docker Desktop** users this step isn't required. If you are facing issues during the workshop, apply the following configuration, but replace `<minikube-ip>` with 127.0.0.1, or whatever is appropriate for your custom docker setup.

Add the following lines to your hosts file, replacing `<minikube-ip>` with the output of the `minikube ip` command. You can follow [these instructions on updating your hosts file on Windows, Linux and MacOS](https://www.howtogeek.com/howto/27350/beginner-geek-how-to-edit-your-hosts-file/) to do so.

```
<minikube-ip>	wordpress.localtest.me
<minikube-ip>	prometheus.localtest.me
<minikube-ip>	grafana.localtest.me
<minikube-ip>	alertmanager.localtest.me
<minikube-ip>	mattermost.localtest.me
<minikube-ip>	home.localtest.me
<minikube-ip>	dashboard.localtest.me
```


## FAQ

**`Error: failed to create resource: Internal error occurred: failed calling webhook \"validate.nginx.ingress.kubernetes.io\": an error on the server (\"\") has prevented the request from succeeding`**
The following should fix the issue on minikube: `kubectl delete -A ValidatingWebhookConfiguration ingress-nginx-admission`