# 도커설치 

## 1. 먼저 요구하는 패키지 설치

    sudo apt-get install -y apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
    
## 도커 홈페이지 접속

[docs.docker.com](https://docs.docker.com/engine/install/ubuntu/)

### 구글 인증키 발급

    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

### 리포지토리 변경

    echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

### 도커 설치

     sudo apt-get update
     sudo apt-get install docker-ce docker-ce-cli containerd.io
     
     
 
