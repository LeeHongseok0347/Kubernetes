# 쿠버네티스 설치

[kubernetis.io](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)

## worker_node 들의 swap영역을 비활성화

    swapoff -a && sed -i '/swap/s/^/#/' /etc/fstab
    
## iptable을 k8s.conf를 통해 bridge에 연결

    cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
    br_netfilter
    EOF

    cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
    net.bridge.bridge-nf-call-ip6tables = 1
    net.bridge.bridge-nf-call-iptables = 1
    EOF
    sudo sysctl --system
    
