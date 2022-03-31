### 기본개념
<b>1. Server vs Client</b></br>
client는 web service를 사용하는 사용자, server는 사용자(Client)에게 web service를 사용할 수 있도록 서비스를 제공하는 자를 말함. </br>
현재 github에 글을 쓰고 있는 나는 github의 web service를 사용하는 client이고, github는 나에게 service를 제공하는 server인 셈임.</br>

<b>2. SSH (Secure Shell)</b> </br>
unix 계열의 운영체제를 원격에서 제어하기 위한 방법, ssh client를 이용해서 server와 멀리 떨어진 곳에서도 해당 server를 제어할 수 있다.</br>
Window에는 ssh client가 따로 설치되어 있진 않기 때문에 puTTY나 Xshell 같은 프로그램을 설치하여 ssh client로 이용한다.</br>
(* 과거에는 telnet을 사용했지만, 보안상의 문제 때문에 지금은 SSH를 주로 사용함. SSH는 암호화된 데이터를 주고 받기 때문에 telnet보다 보안상의 이점이 더 큼)</br>

<b>3. OS (operating system, 운영체제)의 주요 역할</b></br>
(1) 컴퓨터 프로그램을 사용자가 쉽게 이용할 수 있도록 환경을 만들어줌</br>
(2) 컴퓨터의 제한된 자원 (CPU, memory 등)을 효율적으로 할당하고 관리해줌</br>
(3) 입출력장치 (키보드, 마우스, 모니터) 등의 연산과 제어를 관리함</br>
(4) 프로그램의 오류나 컴퓨터 자원의 잘못된 사용을 감시</br>
(5) 주로 사용되는 OS로는 Window, Mac, Linux 등이 있음.</br>

<b>4. Unix vs Linux</b> </br>
Unix는 server 관리에 특화된 운영체제이며, Linux는 Unix에서 파생되어 만들어진 운영체제임.</br>
Linux는 Unix와 호환되면서도, 소스코드가 모두 공개되어있기 때문에 유지보수가 편하다는 장점이 있음.</br>

<b>5. GUI vs CLI vs TUI</b> </br>
(1) GUI (Graphical User Interface) : 프로그램의 동작을 그래픽으로 구현하여, 일반인들도 쉽게 컴퓨터를 조작할 수 있도록 한 interface. </br>
그래픽으로 구현했기 때문에 인간이 사용하기엔 편리하지만, 그만큼 컴퓨터의 자원을 더 잡아먹는다는 단점이 있음.</br>
(2) CLI (Command Line Interface) : 컴퓨터에 명령어를 입력해서 프로그램의 동작을 제어하는 방식의 interface. (ex. Window의 명령프롬프트)</br>
GUI에 비해 컴퓨터 자원을 덜 잡아먹으며, 명령 몇줄만으로 막대한 양의 데이터를 처리할 수 있어서 server 운영에 유리함</br>
(3) TUI (Text-based User Interface) : GUI처럼 그래픽을 제공하진 않지만, 마우스와 키보드를 모두 사용하며 프로그램과 상호작용할 수 있는 interface.</br>

<b>6. Web server 와 Was server</b> </br>
(1) Web server : 정적인 자료(html, css, image 등)를 처리하는 server (Apache가 대표적)</br>
(2) Was server : 동적인 자료(db연동, 비즈니스로직 등)를 처리하는 server (Tomcat이 대표적) </br>

앞으로 더 정리해야할 것들 : </br>
IP,라우팅,DNS, NAT/고정IP, 공인IP/사설IP, 포트포워딩</br>


### Linux의 계정
<b>1. 계정</b></br>
운영체제를 사용하는 사용자</br>

<b>2. 그룹</b></br>
여러개의 계정을 묶어 놓은 그룹</br>

<b>3. root 계정 & 그룹</b></br>
윈도우의 administrator에 해당, 관리자계정.</br>

<b>4. 명령어 모음</b></br>
$ adduser [계정이름] : 새로운 계정 생성</br>
$ deluser [계정이름] : 해당 계정을 삭제</br>
$ groupadd [그룹이름] : 새로운 그룹 생성</br>
$ groupdel [그룹이름] : 해당 그룹을 삭제</br>
$ addgroup [계정이름] [그룹이름] : 해당 계정을 그룹에 넣음</br>
$ delgroup [계정이름] [그룹이름] : 해당 계정을 그룹에서 뺌</br>
$ passwd [계정이름] : 해당 계정의 비밀번호를 변경 (현재 계정의 비밀번호를 변경하려면 그냥 passwd만 입력)</br>
$ id : 현재 계정에 대한 정보 확인</br>
$ logout : 로그아웃</br>
$ su - [계정이름] : root권한으로 해당계정에 접속 (그냥 su만 입력하면 root 계정으로 변경)</br>
$ sudo + 명령어이름 : 해당 명령어에만 root권한을 부여</br>

기타</br>
$ man + 명령어이름 : 해당 명령어에 대한 manual(설명)을 보여줌</br>

앞으로 더 정리해야할 것들 : </br>
 1) 가상 터미널(ssh,telnet)을 사용하기 위하여 서버, PC에서 사용되는 프로그램 및 설정에 대하여 설명하라
 2) TCP/IP기본 네트워크 (IP,PORT)에 대하여 설명하라.
 3) 고정IP, NAT, DHCP, 포트 포워딩 에 대하여 설명하라.
 4) VMware, virtualbox에 대하여 네트워크 설정에 대하여 설명하라.
