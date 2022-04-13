#명령어
여러줄 입력시에는 백틱을 넣어주면 된다. (입력종료는 엔터를 두번 친다)  

##디렉터리 변경(cd)
    
    cd "my file"        #디렉터리에 공백이 있을 경우
    cd C:\              #루트로 이동
    cd \                #루트로 이동
    
##디렉터리 생성(md)
    
    md "my file"       
    mkdir directory
    
#디렉터리 내용 보기

    dir d*              #와일드 카드 사용 가능


#더미 파일 만들기

    fsutil file createnew 파일명 사이즈(byte)
    
#IP 주소 변경

    Get-NetIPAddress | fl IPAddress, *Index         #바꿔야할 인덱스를 찾는다.
    New_NetIPAddress -InterfaceIndex 인덱스값 -IPAddress IP값 -PrefixLength 서브넷 비트수 -DefaultGateway 게이트웨이IP
    Set-DnsClientServerAddress -InterfaceIndex 인덱스값 -ServerAddress IP값
    Remove-NetIPAddress -IPAddress 주소               #삭제
    
#변수 저장,호출 방법

    $A = 1, 2, 3
    $B = "hello"
    $C = hostname       #명령결과 저장

#호스트네임 변경

    hostname    #호스트네임을 알아낸다.
    Rename-Computer -ComputerName 현재이름 -NewName 바꿀이름 `
    -DomainCredential domain\username -Force
    #재시작 후 적용된다.
    
