# quick-eks
Code repository for standing up EKS cluster

### Installing awscli version 2
```
aws --version
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
which aws <note the path /usr/local/bin/aws>
./aws/install --bin-dir /usr/local/bin --install-dir /usr/local/bin/aws-cli --update
aws --version
```

### Installing kubectl to manage kubernetes cluster
```
curl "https://amazon-eks.s3.us-west-2.amazonaws.com/1.16.8/2020-04-16/bin/linux/amd64/kubectl" -o "kubectl"
chmod u+x kubectl
mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$PATH:$HOME/bin
kubectl version --short --client
```

### Installing eksctl for deploying kubernetes cluster
```
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /opt
mv /opt/eksctl /usr/bin/
eksctl version
eksctl help
```

### Creating kubernetes cluster using eksctl
<https://aws.amazon.com/premiumsupport/knowledge-center/eks-cluster-creation-errors/>
```
eksctl create cluster --name my-dev-cluster --version 1.16 --region us-east-1 --zones us-east-1a,us-east-1b --nodegroup-name standard-dev-workers --node-type t3.micro --nodes 3 --nodes-min 1 --nodes-max 4 --managed
eksctl get cluster
```

### Enable CloudWatch Logging for kubernetes cluster (Optional)
```
eksctl utils update-cluster-logging --region=us-east-1 --cluster=my-dev-cluster --enable-types all --approve
```

### Deploying service and nginx image (deploy service first)
```
aws eks update-kubeconfig --name my-dev-cluster --region us-east-1
git clone https://github.com/opstodevops/quick-eks
cd quick-eks/
kubectl apply -f ./nginx-service.yml
kubectl get service
kubectl apply -f ./nginx-deployment.yml
kubectl get deployments
kubectl get replicaset
kubectl get nodes
kubectl get svc
curl XYZ.us-east-1.elb.amazonaws.com
kubectl get pods --all-namespaces
kubectl get pods -n kube-system
```

### Wide output
```
kubectl get pods -o wide
kubectl get nodes -o wide
kubectl get deployments -o wide
```

