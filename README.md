# kubernetes-IP-substitution


Working of the IP substitution script

## Centos-ETCD configuration changes:

CENTOS:
- IP in the network adapter files (em1, br0) is updated on each of the nodes

ETCD:
- ETCDs etcd.env is updated with the IP changes
- Updated the openssl.conf on each of the nodes to be used in ETCD specific certificates generation
- Generated ETCD specific certificates

Centos-ETCD configuration implementation: THESE CHANGES WERE ENFORCED PER EACH NODE CONSECUTIVELY
- Restarted the network system service to update the node IP
- Copied over previously generated certificates to the ETCD ssl directory
- Restarted the ETCD service so that the client_urls are updated
- Retrieved the ETCD member IDs to update the ETCD member peer_server address


## Kubernetes Configuration:
- Stopped the Kubelet/Docker systems service
- Replaced the IPs in all config-files of /etc/kubernetes
- Deleted the apiserver certificate along with its key so that an updated certificate could be generated using kubeadm
- Deleted the admin.conf from /etc/kubernetes only to generate it again using kubeadm
- Copy over the admin.conf to ~/.kube/config to update the kubectl config
- Started the Kubelet/Docker systems service
- Wait some time for the API-server to be functional
- Updated the kube-system configmaps with the latest IPs
- These steps should update the IPs of the cluster without losing any data
