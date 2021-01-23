# k8s-cluster
A CloudFormation script to create a 3 Node Kubernetes Cluster based on Sander Van Vugt Method.

# Pre-requisite:

1. Install and configure AWS CLI
2. Set correct region for AWS CLI
        export AWS_REGION="$@" 
        export AWS_DEFAULT_REGION="$@" 

# Create Master (Control Plane) and Worker (Data Plane)

    $ aws cloudformation create-stack --capabilities CAPABILITY_AUTO_EXPAND CAPABILITY_NAMED_IAM --template-body file://master.yml  --stack-name KubernetesMaster --parameters ParameterKey=KeyName,ParameterValue=<key_name>

# Join Node

1. Obtain SSH command to connect to Master:
        $ aws cloudformation describe-stacks --query "Stacks[*].Outputs[?OutputKey=='MasterSSH'].OutputValue" --output text
2. Login to master using the above SSH command
3. Run command:
        $ sudo -s
        $ export KUBECONFIG=/etc/kubernetes/admin.conf
        $ kubeadm token create --print-join-command
4. Login to Worker using the above SSH command
        $ aws cloudformation describe-stacks --query "Stacks[*].Outputs[?OutputKey=='MasterSSH'].OutputValue" --output text
5. Run the following commands:
        $ sudo -s
        $ (kubeadm join... obtained in Step#3)
