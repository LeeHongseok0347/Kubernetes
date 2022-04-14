# 윈도우 2012R2

## 설치 
1.time and currency format : korean  
2.core : CUI / Desktop Evaluation : GUI  
3.pw : 6글자 이상 숫자와 기호 포함  
4.control pannel(제어판)에서 clock, language, region 에서 add language  
5.오른쪽의 option을 클릭하고 기다린 후 클릭해 설치  
6.primary laguage 버튼을 눌러 로그아웃후 로그인

## 서버관리자 초기 구성
서버관리자> 역할 및 기능추가 -> 기능 -> 닷넷프레임워크 3.5 설치

## 익스플로러 보안낮추기
서버관리자> 로컬서버-> 보안강화구성 -> 사용을 클릭해 사용안함으로 바꾼다

## administrator 패스워드 변경 					
net user USERNAME NEWPASS  

## 원격데스크톱 서비스 세션 수 증가			 	
윈도우로고 right click -> gpedit.msc  -> 관리템플릿 -> windows 구성요소 ->  
터미널서비스 -> 원격 데스크톱 세션 호스트 -> 연결 -> 연결개수 제한 사용 (TS 2개)  

## 호스트명 변경 : didim365test

    powershell>hostname
    호스트명
    powershell>Rename-Computer -ComputerName "호스트명" -NewName "새로룬호스트명"
    (재시작 후 적용)

사용자 추가 : didim365 / ektspt001##(터미널 접근 권한까지)
cmd > net USER 유저 비밀번호 /add

## Admin 그룹에 등록
net localgroup administrators [username] /add
## 원격 접속 그룹에 등록
net localgroup "Remote Desktop Users" [username] /add
## 현재 목록
net user

## 웹사이트 구축 : IIS (홈은 C:\ didim365\test)
### 바인딩 및 hosts파일
https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=tjrwjd3535&logNo=220739185665  

## 403 오류시 검색 가능 설정
https://docs.microsoft.com/ko-kr/troubleshoot/developer/webapps/iis/health-diagnostic-performance/http-403-14-forbidden-webpage  

## rewrite URL 설치
https://www.microsoft.com/web/downloads/platform.aspx  
다른 링크에서 설치시 404 오류와 응용프로그램 풀이 죽는다.  
IIS Rewrite 모듈 설치 : URL 재작성 모듈임  

## 도메인 바인딩 : test365.bgko.co.kr
사이트 생성시 도메인 바인딩을 걸어줄 수 있다.  

## 응용프로그램 풀 변경
사이트에서 -> 응용프로그램 ->응용프로그램 기본값 설정  

## 잘못된 자격 증명 오류 발생
인증 단에서 익명을 사용으로  

## 사용자 iis 연결
사이트 -> 기본설정 -> 연결계정  

## 방화벽 포트 열어주기
윈도우 제어판 -> 방화벽  -> 인바운드  

## 지정된사용자만 포트 접속
실행 -> wf.msc ->인바운드 규칙 ->만든 규칙에 속성-> 영역에 원격IP주소 추가  
## 파워쉘로 공인아이피(외부아이피)
(Invoke-WebRequest ifconfig.me/ip).Content.Trim()  

## 디렉터리 권한 변경
https://comeinsidebox.com/%ED%8A%B9%EC%A0%95-%ED%8C%8C%EC%9D%BC-%ED%8F%B4%EB%8D%94-%EC%82%AC%EC%9A%A9%EA%B6%8C%ED%95%9C-%EB%B6%80%EC%97%AC/  
powershell>icacls 디렉터리 /GRANT 유저명 didimuser:[R/W/X] (/T)  
※(T : 하위 파일들에도 적용)  

## 자세한 오류 보기
사이트 -> 오류페이지 -> 기능설정 편집 -> 자세한오류  

## 아이피 접근 제한
서버관리자> 도구 -> 역할 및 기능 추가 -> 서버역할 -> 웹서버 -> 보안 -> IP및 도메인 제한 체크 및 설치  
권한허용 : 사이트(우선재시작)>허용항목 추가  
나머지 주소 거부 : 기능설정편집 -> 지정되지않은 클라이언트~ : 거부  

## 로그 파일 경로 수정
사이트> 로깅 -> 디렉터리를 변경함   
※꼭 적용을 눌러줘야한다.(오른쪽 작업바)  

## ftp 사이트 구축
서버관리자> 도구 -> 기능 및 역할 추가 -> 서버역할 -> 웹서버 -> ftp서버 체크 및 설치  
사이트> 오른쪽클릭 -> FTP사이트 추가 -> 사용자 계정 및 포트 등록  
iis> 관리 서비스(없을시에 역할추가) -> 자격증명 -> windows 자격증명 또는 ~ -> 적용  
※사이트를 만들고 기본설정에서 연결계정 추가   
※연결된 계정은 사이트의 실제경로에 대한 권한이 필요함  

### 데이터 포트 변경 
iis> FTP 방화벽 지원 -> 데이터 채널 포트 범위  
※사이트가 아닌 시작페이지 및의 iis에서 해야한다  

## 방화벽 열어주기
방화벽 규칙-> FTP 서버 수동 사용안함  
규칙추가하여 방금 포트 범위 입력  
powershell> net stop ftpsvc(중지)  
powershell> net start ftpsvc(재시작)  

※접속시 530-valid hostname 오류는 다음을 참조  
https://studyforus.tistory.com/210  

## 원격 터미널 접속 포트 변경
실행 -> regedit -> [HKEY_LOCAL_MACHINE] - [SYSTEM] - [CurrentControlSet] - [Control] - [Terminal Server] - [WinStations] - [RDP-Tcp] ->portNumber 수정  
※파워쉘로 할 경우  

    Set-ItemProperty "HKLM:SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name PortNumber -Value 16011   
    Restart-Service termservice -Force(터미널 제시작)  

## 계정정책 (주로 비밀번호 정책)
실행> secpol.msc  

## 백업(robocopy)
robocopy 원본디렉토리 복사할디렉토리 /copyall  

## bat 파일 
변수지정 : set A=10 (연산시 set /a A=A+10)  
변수 표출 : echo %A%  
조건문 : if %A%==10 echo %ture%  
(if exist,if not, else등 사용 가능)  

      EQU - 같음  
      NEQ - 같지 않음  
      LSS - 보다 작은   
      LEQ - 작거나 같음  
      GTR - 보다 큰  
      GEQ - 크거나 같음  

## 작업 스케줄러
작업스캐줄러 -> 새 규칙에서 일단 매일로 추가-> 추가된 규칙 속성으로 -> 트리거에서 3시간으로 바꿈  

## Windows Server Backup
서버관리자 -> Windows Server 백업 기능 추가  

## 더미파일 만들기

    fsutil file createnew 파일명 크기(바이트단위)\  

## SMTP 서버 
기능 및 역할추가로 추가  
제어판 -> 관리도구에서 IIS(인터넷정보서비스) 6.0 관리자로 접속->ip,도메인,경로 입력 후 생성 완료  
생성된 smtp서버> 속성 ->엑세스 제어 인증에 익명인증,  windows 통합인증 체크 -> 보안탭에서 사용자 등록   

## ASP
기능 및 역할추가로 추가 후 (asp만 추가 3.5 4.5 추가시 에러 발생)  
사이트의 MIME 형식에 들어가서 .asp를 추가한 후 형식에는 text.html을 기입한다.  

## 라우팅 테이블 추가
powershell>route add 아이피 mask 마스크 게이트웨이 -p  
※-p : 재부팅시에도 사라지지 않게 영구적으로  

## 특정경로 exe파일 차단
서버관리자 > 기능및 역할 추가 -> 파일 및 저장소 -> 파일 서버 리소스 관리자 설치  
파일서버 리소스 관리자> 파일 차단관리 -> 파일차단만들기 -> 경로설정 -> 사용자지정 파일 차단 속성정의 -> *.exe 추가 -> 이벤트로그로 메세지 편집  
로컬정책>컴퓨터 구성> windows 설정 > 보안설정 > 소프트웨어 제한정책    
로컬정책>사용자구성> 관리템플릿 > 시스템 에서 지정된 응용프로그램 실행안함  
로컬정책>컴퓨터구성>windows설정>보안설정>응용프로그램 제어 정책>AppLocker (서비스에 들어가서 application Identity를 자동으로 하고 시작까지)  

## 레지스트리 바로적용
작업관리자>파일탐색기 재시작  
## 그룹정책 바로적용 
cmd>gpupdate /force  
