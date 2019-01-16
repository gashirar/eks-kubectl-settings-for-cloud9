# eks-kubectl-settings-for-cloud9
Enjoy EKS from AWS Cloud9!

## Summary
You'll be able to use...
- kubectl (with bash completion!)
- kubens (Useful!)
- kubectx (Useful!)

## Getting Started


### Install Commands

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

```

``` bash
source ~/.bashrc
```

### Change IAM Settings
Cloud9 normally manages IAM credentials dynamically using STS.
So you will disable it and rely on the IAM role instead.

#### Change Cloud9 Settings
- Return to your workspace and click the sprokect, or launch a new tab to open the Preference tab.
- Select **AWS SETTINGS**
- Turn off **AWS managed temporary credentials**

#### Create IAM role and Attach role to Cloud9 EC2 instance

https://docs.aws.amazon.com/ja_jp/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html#attach-iam-role

#### Add an IAM role to an Amazon EKS cluster

Example:
```bash
# Please edit the object below. Lines beginning with a '#' will be ignored,
# and an empty file will abort the edit. If an error occurs while saving this file will be
# reopened with the relevant failures.
#
apiVersion: v1
data:
  mapRoles: |
    - rolearn: arn:aws:iam::555555555555:role/devel-worker-nodes-NodeInstanceRole-74RF4UBDUKL6
      username: system:node:{{EC2PrivateDNSName}}
      groups:
        - system:bootstrappers
        - system:nodes
    - rolearn: arn:aws:iam::012345678901:role/YOUR-ROLE-NAME
      username: system:node:{{EC2PrivateDNSName}}
      groups:
        - system:masters
  mapUsers: |
    - userarn: arn:aws:iam::555555555555:user/admin
      username: admin
      groups:
        - system:masters
    - userarn: arn:aws:iam::111122223333:user/ops-user
      username: ops-user
      groups:
        - system:masters
```

#### Create a kubeconfig for EKS
```bash
export AWS_EKS_CLUSTER_NAME="YOUR-CLUSTER-NAME"
aws eks update-kubeconfig --name ${AWS_EKS_CLUSTER_NAME}
```

Enjoy!
