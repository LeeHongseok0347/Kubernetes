# 윈도우 2012R2

## 설치 
1.time and currency format : korean  
2.core : CUI / Desktop Evaluation : GUI  
3.pw : 6글자 이상 숫자와 기호 포함  
4.control pannel(제어판)에서 clock, language, region 에서 add language  
5.오른쪽의 option을 클릭하고 기다린 후 클릭해 설치  
6.primary laguage 버튼을 눌러 로그아웃후 로그인
    
## iptable을 k8s.conf를 통해 bridge에 연결

    cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
    br_netfilter
    EOF

    cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
    net.bridge.bridge-nf-call-ip6tables = 1
    net.bridge.bridge-nf-call-iptables = 1
    EOF
    sudo sysctl --system
    
