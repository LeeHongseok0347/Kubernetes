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
    
## 중간 확인

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
    
    
# 마스터 노드 admin 구성

## sudo kubeadm init (root로 실행)
API, controller, scheduller, efcd, coreDNS를 구성해준다   
※마지막 두줄은 키로써 복사해둔다.   
※오류발생시 kubeadm reset으로 재설정   

## 메세지에 나온 명령어 3줄을 입력해 구성 완료
명령을 내려줄 사용자마다 해줘야함

    mkdir -p $HOME/.kube
    sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
    sudo chown $(id -u):$(id -g) $HOME/.kube/config
    
## pod네트워크(CNI)를 설치
노드들을 연결해주는 네트워크 설치   

위브넷

        kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"
