# aws-eks-demo

Note: 
At the moment, EKS is only available in the following regions:
- EU (Stockholm)
- Asia Pacific (Mumbai)
- EU (Paris)
- EU (London)
- EU (Ireland)
- Asia Pacific (Seoul)
- Asia Pacific (Tokyo)
- Asia Pacific (Singapore)
- Asia Pacific (Sydney)
- EU (Frankfurt)
- US East (N. Virginia)
- US East (Ohio)
- US West (Oregon)

At the moment, Cloud9 is only available in the following regions:
- EU (Ireland)
- Asia Pacific (Singapore)
- EU (Frankfurt)
- Asia Pacific (Tokyo)
- US East (N. Virginia)
- US East (Ohio)
- US West (Oregon)

# SECTION I -- Client and EKS Cluster setup

## 1. AWS Environment setup
 - show the demo app that will be deployed to AWS
 - AWS user/roles setup 
 - Cloud9 setup
   * Why Cloud9?, it has aws cli, golang, git 
   * create workspace
   * create an IAM role (eksworkshop-admin, assig AdministratorAcccess) for your workspace
   * attach the IAM role for your workspace 
      ** find EC2 of cloud9
 - install kubernetes tools: kubectl
   1. Create the default ~/.kube directory for storing kubectl configuration 
      ```
	  mkdir -p ~/.kube
	  ```
   2. Install kubectl
      ```
	  sudo curl --silent --location -o /usr/local/bin/kubectl https://amazon-eks.s3-us-west-2.amazonaws.com/1.12.7/2019-03-27/bin/linux/amd64/kubectl
      sudo chmod +x /usr/local/bin/kubectl
      kubectl version
	  ```
   3. Install AWS IAM Authenticator
      ```
	  go get -u -v github.com/kubernetes-sigs/aws-iam-authenticator/cmd/aws-iam-authenticator
      sudo mv ~/go/bin/aws-iam-authenticator /usr/local/bin/aws-iam-authenticator
	  ```
   4. Install JQ and envsubst
      ```
      sudo yum -y install jq gettext
	  ```
   5. Verify the binaries are in the path and executable
  
  ```
  for command in kubectl aws-iam-authenticator jq envsubst
  do
    which $command &>/dev/null && echo "$command in path" || echo "$command NOT FOUND"
  done
  ```
  
  - UPDATE IAM SETTINGS FOR YOUR WORKSPACE
   Cloud9 normally manages IAM credentials dynamically. This isn’t currently compatible with the aws-iam-authenticator plugin, 
   so we will disable it and rely on the IAM role instead.
   1. Return to your workspace and click the sprocket, or launch a new tab to open the Preferences tab
   2. Select AWS SETTINGS
   3. Turn off AWS managed temporary credentials
   4. Close the Preferences tab


  - Configure AWS credentials in terminal
   To ensure temporary credentials aren’t already in place we will also remove any existing credentials file:
   rm -vf ${HOME}/.aws/credentials
 
   We should configure our aws cli with our current region as default:
 
   ```
   export ACCOUNT_ID=$(aws sts get-caller-identity --output text --query Account)
   export AWS_REGION=$(curl -s 169.254.169.254/latest/dynamic/instance-identity/document | jq -r '.region')

   echo "export ACCOUNT_ID=${ACCOUNT_ID}" >> ~/.bash_profile
   echo "export AWS_REGION=${AWS_REGION}" >> ~/.bash_profile

   cat ~/.bash_profile

   aws configure set default.region ${AWS_REGION}
   aws configure get default.region
  ```
   
   - Validate the IAM role
   ```
   aws sts get-caller-identity
   ```
   * Output should contain the following
    eksworkshop-admin

 - CREATE AN SSH KEY
  1. generate SSH Key in Cloud9
   ```
   ssh-keygen
   ```
  2. Upload the public key to your EC2 region:
   ```
   aws ec2 import-key-pair --key-name "eksworkshop" --public-key-material file://~/.ssh/id_rsa.pub
   ```
   
  - download eksctl
   ```
   curl --silent --location "https://github.com/weaveworks/eksctl/releases/download/latest_release/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
   sudo mv -v /tmp/eksctl /usr/local/bin
   eksctl version
   ```


## 2. Launch EKSCTL

  - create the cluster (takes 15 mins)
  ```
  eksctl create cluster --region ${AWS_REGION} --name eks-workshop-eksctl --nodes=3
  ```
  
  - verify
  ```
  eksctl get clusters
  ```
  
  - scale
  ```
  eksctl scale nodegroup --name eks-workshop-eksctl  --nodes=4 --region=${AWS_REGION}
  ```



### Test the Cluster
- List the worker nodes
```
kubectl get nodes -o wide
```

- Export the Worker Role Name for use throughout the workshop

```
INSTANCE_PROFILE_NAME=$(aws iam list-instance-profiles | jq -r '.InstanceProfiles[].InstanceProfileName' | grep nodegroup)

INSTANCE_PROFILE_ARN=$(aws iam get-instance-profile --instance-profile-name $INSTANCE_PROFILE_NAME | jq -r '.InstanceProfile.Arn')
ROLE_NAME=$(aws iam get-instance-profile --instance-profile-name $INSTANCE_PROFILE_NAME | jq -r '.InstanceProfile.Roles[] | .RoleName')

echo "export ROLE_NAME=${ROLE_NAME}" >> ~/.bash_profile
echo "export INSTANCE_PROFILE_ARN=${INSTANCE_PROFILE_ARN}" >> ~/.bash_profile

- list content of  .bash_profile
cat ~/.bash_profile

- Get version of client and server
kubectl version
```
