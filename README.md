# CS_STUDY

# 1. 운영체제 
 - 프로세스와 스레드의 차이
  프로그램: 어떤 작업을 위해 실행할 수 있는 파일
  프로세스: 연속적으로 실행되고 있는 컴퓨터 프로그램
  프로세서: 내부에서 수행하는 하드웨어의 유닛단위(서 랑 스 랑 구별 잘할것)
   프로세스의 상태 
    -1. 생성: 프로세스가 생성되는 중
    -2. 실행: 프로세스가 CPU를 점유하여 명령어들이 실행되고 있는 중
    -3. 준비: 실행되진 않지만 언제든 사용가능하도록 CPU 할당을 대기하고 있는 단계
    -4. 대기: 입출력 완료 등 특정 시그널을 기다리고 있는 상태 
    -5. 종료: 프로세스 실행이 종료됨
    
   스레드: 프로세스가 실행되는 여러 흐름의 단위, 프로세스의 특정한 수행 경로
     - STACK만 따로 할당 받고 코드, 데이터. 힙 영역은 모든 스레드가 공유한다. 스택만을 독립적으로 가지는 것은 스택이 호출시 전달되는 인자, 돌아갈 주소값, 함수 내 지역 변수등을 저장되기 위해 사용되는 메모리 공간이기에 독립적인 함수호출이 가능하다는 것이고 이는 독립적인 실행흐름을 추가하고자 하는 스레드의 목적의 최소한의 요구사항이기도 하다. 

사용자 수준 스레드의 장점
 - 이식성이 높다: 커널에 독립적으로 스케쥴링이 가능하여 모든 운영체제에 적용이 가능하다. 
 - 오버헤드가 적다: 스케쥴링이나 동기화를 위해 커널 호출을 하지 않기에 커널 영역으로 전환하는 오버헤드가 줄어든다. 
 - 유연한 스케쥴링의 가능: 커널이 아닌 스레드 라이브러리에서 스레드 스케줄링을 제어하기에 응용 프로그램에 맞게 스케줄링 할 수 있다. 

사용자 수준 스레드의 단점: 시스템의 동시성을 지원하지 않는다: 스레드가 아닌 프로세스 단위로 프로세서를 할당하여 다중 처리환경을 갖춰도 스레드 단위로 다중 처리를 하지 못한다. 동일한 프로세스의 스레드 한개가 대기상태가 되면 이 중 어떤 스레드도 실행하지 못한다.

 - 확장에 제약이 따름: 커널이 한 프로세스에 속한 여러 스레드에 프로세서를 동시에 할당할 수 없어 다중 처리 시스템에서 규모를 확장하기가 어렵다. 
 - 스레드간 보호 불가느이 스레드 간 보호에 커널의 보호방법을 사용할 수 없어 스레드 라이브러리에서 보호를 제공해야 하며 프로세스 수준에서 보호가 가능하다. 
 
 - 커널 수준 스레드와 유저 수준 스레드
  커널 수준 스레드: 커널 수준 스레드는 커널 레벨에서 생성되는 스레드이다. 운영체제 시스템 내에서 생성되어 동작하는 스레드로, 커널이 직접 관리한다. 또한 커널 수준 스레드는 사용자 수준 스레드와 1:1 관계이기 때문에 하나의 프로세스의 여러 스레드들을 병행으로 수행이 가능하다. 단 유저 모드에서 커널 모드로의 전환이 빈번하게 이뤄져 성능 저하가 발생한다. 
  
  사용자 수준 스레드: 사용자 라이브러리를 통해 생성되고 관리된다. 커널이 사용자 수준 스레드의 존재를 알지 못하기 때문에 스레드 교환에 커널이 개입하지 않는다. 커널은 스레드가 아닌 프로세스를 한 단위로 인식하고 다량의 사용자 수준 스레드가 하나의 커널 수준 스레드에 할당되기에 다대일 스레드 매핑이다. 
     
#스레드를 사용할 경우 충돌을 막기 위해 동기화를 해야하는데 이때 교착 상태에 유념해야 한다. 

 - 멀티 프로세스와 멀티 스레드 
  1. 멀티 프로세스: 하나의 응용프로그램을 여러 프로세스로 구성하여 하나의 프로세스가 하나의 작업을 맡게 하는 것. 
   -> 프로세스들은 독립된 메모리를 사용하기 때문에 문맥 교환 과정에서 캐쉬 메모리 초기화 등 무거운 작업이 진행되고 시간이 소요되며 오버헤드가 발생 
  2. 멀티 스레드: 하나의 응용프로그램을 여러개의 스레드로 구성하고 각 스레드가 하나의 작업을 맡게 하는 것, 웹 서버가 대표적인 멀티 스레드 응용프로그램 (node.js 는 싱글 스레드)
   -> 공유하는 메모리가 많아 문맥 교환 비용이 멀티 프로세스에 비해 저렴하다. 단 공유하는 메모리가 많아 하나의 쓰레드에 문제가 발생하면 전체 프로세스가 영향을 받는다. 
   
- 웹 서버에서 멀티 프로세스와 멀티 스레드 차이
 멀티 프로세스: 클라이언트가 연결 요청을 하면 서버 프로그램은 자신을 복제하여 클라이언트에 대응하게 되고 자신은 다른 클라이언트의 요청을 기다리게 됨 -> 원본 프로세스의 메모리를 모두 복제하기 때문에 자원낭비가 심하다. 

 멀티 스레드: 클라이언트 요청을 처리하는 프로세스가 아니라 스레드를 생성하여 클라이언트의 요청을 처리하도록 함. 
   
   - 멀티 프로세스 대신 멀티 스레드를 사용하는 이유 
    프로그램을 여러개 키는 것 보다 하나의 프로그램 안에서 여러 작업을 해결하는 것이 당연히 효율적이다. 문맥교환시 스레드는 스택 영역만 처리하게 되기 때문. 
    
- 경쟁 상태란 무엇인가?: 둘 이상의 입력이나 조작이 동시에 일어나 의도하지 않은 결과를 가져오는 경우. 공유자원을 접근하는 하나 또는 그 이상의 프로세스나 스레드들의 다중 접근이 올바르게 제어도지 않아 원하지 않는 결과를 불러오는 상태를 말한다. 
-      
- 교착상태란 무엇이며 교착 상태가 발생하기 위해서는 어떤 조건이 있어야 하나요? 
 교착상태(dead-lock): 서로 상대방의 작업이 끝나기 만을 기다리고 있기 때문에 결과적으로 아무것도 완료되지 못하는 상태를 말한다. 
  발생조건 4가지 모두가 충족되야 
   1. 상호배체: 한번에 한개의 프로세스만이 공유 자원을 사용할 수 있어야 한다.
   2. 점유와 대기: 최소한 하나의 자원을 점유하고 있으면서 다른 프로세스에 할당되어 사용되고 있는 자원을 추가로 점유하기 위해 대기하는 프로세스가 있어야 한다.
   3. 비선점: 다른 프로세스에 할당된 자원은 사용이 끝날 때까지 강제로 빼앗을 수 없어야 한다. 
   4. 순환 대기: 프로세스의 집합에서 n-1 은 n이 점유한 자원을 대기하여야 한다. 

 - 교착상태 해결법 -> 예방 회피, 탐지 및 회복 이렇게 크게 3가지가 존재

1. 예방 
 - 자원의 상호배제 조건 방지: 상호배제는 자원의 비 공유가 전제되어야 한다. 공유자원은 배타적 접근이 필요없어 교착 상태가 발생하지 않는다. 
 - 점유와 대기 조건 방지: 방지하기 위해선 프로세스가 작업을 수행하기 전에 필요한 자원을 모두 요청하고 획득해야 한다. 이를 최대 자원 할당 이라고 하는데 이는 자원 효율성이 낮고 기아 상태를 발생할 수 있다는 단점이 존재한다.  
 - 비선점 조건 방지: 이미 할당된 자원에 선점권이 없어야 한다. 하지만 비선점 조건을 사용한다면 어떤 프로세스든 중간에 자원을 사용할 수 있어서 기존에 사용중이던 프로세스는 작업 내용을 잃을 수 있다. 
 - 순환 대기 조건 방지: 모든 자원에 일련의 순서를 부여하고 각 프로세스가 오름차순으로만 자원을 요청할 수 있게 한다. 하지만 작업에 필요한 자원은 훨씬 오래 전부터 자원을 할당받은 상태가 되야 하므로 자원의 낭비가 있을 수 있을 수 있다. 

2. 회피
 - 프로세스 시작 중단, 은행가 알고리즘 등이 존재

동기화 문제의 해결법 
 - 유저 모드 동기화 : CRITICAL SECTION
 - 커널 모드 동기화: semaphore, mutex 
   CRITICAL SECTION: 한 프로세스 내의 스레드 사이에서만 동기화가 가능하다. 
   mutex: 여러 프로세스의 스레드 사이에서도 동기화가 가능하다. (커널 객체이기 때문이다.), 뮤텍스는 자원을 점유한 프로세스나 스레드가 잠시 소유하였다가 작업이 끝나는 반환하는 개념. 
   세마포어: 뮤텍스는 동기화 시 하나의 스레드만 실행되게 하지만 세마포어는 지정된 수만큼의 스레드가 동시에 실행되도록 동기화 하는 것이 가능함., 자원의 상태를 나타내는 일종의 변수로서 소유개념이 아님
   
   
컨텍스트 스위칭
 : 멀티 프로세스 환경에서 cpu가 하나의 프로세스를 실행하고 있는 상태에서 인터럽트 요청에 의해 다음 우선 순위의 프로세스가 실행되어야 할 때 기존의 프로세스의 상태 또는 레지스터 값을 저장하고 cpu가 다음 프로세스를 수행하도록 새로운 프로세스의 상태 또는 레지스터 값을 교체하는 작업
 
동기 비동기 블로킹 논블로킹
 - 블로킹과 논블로킹: 
 

 
# 2. 웹

7계층
1. 물리계층: 하드웨서 전송 기술로 이루어져 있다. (허브, 리피터, 케이블 등등)
2. 데이터 링크 계층: 신뢰성 있는 전송을 보장하기 위한 계층 (스위치, 브릿지, mac 주소가 여기)
3. 네트워크 계층: ip주소를 제공하는 계층. (라우터, ip공유기 등등) 전송단위가 패킷임
4. 전송 계층: 양 끝단 사용자들이 데이터를 주고 받게 하는 계층(tcp, upd), 전송 단위가 세그먼트  
5. 세션 계층: 여기부터 데이터를 만들어내는 계층, 양 끝단의 응용프로세스가 통신을 관리하기 위한 방법을 제공. 이 계층은 tcp/ip 세션을 만들고 없애는 책임을 진다. (소켓)
6. 표현 계층: 코드간의 번역을 담당하여 사용자 시스템에서 데이터의 형식상 차이를 다루는 부담을 응용계층으로부터 덜어준다. mime 인코딩이나 암호화 등의 동작이 이 계층에서 이루어짐. 압축이나 인코딩 등이 여기에서 다루어진다.
7. 응용계층: 응용프로세스와 직접관계하여 일반적인 응용서비스를 수행한다. 우리가 사용하는 사용자 인터페이스를 제공하는 프로그램이 여기속함. 우리가 잘 알고 있느 http, ftp 등의 프로토콜이 응용 계층에 속함(dns, http, ftp, dhcp)
 
 dhcp, arp 
 
 dhcp: 호스트의 tcp/ip 설정을 클라이언트에 자동으로 제공하는 프로토콜임. dhcp 서버에서 사용자 자신의 ip주소, 가장 가까운 라우터의 ip주소, 가장 가까운 dns 서버의 ip주소를 받는다. 
 arp: 가장 가까운 라우터의 mac주소를 알아낸다. 
dhcp 와 arp 를 사용하여 외부 통신 준비를 마친 후에 dns 쿼리를 dns서버에 전송하고 웹 서버의 주소 ip를 받아온다. 이후 http request를 위해 tcp소켓을 개방하고 연결한다.
이 과정에서 3 hand shaking을 통해 tcp를 연결하는 것이다. 

웹 렌더링

1. dom. cssom 생성: 서버로부터 받은 html, css를 다운받는다. 이들은 단순한 텍스트이므로 연산과 관리가 유용하도록 오브젝트 모델로 만들게 된다. (dom tree, cssom)
2. render tree: 이 두 트리를 활용하여 렌더 트리를 생성하게 된다. 순수하게 요소들의 구조와 텍스트만 존재하는 트리들과 달리 렌더 트리에는 스타일 정보가 설정되어 있으며 실제 화면에 표현되는 노드들로만 구성된다. 
3. 레이아웃 단계: 브라우저의 뷰포트 내에서 각 노드들의 정확한 위치와 크기를 계산한다. 풀어서 얘기하자면 생성된 렌더 트리 노드들이 가지고 있는 스타일과 속성에 따라서 브라우저 화면의 어느위치에 어느 크기로 출력될지 계산하는 단계하고 할 수 있다. 
4. 패인트: 레이아웃 단계 종료 후 이제 요소들을 실제 화면에 그리게 된다. 이미 요소들의 크기와 위치가 계산된 렌더 트리를 활용해 실제 픽셀값을 채워넣게 되는 단계

렌더링 최적화 - reflow, repaint 최소화
 reflow: 레이아웃 과정이 끝난 후에 액션이나 이벤트에 따라 html요소의 크기나 위치 등 레이아웃 수치를 수정하면 그에 영향을 받는 자식, 부모 노드들을 포함하여 레이아웃 과정을 다시 수행하게 된다. 
 repaint: 페인트를 또 하는 단계 
 
 즉 사용하지 않는 노드에는 visability: invisible보다 display: none을 사용한다. 인비지블은 레이아웃 공간을 차지하여 리플로우 대상이 되지만 논 디스플레이는 아예 렌더 트리에서 제외되기 때문
 또한 리페인트, 리플로우가 발생하는 css속성을 가급적 피해야 한다. 프레
tcp 헤더
 -> 160비트의 크기임. 각 필드 비트를 0과 1로 표현하여 전송하고자 하는 세그먼트의 정보를 나타낸다. 기본이 20바이트고 최대 60바이트 까지 커질 수 있다. 
tcp 연결과정. 
1. 사용자가 네이버 를 입력한다. 
2. 지정된  dns 서버로 해당 dns쿼리를 송신한다. 
3. 로컬 네임서버는 root 네임 서버에 www.naver.com 을 질의하고 
4. root 네임서버는 .com 네임서버의 ip 주소를 알려준다.  
5. 로컬 네임서버는 루트에게 받은  ip주소로 다시 .com 네임서버에 질의한다. 
6. .com 네임서버는 구글의 네임서버 ip를 전달해준다. 
7. 로컬네임서버는 받은 구글 네임서버에 쿼리를 질의한다.
8. 구글 네임서버는 구글 닷 컴 에대한 ip주소를 응답해준다. 그리고 ttl시간동안 자신의 캐쉬에 ip를 저장해준다. 
9. 로컬은 받은 ip주소를 클라이언트로 응답해준다. 
10. 클라이언트는 응답받은 ip정보로 패킷을 생성하기 시작하는데 맨 먼저 애플리케이션 계층에서 전송할 데이터를 생성한다.   그리고 시스템 콜을 호출하여 데이티를 아래 계층으로 전송한다. 
11. 그 후 각 계층을 지나며 각자 자기 계층의 헤더를 점점 더 덧 붙인다. 
12. Write 시스템 콜을 호출하면 유저 영역의 데이터가 커널 메모리로 복사되고, send socket buffer의 뒷부분에 추가된다. 순서대로 전송하기 위해서다. 그림에서 옅은 회식 상자가 이미 socket buffer에 존재하는 데이터를 의미한다. 이 다음으로 TCP를 호출한다.

http란? : html문서를 주고받기 위한 tcp/ip기반의 프로토콜. 기본적으로 요청/응답 구조로 이루어져 있으며 상태를 저장하지 않는 stateless구조이다.  

http 헤더 구조 (1.1)
: 크게 3부분으로 나눠져있다. 
 - 일반 헤더
 - 요청/응답 헤더
 - 엔터티 헤더
 
 일반헤더: 요청 응답이 생성된 시간이 기록됨, 날짜, 시간등도 여기 기록됨 요청과 응답메세지에 공통으로 사용되는 헤더. 
 요청/응답 헤더: 서버에 요청할땐 요청헤더고, 응답할때는 응답헤더를 사용한다.  여기에는 서버나 브라우저의 정보등이 담겨있다.
 엔터티 헤더: 본문 길이, 본문 언어 등의 본문과 관련된 내용들이 담겨있다. 
 https? -> http +ssl(보안) 공개키 암호화 방식을 사용하여 보안을 강화한 방식 
 사용자는 서버의 공개된 키로 요청을 암호화 하여 html요청을 보내고 서버는 그에 맞는 응답을 자신의 비밀키로 암호화 하여 보낸다. 
 
 공개키 암호화 방식
  비밀키로 암호화한건 공개키로만 복호화가 가능하고 공개키로 암호화한 것은 비밀키로만 복호화가 가능한 구조 
  공개키를 얻는 방식은 ca라 불리는 공개키 저장회사에 돈주고 자신의 공개키를 암호화해달라 부탁하면 된다. 

http 1.0 1.1 2.0

1.0 vs 1.1 : TCP Connection당 하나의 URL만 fetch하며, 매번 request/response가 끝나면 연결이 끊기므로 필요할 때마다 다시 연결해야하는 단점이 있어 속도가 현저히 느리다. HTTP 1.0에서는 open/close를 위한 flow의 제한으로 대역폭이 적게 할당되어 연결되는데, 이로 인해 congestion information이 자주 발생하고 disconnect가 반복적으로 나타나게 된다.
반복되는 disconnect 현상으로 인해 한 서버에 계속해서 접속을 시도하게 되면 과부하가 걸리고 성능이 떨어지게 되는 문제가 발생한다.

2.0 -> 1.1에서 계속 중복되는 헤더들을 감소시켰다. 1.1에서는 keep alive옵션을 통해 데이터 파이프라이닝이 아닌이상 기존 데이터 전송이 끝나야 다음 데이터를 받는 구조였는데 2.0 은 병렬로 동시에 여러 데이터를 받을 수 있다. 구글의 spdy 프로토콜을 사용했다고 한다. 

keep alive 옵션: 이미 연결되어있는tcp연결을 살려서 계속 통신을 하는 방법 즉 추가적인 요청을 위해 핸드세이크 과정이 생략되기 떄문에 효율적이다. 
단 성능향상을 보려면 적은 접속이 유지되어야 함. keep alive time을 통해 연결 지속 시간을 결정하는데 다량의 접속자가 발생하면 연결이 유지된체로 너무 많은 연결이 생성되기 때문에 서버 성능자체가 저하될 수도 있다. 

http 4대 통신 용어

get: 서버에게 리소스를 보내달라고 단순 요청만을 보낸다. 
post: 서버에게 리소스를 보내면서 새로 생성해달라고 요청한다
put: 서버에게 보낸 리소스로 수정해달라고 요청한다 단 없을 시 새로 생성도 해준다.
delete: 삭제요청

쿠키와 세션
http는 상태저장을 하지 않는 프로토콜이기에 (stateless) 연결이 끊기면 상태정보가 다 날라가서 이를 보완하기위해 쿠기와 세션이라는 것을 사용하게 되었다. 

쿠키: 로컬(클라이언트)에 저장되어 키와 값이 저장되는 작은 데이터 파일, 이름 만료시각 등의 데이터가 포함, 예시로는 로그인 상태 유지에 사용된다. 
  지속쿠키: 브라우저가 종료되도 남아있는 쿠키, 만료날짜를 안정해주면 이걸로 저장된다.
  세션쿠키: 브라우저 종료시 날라감, 브라우저의 메모리에 저장되고 파일로 저장되는 지속쿠키보다 보안에있어서 더 안전하다. 
세션: 서버에 클라이언트의 상태정보를 저장하는 기술로 논리적인 연결을 세션이라고 한다. 서버는 클라이언트 마다 구별가능한 id 를 부여하는데 이를 세션id라고 부른다. 

cors : 교차 출처 리소스 공유 
-> 먼저 sop는 같은 출처에서만 리소스를 공유할 수 있다는 정책이다. 이를 예외로 하는 것이 cors인데 출처가 다른 리소스 요청을 하기 위해선 sop의 예외조항인 cors 정책을 지킨 리소스 요청을 해야 하는 것이다. http 요청헤더에 출처가 기록되는데 이와 다르면 안됨 

cors 해결법
1. 엑세스 컨트롤 얼로우 오리진-> 이걸 * 으로 해두면 모든 출처에서 오는 요청을 받아먹겠다는 것이기 때문에 보안상 위험하다.
2. Webpack Dev Server로 리버스 프록싱하기: Webpack Dev Server라는 라이브러리를 이용하면 프록싱을 사용해 cors 정책을 우회 가능하다. 

Representational State Transfer - rest api 의 풀네임
: 자원 행위 표현 이렇게 3개로 구성되어있다. 즉 http method (get put post delete) 를 통해 자원에 대해서 crud operation(create, read, update, delete)를 적용하는 것이다. 

자원: 서버에 존재하는 자원으로 각각 고유한 id 를 가지고 있다. 클라이언트는 url을 통해 해당자원에 대한 연산을 요청가능하다. id는 url로 구분 가능하다.(id = url)
행위: http method로 crud연산을 요청할 수 있다. 
표현: 서버는 요청받은 행위에 대하여 적절한 응답을 보내준다. 

rest api 의 특징
1. 유니폼 인터페이스: 클라이언트의 자원 조작 행위를 통일되고 한정된 인터페이스로 한정시키는 것
2. 무상태성(stateless): 상태정보를 별도로 저장하지 않고 서버는 들어오는 요청을 단순히 처리하기만 한다. 즉 서버 구현이 좀 더 편해짐 
3. cachable (캐시 가능): 가장 큰 특징 중 하나로 http를 그대로 사용하기 때문에 http가 가진 캐싱 기능을 사용가능하다. 
4. client - server 구조: 서버는  api 제공, 클라이언트는 사용자 인증이나 컨텍스트등을 직접 관리하는 구조이기에 서버와 클라이언트간 역할이 명확하여 서로간 의존성이 매우 적다. 
5. 자체 표현 구조: api만 보고도 자원이 어떤 구조로 이루어져있는지 알 수 있다. 
6. 계층형 구조: rest api 서버는 다중 계층 으로 구성될 수 있으며, 보안 로드 밸런싱등의 계층을 추가할 수 있기에 가지는 특징 

api: 어플리케이션 프로그래밍 인터페이스, 응용프로그램등에서 사용가능하도록 운영체제나 프로그래밍 언어가 제공하는 기능을 제어할 수 있도록 만든 인터페이스

api gateway: 모든 api요청의 관문. 클라이언트의 api요청은 전부 직접 서버로 가는것이 아니라 이 게이트웨이를 통해 이루어지게 된다. 시스템의 구조를 숨기고 단순 api에 대한 표현만을 클라이언트에게 주게 된다. 
 게이트웨이 왜 씀? -> 서버 부하를 줄여주는 로드 밸런서의 역할 + 보안을 위해 내부 서버를 숨길 수 있음 + 동일한 요청에 대한 불필요한 반복을 줄이는 캐싱 사용가능 + 시스템을 오가는 요청 및 응답에 대한 모니터링 가능 + 서비스에 대한 변경 사항이 생기더라도 클라이언트는 알 필요없이 게이트웨이 내부의 변경만으로도 처리가 가능해진다. 
(디자인 패턴 중 facade 패턴과 유사하게 생김) 또한 다양한 출처의 서비스를 모두 관리하기에 cors를 가능하게 설정해줘야 한다. 


# 3. 메모리

cpu와 가까운(빠른 메모리) 순서 
레지스터 -> cpu 캐시 메모리 -> 메인 메모리 -> 보조기억장치 (하드) -> 외부 기억장치 
-> cpu로 부터 멀어질수록 데이터를 많이 저장가능하지만 용량이 커지고 접근속도는 느려진다. 

메모리 구조 
: 크게 5개의 영역으로 분류됨 
낮은 주소부터 
1. code 영역: 사용자가 작성한 코드가 저장되는 영역이다. 실행할 프로그램의 코드가 저장됨, 프로그램이 끝날때까지 메모리에 계속 남게되는 영역이기도 함(rom : 읽기 전용)
2. data 영역: 프로그램의 전역변수와 static 변수 등이 저장되어 있는 영역, 프로그램 실행 시 이 두 변수는 메인 함수가 호출되기 전에 데이터 영역에 할당된다. 얘도 끝까지 메모리에 남음
 또한 bss, data 2개의 세부 영역으로 또 나뉜다. 
   2-1. data 영역: 초기화가 이루어진 변수들이 저장됨. 프로그램 실행 중 자유롭게 접근해서 수정과 변경이 가능함. 사실 데이터 영역은 rom 영역에 저장되는데 전역변수와 스태틱 변수를 롬에 저장하면 계속 초기값에서 바뀔 수 없으니 데이터 영역을 램에 복사해둔다고 함 
   2-2 bss
   
3. heap 영역: 프로그램이 실행되는 동안 동적으로 사용할 데이터가 저장되는 영역. 사용자가 직접 관리하는 영역. 메모리의 낮은 주소부터 높은 주소로 할당된다. (커짐) 런타임 시 힙 영역의 크기가 결정
4. stack 영역: 함수 호출 시 생성되는 지역변수와 매개변수가 저장되는 영역. 함수 호출 시 할당되며 실행이 끝나면 메모리에서 해제된다. 높은주소 부터 낮은 주소로 할당된다. 컴파일 시 할당될 영역의 크기가 정해진다. 

+ 스택과 힙은 같은 공간을 사용한다. 그래서 둘중 하나가 커지면 나머지는 점점 작은 영역만 사용가능해짐 


페이징과 세그멘테이션 

 - 페이징: 가상 메모리는 페이지라 불리는 고정 크기의 블록으로 나누어지고 물리메모리는 프레임이라 불리는 페이지와 같은 크기의 블록들로 나누어진다. 사용자는 하나의 주소를 지정하고 하드웨어에 의해 페이지 번호와 변위로 분할된다. 페이지 테이블에는 각 페이지 번호와 그에 해당하는 프레임의 시작 물리주소를 저장해두는데 할당은 항상 프레임의 정수 배로 할당되는데 이 때 프로세스가 페이지 경계와 일치 않는 크기의 메모리를 요구하게 되면 마지막 페이지 프레임은 전부 사용되지 않는 문제가 발생(내부 단편화) 

- 세그멘테이션: 페이징 처럼 논리 메모리와 물리메모리를 같은 크기의 블록이 아닌 서로 다른 크기의 논리적 단위인 세그먼트로 분할. 사용자가 2개의 주소를 지정(세그먼트 번호와 변위) 세그먼트 테이블에는 각 세그먼트의 기준(시작 물리주소) 와 한계 (세그먼트 길이)를 저장. 서로 다른 크기의 세그먼트들이 메모리에 적재되고 제거되는 일이 반복되다 보면 자유 공간들이 많은 수의 작은 조각들로 나누어져 못 쓰게 될 수도있다. (외부 단편화)

외부 단편화를 없애는 방법으로 연속 메모리 할당 방식이 사용된다. 크게 3가지 방법으로 나뉜다.
1. 최소 적합: 메모리를 순차탐색하여 제일 먼저 발견한 적절하게 들어갈 수 있는 곳을 찾아 프로세스를 적재 -> 최고로 속도가 빠름
2. 최적 적합: 메모리를 탐색해 제일 적절하게 들어갈 수 있는 곳을 찾아 프로세스를 적재 -> 메모리 이용률 측면에서 최고, 최대한 빈 구멍을 안만드는 방식으로 운영됨 
3. 최악 적합: 메모리를 탐색해 프로세스의 크기와 제일 안 맞는 공간에 프로세스를 넣는 방식

메모리 관리 방법 중 페이지 교체 알고리즘

1. fifo 메모리에 가장 먼저 올라온 페이지를 먼저 내보낸다. 하지만 프레임의 수가 적을수록 패아자 결함이 더 발생함. 계속 교체를 해야하기 때문.
2. opt 알고리즘: 가장 사용하지 않을 페이지를 가장 우선적으로 내려 보내는 알고리즘인데 사실상 미래를 알 수 없어서 쓰기가 힘들다.
3. lru 알고리즘: 최근에 사용하지 않은 페이지를 가장 먼저 내려보내는 알고리즘: 과거만 보고 판단하기에 실질적으로 사용가능한 알고리즘. 

# 4. 데이터베이스
