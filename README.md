# eks-kubectl-settings-for-cloud9
Enjoy EKS from AWS Cloud9!

## Summary
You'll be able to use...
- kubectl (with bash completion!)
- kubens (Useful!)
- kubectx (Useful!)

## Getting Started

### Before Install
```bash
export AWS_EKS_CLUSTER_NAME="YOUR-CLUSTER-NAME"
```

### Install Script
```bash
#!/bin/bash -eu

# --------------------------------------
# install aws-iam-authenticator
# --------------------------------------

curl -o aws-iam-authenticator https://amazon-eks.s3-us-west-2.amazonaws.com/1.10.3/2018-07-26/bin/linux/amd64/aws-iam-authenticator
chmod +x aws-iam-authenticator 
mv ./aws-iam-authenticator /usr/local/bin/
sudo mv ./aws-iam-authenticator /usr/local/bin/

# --------------------------------------
# kubectl
# --------------------------------------

curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo -S mv ./kubectl /usr/local/bin/kubectl

# --------------------------------------
# kubectl bash complition
# --------------------------------------

if type "kubectl" > /dev/null 2>&1
then
  source <(kubectl completion bash)
fi

# --------------------------------------
# kubectx & kubens & fzf
# --------------------------------------

sudo git clone https://github.com/ahmetb/kubectx /opt/kubectx
sudo ln -s /opt/kubectx/kubectx /usr/local/bin/kubectx
sudo ln -s /opt/kubectx/kubens /usr/local/bin/kubens
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
yes | ~/.fzf/install

# --------------------------------------
# Setting kubectl
# --------------------------------------

aws eks update-kubeconfig --name ${AWS_EKS_CLUSTER_NAME} 
```

### After Install
``` bash
source ~/.bashrc
```

Enjoy!
