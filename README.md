# kubernetes-the-hard-way

## Installs

### Networking Setup

#### Virtual Private Cloud Network

```
gcloud compute networks create kubernetes-the-hard-way --subnet-mode custom
Created [https://www.googleapis.com/compute/v1/projects/ckad-237902/global/networks/kubernetes-the-hard-way].
NAME                     SUBNET_MODE  BGP_ROUTING_MODE  IPV4_RANGE  GATEWAY_IPV4
kubernetes-the-hard-way  CUSTOM       REGIONAL
```

#### Subnet

```
gcloud compute networks subnets create kubernetes --network kubernetes-the-hard-way --range 10.240.0.0/24
Created [https://www.googleapis.com/compute/v1/projects/ckad-237902/regions/us-west1/subnetworks/kubernetes].
NAME        REGION    NETWORK                  RANGE
kubernetes  us-west1  kubernetes-the-hard-way  10.240.0.0/24
```

#### Firewall

> For Internal Communication

```
gcloud compute firewall-rules create kubernetes-the-hardway-allow-internal --allow tcp,udp,icmp --network kubernetes-the-hard-way --source-ranges 10.240.0.0/24,10.200.0.0/16
Creating firewall...⠧Created [https://www.googleapis.com/compute/v1/projects/ckad-237902/global/firewalls/kubernetes-the-hardway-allow-internal].
Creating firewall...done.
NAME                                   NETWORK                  DIRECTION  PRIORITY  ALLOW         DENY  DISABLED
kubernetes-the-hardway-allow-internal  kubernetes-the-hard-way  INGRESS    1000      tcp,udp,icmp        False
```

> For external traffic

```
gcloud compute firewall-rules create kubernetes-the-hard-way-allow-internal --allow tcp:22,tcp:6443,icmp --network kubernetes-the-hard-way --source-ranges 0.0.0.0/0
Creating firewall...⠛Created [https://www.googleapis.com/compute/v1/projects/ckad-237902/global/firewalls/kubernetes-the-hard-way-allow-internal].
Creating firewall...done.
NAME                                    NETWORK                  DIRECTION  PRIORITY  ALLOW                 DENY  DISABLED
kubernetes-the-hard-way-allow-internal  kubernetes-the-hard-way  INGRESS    1000      tcp:22,tcp:6443,icmp        False
```

#### Public IP Address

```
gcloud compute addresses create kubernetes-the-hard-way --region $(gcloud config get-value compute/region)
Your active configuration is: [ckad-237902]
Created [https://www.googleapis.com/compute/v1/projects/ckad-237902/regions/us-west1/addresses/kubernetes-the-hard-way].
```

> Check the public-IP created above:
```
gcloud compute addresses list --filter="name=('kubernetes-the-hard-way')"
NAME                     ADDRESS/RANGE  TYPE  PURPOSE  NETWORK  REGION    SUBNET  STATUS
kubernetes-the-hard-way  35.197.57.XYZ                          us-west1          RESERVED
```

### Provision Compute Instances

#### Create Controller Nodes

[Create Controller Instances](./scripts/create-controller-instances.sh)

#### Create Worker Nodes

[Create Worker Instances](./scripts/create-worker-instances.sh)
