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
    
### 오류발생시 kubelet의 동작 확인
      journalctl -xeu kubelet
#### 쿠버네티스의 cgroup과 도커의 cgroup 이름이 일치하지 않아서 발생하는 문제
불일치 여부 확인

      sudo docker info | grep -i cgroup
      # 출력
      Cgroup Driver: cgroupfs -> systemd 여야 함
      
불일치시에 변경 명령어

    cat <<EOF | sudo tee /etc/docker/daemon.json
    {
      "exec-opts": ["native.cgroupdriver=systemd"]
    }
    EOF

    service docker restart
    
    
## 방화벽 허용 시켜주기
웬만하면 이전단에 방화벽이 전부 구성되어 있다.
    
    systemctl stop firewalld && systemctl disable firewalld
    
## kubeadm, kubelet, kubectl 설치
  ### kubeadm : 큐브어드민, 쿠버네티스 운영 관리
  ### kubelet : 데몬, 컨테이너조작 및 통신(enable 시켜줘야함)
  ### kubectl : 쿠버네티스에 명령을 내릴때 사용

    sudo apt-get update
    sudo apt-get install -y apt-transport-https ca-certificates curl
    
    sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
    
    echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
    
    sudo apt-get update
    sudo apt-get install -y kubelet kubeadm kubectl
    sudo apt-mark hold kubelet kubeadm kubectl
