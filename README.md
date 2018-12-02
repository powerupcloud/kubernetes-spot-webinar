<img src="puclogo.jpeg" align="right" />

# Run your Kubernetes Workloads on Amazon EC2 Spot Instances 

> Reference: https://aws.amazon.com/blogs/compute/run-your-kubernetes-workloads-on-amazon-ec2-spot-instances-with-amazon-eks/

## Provision EKS Worker Nodes
With Spot Instances, each instance type in each Availability Zone is a pool with its own Spot price based on the available capacity. A recommended best practice when working with Spot Instances is to use a diversified fleet of instances with multiple instance types, as created by Spot Fleet or EC2 Fleet. Unfortunately, Cluster Autoscaler does not support Spot Fleets at this time. You need a different strategy to provide diversification. Cluster Autoscaler for AWS provides integration with Auto Scaling groups.
For Cluster Autoscaler and other cluster administration and management pods that run on EKS worker nodes, create a small Auto Scaling group using On-Demand Instances. This ensures that the health of the cluster is not impacted by Spot interruptions.
The Cloudformation template deploys Auto Scaling groups dedicated to the following instance types:
- Spot Instances, m4.large, across three Availability Zones.
- Spot Instances, t2.medium, across three Availability Zones.
- On-Demand Instances, across three Availability Zones.
Create a Cloudformation stack in the AWS Console  to provision the EKS worker nodes. Make sure that you apply the aws-auth-cm.yaml file with the appropriate NodeInstanceRole value, as provisioned by the CloudFormation template. 

## Cluster Autoscaler
Cluster Autoscaler scales the worker nodes available for pods to be placed.  It automatically increases the size of an Auto Scaling group so that pods have a place to run. And it attempts to remove idle nodes, that is, nodes with no running pods.
Update the below variables in cluster-autoscaler-ds.yaml:
- Autoscaling Group Names of Ondemand and Spot Groups
- MIN Count of the Autoscaling Groups
- MAX Count of the Autoscaling Groups
- AWS Region
```
kubectl apply -f cluster-autoscaler-ds.yaml
```
A PDB limits the number of replicated pods that can be down at a given time. Create a PDB to ensure that you always have at least one Cluster Autoscaler pod running.
```
kubectl apply -f cluster-autoscaler-pdb.yaml
```

## Spot Interrupt Handler
The workflow of the Spot Interrupt Handler can be summarized as:
- Identify that a Spot Instance is being reclaimed.
- Use the 2-minute notification window to gracefully prepare the node for termination.
- Taint the node and cordon it off to prevent new pods from being placed.
- Drain connections on the running pods.
- To maintain desired capacity, replace the pods on remaining nodes.
```
kubectl apply -f spot-interrupt-handler.yaml
```
