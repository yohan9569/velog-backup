# [ASAC 복습] 01. 웹 구성 간 흐름 및 직무 종류

https://velog.io/@yohan9569/ASAC-%EB%B3%B5%EC%8A%B5-01.-%EC%9B%B9-%EA%B5%AC%EC%84%B1-%EA%B0%84-%ED%9D%90%EB%A6%84-%EB%B0%8F-%EC%A7%81%EB%AC%B4-%EC%A2%85%EB%A5%98

<ul>
<li>주제: 웹의 등장 및 웹의 구성.</li>
<li>글의 목적: 누군가에게 설명하는 글이 아닌 배운 것을 되짚고 정리하는 용입니다. 꾸준히 하기 위해 부담 없이 작성할 것이기에, 두서가 없거나 설명이 부족할 수 있습니다.</li>
</ul>
<hr>
<h2 id="01-01-웹-개요--웹-개발은-무엇이며-왜-배우는가">01-01. 웹 개요 : 웹 개발은 무엇이며 왜 배우는가</h2>
<ul>
<li>웹 개발은 무엇인가? 
웹 페이지에 무언가를 그려내는 것. 
무언가 → 데이터 → Backend
그린다 → Rendering → Frontend</li>
<li>웹 개발 왜 배우는가? 
웹 개발의 장점: 범용성. 크로스플랫폼.</li>
<li>웹 개요 중요성? 
결국 소통을 위해. 협업할 때 다른 직무 팀원의 말을 알아들어야 함.
개발자는 코딩 실력보다 대화 실력이 더 중요할 수도 있다.</li>
</ul>
<p><br/><br/><br/></p>
<h2 id="01-02-웹-본질--요청--반환의-주체와-방법">01-02. 웹 본질 : 요청 — 반환의 주체와 방법</h2>
<h4 id="1-요청--반환-주체--웹-브라우저--웹-서버">1. 요청 — 반환 “주체” : 웹 브라우저 + 웹 서버</h4>
<ul>
<li>웹 본질: <strong>요청과 반환</strong></li>
<li>누가 요청? 브라우저 or 다른 서버</li>
<li>누가 반환? 서버 (내 외부)
서버 하나 여러 서비스→ 모놀리식
서버 여러개 각 (하나) 서비스 → MSA</li>
<li>서버와 서버의 중개자? API Gateway</li>
</ul>
<h4 id="2-요청---반환-방법-rest-api-graphql-grpc-등">2. 요청 - 반환 “방법”: REST API, GraphQL, gRPC 등</h4>
<ul>
<li>REST API: <strong>한 눈에 알 수 있게 전달하자!</strong>  REpresentational State Transfer 표현하는 상태 통신(?). 요청-반환 방식 중 하나. (사실상 표준).
무엇을? 리소스 uri, 동사 method, 메시지 response status<ul>
<li>uri(식별, 장소 내 지정) = url(장소)  + urn.  스키마 + 호스트 + path.  <ul>
<li>/collection(집단)/doucument(id) (그중 누구)/store(걔의 뭐)/controller(메서드 한번더)</li>
<li>더 자세히? path variable, query parameter, request body, form data</li>
</ul>
</li>
<li>method → get, put, delete, post, patch 등.</li>
<li>response status → 10x 임시 정보성. 20x 성공. 30x 리다이렉트. 40x you fucked up. 50x we fucked up.</li>
</ul>
</li>
<li>GraphQL: 자원 특정하는 부담을 클라가. 하나의 단일 포인트에 원하는 데이터를 쿼리로 요청. 응답도 제각각.</li>
<li>gRPC: 스펙 몰라도 됨. (잘 모르겠음).</li>
</ul>
<p><br/><br/><br/></p>
<h2 id="01-03-웹-내-주체--웹-페이지를-사이에-둔-웹-브라우저와-웹-서버">01-03. 웹 내 주체 : 웹 페이지를 사이에 둔 웹 브라우저와 웹 서버</h2>
<blockquote>
<p>웹 주체 = 웹 서버 → 웹 페이지 → 웹 브라우저.</p>
</blockquote>
<ul>
<li>웹 등장: 물리학자들 문서(서버) 공유 → 링크 필요. 틀 필요. → html(웹 페이지) ⇒ 리더기 (웹 브라우저)</li>
</ul>
<h4 id="1-웹-페이지--html--css--js">1. 웹 페이지 : HTML + CSS + JS</h4>
<ul>
<li>웹 페이지<ul>
<li>html(구조, 링크) - 하이퍼텍스트 마크업 랭귀지. 링크, 구조(태그).</li>
<li>css(꾸미기) - 폭포수. 종속 관계 표현. 상위에 조작하면 하위도 포함. 
우선순위: 명시도, 하단.</li>
<li>js(이벤트 상호작용)</li>
</ul>
</li>
</ul>
<h4 id="2-웹-브라우저--rendering-절차">2. 웹 브라우저 : Rendering 절차</h4>
<ul>
<li>웹 브라우저: 랜더링.<ul>
<li>DOM Tree + CSSOM Tree → Render Tree ⇒ 브라우저 엔진이 그림. 
→ Layout(뷰 포트 계산) → Paint(색칠) html 바뀔 때마다 repaint. 
→ 그 비용 줄이기 위해 vdom(3개 요소 변경 → 3번 dom 변경X → vdom 3번, 모아서 dom 1번 변경)</li>
</ul>
</li>
</ul>
<h4 id="3-웹-서버와-웹-어플리케이션-서버">3. 웹 서버와 웹 어플리케이션 서버</h4>
<ol>
<li>WS만: 정적 웹 리소스 반환. → 데이터 바뀌면 매번 새로 페이지 만들거야? ⇒ 동적 웹 페이지(어플리케이션: 요청-연산-반환).</li>
<li>WS + A 그 사이 가교 역할이 common gateway interface 
a. CGI: 1요청 1비상주 프로세스. → 느림
b. FCGI: 1요청 1상주 프로세스. → 빠르지만, 상태가 남아서 보안 구림.
c. PHP: cgi를 포함하는 어플리케이션 형태 → 어플리케이션을 포함하는 WS 형태(톰캣과 유사)</li>
<li>WAS: <ul>
<li>톰캣&amp;네티: 1요청 1스레드(할당)<ol>
<li>스레드 풀에서 요청마다 스레드 할당시킴</li>
<li>서블릿: 할당된 1 자바 스레드(WAS)</li>
<li>서블릿 컨테이너: 할당, 회수 주체.</li>
</ol>
</li>
<li>템플릿 엔진
<img src="https://velog.velcdn.com/images/yohan9569/post/6f548479-44d8-4370-8166-d7b33edb3e4e/image.png" alt=""></li>
</ul>
</li>
</ol>
<p><br/><br/><br/></p>
<h2 id="01-04-인터넷과-인트라넷-그-사이의-isp-와-dns">01-04. 인터넷과 인트라넷 그 사이의 ISP 와 DNS</h2>
<ul>
<li><p>인트라넷: 내부 망</p>
<ul>
<li>라우터: 인트라넷 안의 private ip 연결</li>
<li>GW: 외부로 나가는 통로</li>
</ul>
</li>
<li><p>인터넷: 외부 망</p>
<ul>
<li>ISP: (인터넷 서비스 제공자), GW 연결해줌. <ul>
<li>DNS Resolver: 이 도메인이면 이 주소입니다.
루트서버(ICANN, . → .com이면 저기로) → TDL(.com / .net registry) → 네임서버(가비아 등에서 사는 것, asac.com  registrar)</li>
</ul>
</li>
</ul>
</li>
<li><p>AWS</p>
<ul>
<li>VPC 는 사설망 → GW 붙여야 인터넷. + 그걸 라우터 라우트 테이블에 등록.</li>
</ul>
</li>
</ul>
<p><br/><br/><br/></p>
<h2 id="01-05-웹-검색-엔진구글과-seo-그리고-core-web-vital">01-05. 웹 검색 엔진(구글)과 SEO 그리고 Core Web Vital</h2>
<ul>
<li><p>구글이(무언가) 웹 페이지 다 찾고(크롤링) 분류해서(인덱싱) 검색에 따라 결과 보여줌(검색 엔진).</p>
</li>
<li><p>검색어 - 결과 연관성: 구글 서비스의 핵심. 연관도 높여야 함. → 연관도 높게 보여야 함. == SEO</p>
</li>
<li><p>SEO 방식</p>
<p>좋은 내용, 시맨틱, 메타 태그와 키워드, (성능 performance metric,  웹 접근성)</p>
</li>
<li><p>성능: 로딩 속도, 반응 속도, 각각 용어들.</p>
</li>
</ul>
<p><br/><br/><br/></p>
<h2 id="01-06-웹-개발자-직무-종류">01-06. 웹 개발자 직무 종류</h2>
<h4 id="1-프론트엔드-개발자">1. 프론트엔드 개발자</h4>
<ul>
<li><p>퍼블리셔</p>
<p>  요새 리액트만 잘하고, 퍼블리싱 능력 없는 애들 많음 → 퍼블리싱 능력 갖춘 개발자 찾는 추세가 생김.</p>
</li>
<li><p>백오피스: 내부에서만 보는 페이지 만드는 일. SEO 및 Performance Metric 과 멀어짐(편하지만 위험). 대신 다양한 기술셋 사용 가능(어 해봐~)</p>
</li>
<li><p>대고객 페이지: SEO 및 Performance Metric 에 미침. 기술 선택은 보수적. UX 관점에서 사용성 극대화 및 인터렉티브 웹.</p>
</li>
</ul>
<h4 id="2-백엔드-개발자">2. 백엔드 개발자</h4>
<ul>
<li>솔루션 및 (게임) 클라이언트: CPU 및 메모리 자원 최적화, Rust 와 같이 커널 레벨에서의 컴퓨팅 자원 활용.</li>
<li>웹 백엔드: API 반환 레이턴시(자극과 반응 사이의 시간), DB 사용 시 Transaction 고려(@Transactional 잘못쓰면 다른 api 오염 가능함).</li>
</ul>
<h4 id="3-엔지니어">3. 엔지니어</h4>
<ul>
<li>인프라스트럭처 엔지니어<ul>
<li>DevOps: 빌드 및 배포 (CI/CD)</li>
<li>SRE (Site Reliability 안정성 신뢰성 Engineering): 서버 및 컨테이너 구성 → K8S 필수.</li>
<li>Security (White / Black): Server 관련 보안 요소 (예, MITM, Wo 등)</li>
</ul>
</li>
<li>DBA: 쿼리 및 인덱스 최적화, DB 수평적/수직적 확장 구조 설계.</li>
</ul>
