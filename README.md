# OpenEBS-setup
OpenEBS storage setup on a a k8s cluster

# k8s-automation-docs
Server Requirement
Resource	    Value
CPU	            4 vCPU
RAM	            8 GB
OS Disk	        40 GB
Data Disks	    2x 100 GB - N.B it should not be partitioned
Network	        1 Gbps

- Add the openebs repo using helm
helm repo add openebs https://openebs.github.io/charts
helm repo update

- Identify available disks on server
kubectl get blockdevice -n openebs
Example outputs
| NAME               | SIZE  | STATE     | NODE   |
| ------------------ | ----- | --------- | ------ |
| blockdevice-abc123 | 100Gi | Unclaimed | node-1 |
| blockdevice-def456 | 100Gi | Unclaimed | node-2 |
| blockdevice-ghi789 | 100Gi | Unclaimed | node-3 |
N.B: Make sure the disks you want to use are not mounted or used by OS.

- Create CRDs using the yaml files defined in the crds folder

- Create a CStorPoolCluster (CSPC) using the cstorpoolcluster.yaml file and apply 
N.B - Replace blockdevice-xxx and node-x with actual names from your cluster.

- Verify pool creation
kubectl get cstorpoolcluster -n openebs
kubectl get cstorpool -n openebs
kubectl get pods -n openebs

- Create cstorpool storageclass with the cstor-sc.yaml

Important - Install isciadmin on the application worker nodes
sudo yum install iscsi-initiator-utils
which iscsiadm
sudo systemctl status iscsid
sudo systemctl start iscsid
sudo systemctl enable iscsid
- Restart kubelet
systemctl restart kubelet
