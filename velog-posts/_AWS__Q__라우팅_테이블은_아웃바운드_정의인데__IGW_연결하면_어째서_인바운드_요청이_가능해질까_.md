# [AWS] Q. 라우팅 테이블은 아웃바운드 정의인데, IGW 연결하면 어째서 인바운드 요청이 가능해질까?

https://velog.io/@yohan9569/AWS-%EB%9D%BC%EC%9A%B0%ED%8C%85-%ED%85%8C%EC%9D%B4%EB%B8%94%EC%9D%80-%EC%95%84%EC%9B%83%EB%B0%94%EC%9A%B4%EB%93%9C-%EC%A0%95%EC%9D%98%EC%9D%B8%EB%8D%B0-%EC%96%B4%EC%A7%B8%EC%84%9C-%EC%9D%B8%EB%B0%94%EC%9A%B4%EB%93%9C-%EC%9A%94%EC%B2%AD%EC%9D%B4-%EA%B0%80%EB%8A%A5%ED%95%B4%EC%A7%88%EA%B9%8C

<h1 id="요약">요약</h1>
<p>결론: IGW 라우팅 연결 없어도, Public IP가 있으면 인바운드 요청을 받을 수 있지만, 응답은 나가지 못함.</p>
<ol>
<li>IGW 는 자체적으로 Public IP ↔ Private IP 매핑 테이블이 있다.</li>
<li>Public Subnet에 있는 EC2 Instance는 Public IP 가 있다. (Auto-assigned IP 옵션 선택 시)</li>
<li>외부에서 해당 Instance의 Public IP 로 요청을 보내면, IGW 를 거쳐서 요청은 인스턴스에 도달한다.</li>
<li>근데 응답할 때가 문제. IGW 를 통해 내부에서 사용하던 Private IP를 Public IP로 변환하고, 요청 처로 응답을 보내줘야 하는데, IGW 로 가는 라우팅 정보가 없으니 응답이 나가질 못한다.</li>
</ol>
<p></br></br></p>
<hr>
<p></br></br></p>
<h1 id="의문점과-가설">의문점과 가설</h1>
<h3 id="배경">배경</h3>
<p>아래 주제로 AWS 실습을 진행 중이었습니다.</p>
<blockquote>
</blockquote>
<ul>
<li>주제: AWS VPC 및 서브넷 설정하기 &amp; Public EC2 생성 후 외부 접근 허용</li>
<li>순서: <ol>
<li>VPC 직접 만든 뒤 설정 확인하기</li>
<li>Subnet 생성하기</li>
<li>EC2 (Public Subnet) 과제용 테스트 인스턴스 만들기</li>
<li>IGW 생성 및 VPC 내 설정하여 앞선 EC2 의 외부 노출 시도하기</li>
<li><strong>Route Table 를 통해 IGW 와 EC2 를 연결하여, 외부 노출을 위한 설정 완료하기</strong></li>
<li>인스턴스 SSH (22) 접근 확인 후 인스턴스 내에서 ping google.com 입력하기</li>
</ol>
</li>
</ul>
<p>여기서 중요한 점은 VPC에 IGW 도 있고, EC2 인스턴스가 Public IP를 가지고 있더라도,
<strong>라우팅 테이블로 IGW를 target으로 정해줘야 한다</strong>는 부분입니다.</p>
<p>그렇구나~ 하던 중 아래와 같은 궁금증이 일어났습니다.</p>
<p></br></br></p>
<h3 id="질문-과정">질문 과정</h3>
<p><img src="https://velog.velcdn.com/images/yohan9569/post/b2bc537c-5853-4135-9b2d-3fab83b5c45c/image.png" alt=""></p>
<ul>
<li>라우팅 테이블은 일종의 표지판 역할을 합니다.
첫번째 컬럼의 &quot;대상&quot;은 대상 주소를 의미합니다.
라우팅 테이블의 한 행은 해당 범위 주소의 트래픽은 이 곳으로 향해라 라는 의미입니다.</li>
</ul>
</br>

<p>여기서 다음과 같은 의문이 생겼습니다.</p>
<blockquote>
<ol>
<li>이 설정이 양방향은 아닌 것 같고, 아웃바운드에 대한 설정 같은데?</li>
<li>그럼 내 EC2 인스턴스로 가라는 안내는 어디있지?</li>
</ol>
</blockquote>
</br>

<p>그래서 아래의 질문을 챗 GPT에게 던지면서 질문을 구체화 해갔습니다.</p>
<p>✅ Route Table의 Destination, Target은 인바운드인가? 아웃바운드인가? 아니면 둘 다인가?
⇒ 아웃바운드: Route Table은 네트워크 내부에서 패킷이 &quot;어디로 향해야 하는지&quot;를 결정하는 역할.</p>
<p>✅ Route Table이 아웃바운드(Outbound)라면서, 왜 들어오는 요청도 IGW로 향하는 Route Table이 필요해?
⇒ 얼버무리기 시작</p>
<p>✅ 0.0.0.0/0 Destination → IGW로 가라. == 아웃바운드 설정. 근데 어떻게 인바운드가 가능해지는가?
⇒ 흐음..</p>
<p></br></br></p>
<h3 id="단초">단초</h3>
<blockquote>
<p><img src="https://velog.velcdn.com/images/yohan9569/post/f3af8796-5614-4df4-9a8d-3196b314dd86/image.png" alt=""></p>
<p>재밌는 사실: 서브넷에 IGW가 없고 노드에 퍼블릭 IP 주소가 있어도 노드는 여전히 인터넷에서 트래픽을 수신합니다. 나가는 트래픽이 삭제된다는 것은 또 다른 문제입니다.</p>
<p>출처: Reddit의 &quot;Do route tables control incoming traffic&quot; 의 댓글 내용
<a href="https://www.reddit.com/r/aws/comments/ldw3tr/do_route_tables_control_incoming_traffic/?rdt=44538">https://www.reddit.com/r/aws/comments/ldw3tr/do_route_tables_control_incoming_traffic/?rdt=44538</a></p>
</blockquote>
<p>&quot;아?&quot; 들어오는 건 되지만, 나가질 못한다?!</p>
<p></br></br></p>
<h3 id="가설-도달-강사님께-질문">가설 도달, 강사님께 질문</h3>
<blockquote>
<p>여러 자료 조사 후 잠정적 결론이 아래와 같습니다.</p>
<p>결론: IGW 라우팅 연결 없어도, Public IP가 있으면 인바운드 요청을 받을 수 있지만, 응답은 나가지 못함.</p>
</blockquote>
<ol>
<li>IGW 는 자체적으로 Public IP &lt;-&gt; Private IP 매핑 테이블이 있다.</li>
<li>Public Subnet에 있는 EC2 Instance는 Public IP 가 있다.</li>
<li>외부에서 해당 Instance의 Public IP 로 요청을 보내면, IGW 를 거쳐서 요청은 인스턴스에 도달한다.</li>
<li><strong>근데 응답할 때가 문제. IGW 를 통해 내부에서 사용하던 Private IP를 Public IP로 변환하고, 요청 처로 응답을 보내줘야 하는데, IGW 로 가는 라우팅 정보가 없으니 응답이 나가질 못한다.</strong><blockquote>
</blockquote>
일단 이렇게 생각하면 납득이 되는데, 이렇게 받아들여도 괜찮을까요?<blockquote>
</blockquote>
(EC2에서 정말 요청을 받고 처리하는지 로그를 찍어보는 실험을 해보면 정확할 것 같기는 한데, 아직 수행해보진 못했습니다.)</li>
</ol>
<p></br></br></p>
<h3 id="강사님-확인">강사님 확인</h3>
<blockquote>
<ol>
<li>맞습니다. 그래서 Auto-assigned IP 통한 EC2 생성 시 여기 테이블에 추가됩니다.</li>
<li>네, Auto-assigned IP 옵션을 선택했을때만.<ul>
<li>그래서 해당 옵션을 꼭 선택해야한다고 계속 강조한 이유기도합니다.</li>
</ul>
</li>
<li>네, 맞습니다.</li>
<li>오, 그렇게 보는것이 이해에 더 도움되겠네요 🙂</li>
</ol>
</blockquote>
<p>EC2 에서 요청과 응답을 테스트해보고싶으면 <code>tcpdump</code> 명령어 사용하면 됩니다. 예전 쿠팡에서 플랫폼 엔지니어가 저걸로 인바운드 아웃바운드 패킷 분석하던데 멋있더라구요.</p>
<p>그래서 실험하기로!</p>
<p></br></br></p>
<hr>
<p></br></br></p>
<h1 id="실험">실험</h1>
<h3 id="설계">설계</h3>
<ul>
<li>흐름<ol>
<li>패킷 캡쳐 모니터링을 수행할 노트북의 IP 만 IGW로 연결한다.</li>
<li>노트북으로 EC2에 접속해 패킷 캡쳐 명령어를 수행한다.</li>
<li>다른 IP의 휴대폰으로 EC2에 요청을 보낸다.</li>
<li><strong>요청을 캡쳐한다면 가설 입증!</strong></li>
<li>혹시 모르니 전체 IP에 IGW를 연결했을 때는 어떤지 비교한다.</li>
</ol>
</li>
</ul>
<ul>
<li>세부 사항<ul>
<li>라우팅 테이블 1차: 노트북 (WIFI) IP 만 IGW로 연결</li>
<li>라우팅 테이블 2차: 0.0.0.0/0(전체) IP에 IGW로 연결</li>
<li>SG: http 0.0.0.0/0 오픈</li>
<li>패킷 캡쳐: sudo tcpdump -i enX0 port 80</li>
<li>인바운드 요청: 아이폰 (LTE) 노트북과 다른 IP, http 요청</li>
</ul>
</li>
</ul>
<p></br></br></p>
<h3 id="결과">결과</h3>
<p><strong>요청 캡쳐된다!!!</strong></p>
</br>

<ol>
<li>닫혔을 때</li>
</ol>
<ul>
<li><p>모니터링 화면
→ 요청 패킷은 계속 캡쳐되지만, 응답 처리는 안 됨.
<img src="https://velog.velcdn.com/images/yohan9569/post/94ad4988-bf39-467f-afc4-8bd49d816e4c/image.png" alt=""></p>
</li>
<li><p>클라이언트 화면</p>
<p align="center">
  <img src="https://velog.velcdn.com/images/yohan9569/post/e54ab8a1-4739-4c65-9913-9a6e3a84298d/image.PNG" width="50%" height="50%">
</p>

</li>
</ul>
</br>

<ol start="2">
<li>열렸을 때</li>
</ol>
<ul>
<li><p>모니터링 화면
→ 똑같이 요청했을 때 오류 처리되고 끝.
<img src="https://velog.velcdn.com/images/yohan9569/post/efb3d75f-8484-4f95-9b34-c89fa5458fde/image.png" alt=""></p>
</li>
<li><p>클라이언트 화면</p>
<p align="center">
  <img src="https://velog.velcdn.com/images/yohan9569/post/374552d7-cd73-42e8-966c-18aebb2f6fa0/image.PNG" width="50%" height="50%">
</p>





</li>
</ul>
<p></br></br></p>
<h3 id="오래-걸린-이유">오래 걸린 이유</h3>
<ol>
<li>처음 EC2 생성 시 Key Pair를 적용 안 해놓고, 나중에 적용하다가 시간 버림 → 처음 인스턴스 생성부터 어떤 Key Pair 쓸 지 결정해야 함!</li>
<li>IGW 라우팅을 전부 끊는 순간, 애초에 내가 EC2에 접근을 못해서 모니터링을 못함.  → 내 로컬 IP는 IGW 라우팅 연결하고, 요청을 다른 IP로 보내기로.</li>
</ol>
<p></br></br></p>
<h3 id="추가-질문">추가 질문</h3>
<p>Q. IGW 가 아예 없는 경우, Public IP 만으로 요청 패킷이 인스턴스에 도달 가능할까?</p>
<ul>
<li>일단 실험 불가: IGW 가 아예 없으면, 모니터링이 불가.</li>
<li>예상: DNAT(Public IP → Private IP로 목적지 변경) 역할을 해주는 IGW 가 없으니 도달하지 못할 것 같다!</li>
</ul>
<p></br></br></p>
<hr>
<p></br></br></p>
<h1 id="배운점">배운점</h1>
<ul>
<li>편히 가는 데 집착하지 말자. 편히 가려다 잘 안 돼서 오히려 시간을 많이 보냈다.</li>
<li>처음 인스턴스 생성부터 어떤 Key Pair 쓸 지 결정해야 함!</li>
<li>tcpdump 명령어로 패킷 분석할 수 있다.</li>
</ul>
