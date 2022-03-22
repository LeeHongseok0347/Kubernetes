# kubectl 명령어

    kubectl [command] [TYPE]     [NAME]       [flags]
             #명령    #자원타입   #자원이름     #옵션

--help로 모르는 명령어 확인 가능   
kubectl pods --help
   
kubectl 명령어의 약어 정보

    kubectl api -resources
    
정보 표출

    kubectl get [nodes,pods,deployments.app] [-o] [wide,yaml,json] [노드명,파드명,디플로이명]
    
노드,파드 등의 자세한 정보 표출

    kubectl describe node [노드,파드명,디플로이명]

pod 만들기

    kubectl run [파드명] [--image=프로그램명:버전] [--port]
    ex) kubectl run webserver --image=nginx:1.14 --port 80
    #버전은 기술하지않으면 최신버전이 된다. 또는 :latest
    
pod의 yaml 만들기

    kubectl run [파드명] [--image=프로그램명:버전] [--port] --dry-run # 실행가능한지만 확인해준다.
    kubectl run [파드명] [--image=프로그램명:버전] [--port] --dry-run -o yaml [> 파드명-pod.yaml] #yaml으로 생성해준다.
    
yaml로 파드 생성

    kubectl create -f [yaml파일명]
  
deployment 만들기

    kubectl create deployment [deployment] [--image=프로그램명:버전] [--replicas=갯수]
    ex) kubectl create deployment apache --image=httpd --replicas=3
    
deployment 확인하기

    kubectl get deployments.apps [디플로이명]
    
deployment 편집하기
: yaml형식으로 편집가능하며, vi에디터로 접속된다.
    kubectl ecit deployments.apps [디플로이명]
    
컨테이너에 접속하기

    kubectl exec [pod] [--flags]
    ex) kubectl exec apache -it -- /bin/bash
    #아파치 서버가 켜진 컨테이너에 bash쉘로 접속한다.
    
포트포워딩
: 마스터의 포트로 접속할때 컨테이너의 포트를 통해 컨테이너로 접속하도록 해주는 명령어

    kubectl port-forward [파드명] [마스터의포트 : 파드가실행되는컨테이너의 포트]
    ex) kubectl port-forward apache 8080:80
