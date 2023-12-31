# ISEC6000 Secure DevOps

## 

### Amazon EKS Cluster Setup

This guide provides instructions for setting up an Amazon Elastic Kubernetes Service (EKS) cluster on AWS using the AWS CLI and configuring kubectl to manage the cluster.

## Prerequisites

1. **AWS CLI**: Install and configure the AWS Command Line Interface (CLI) on your local machine.

   If you haven't installed AWS CLI yet, follow these steps to install it on your local computer:

   **On Windows**: Download and run the Windows installer, then follow the prompts for installation. Download link: https://aws.amazon.com/cli/

   **On macOS**: You can install AWS CLI using the Homebrew package manager by running the following command:

   ```
    install awscli
   ```

   **On Linux**: 

   ```
   # Install AWS CLI
   pip install awscli --upgrade --user
   
   # Configure AWS CLI with your credentials
   aws configure
   ```

   The crucial part of configuring AWS CLI is setting up your AWS access credentials, which will be used for authentication. You can configure AWS CLI with the following command:

   ```
   aws configure
   ```

   This will prompt you to enter the following information:

   - **AWS Access Key ID**: Your AWS access key ID.
   - **AWS Secret Access Key**: Your AWS secret access key.
   - **Default region name**: The default AWS region name. Enter the region where your EKS cluster is located (e.g., `us-west-2`).
   - **Default output format**: You can leave this as the default value.

   Ensure that the provided credentials have sufficient permissions to access your EKS cluster.

   **Verify AWS CLI Configuration**:

   Once the configuration is complete, you can run the following command to verify if AWS CLI is configured correctly:

   ```
   aws sts get-caller-identity
   ```

2. **kubectl**: Ensure that kubectl, the Kubernetes command-line tool, is installed on your local machine.

   ```
   # Install kubectl
   curl -LO "https://dl.k8s.io/release/$(curl -L -s  https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
   chmod +x kubectl
   mkdir -p ~/.local/bin
   mv ./kubectl ~/.local/bin/kubectl
   
   ```

## Steps

### 1. Create an Amazon EKS Cluster

Use the AWS CLI to create an Amazon EKS cluster with the following command:

```
aws eks create-cluster --name <CLUSTER_NAME> --role-arn <ROLE_ARN> --resources-vpc-config subnetIds=<SUBNET_ID1>,subnetIds=<SUBNET_ID2> --kubernetes-version <K8S_VERSION>
```

- `<CLUSTER_NAME>`: Choose a unique name for your cluster.
- `<ROLE_ARN>`: Replace with the ARN of an IAM role with necessary permissions.
- `<SUBNET_ID1>`, `<SUBNET_ID2>`: Replace with the IDs of the subnets where you want to deploy the cluster.
- `<K8S_VERSION>`: Choose the desired Kubernetes version.

### 2. Wait for Cluster to Become Active

Wait for the cluster to reach the `ACTIVE` state using the AWS CLI:

```
aws eks wait cluster-active --name <CLUSTER_NAME>
```

### 3. Configure kubectl(using AWS CLI)

Configure kubectl to communicate with your Amazon EKS cluster using the AWS CLI:

```
aws eks --region <REGION> update-kubeconfig --name <CLUSTER_NAME>
```

- `<REGION>`: The AWS region where your cluster is located.
- `<CLUSTER_NAME>`: Your cluster's name.

### 4.Configure kubectl(using local PC)

1. **Verify AWS CLI Configuration**:

   Once the configuration is complete, you can run the following command to verify if AWS CLI is configured correctly:

   ```
   aws sts get-caller-identity
   ```

### 4. Verify Connection

Ensure that `kubectl` is installed on your local machine and validate the connection to the Amazon EKS cluster:

```
kubectl get nodes
```

This will list the nodes in your cluster.

### 5. Deploy Applications

You can now use `kubectl` to create and manage Kubernetes deployments and services to deploy your applications on the Amazon EKS cluster.