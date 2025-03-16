# [ERD] 내가 ERD 짤 때의 주요 고려 사항 + ERD 관계 읽는 법

https://velog.io/@yohan9569/ERD-%EB%82%B4%EA%B0%80-ERD-%EC%A7%A4-%EB%95%8C%EC%9D%98-%EC%A3%BC%EC%9A%94-%EA%B3%A0%EB%A0%A4-%EC%82%AC%ED%95%AD-ERD-%EA%B4%80%EA%B3%84-%EC%9D%BD%EB%8A%94-%EB%B2%95

<h1 id="erd-설계-시-주요-고려-사항">ERD 설계 시 주요 고려 사항</h1>
<h3 id="1-어떤-객체테이블-이-필요한지">1. 어떤 객체(테이블) 이 필요한지</h3>
<ul>
<li><strong>한 행이 정확히 무엇</strong>을 의미하는지 정의</li>
<li>이 속성(컬럼)이 정말 이 객체의 속성일까?</li>
</ul>
</br>

<h3 id="2-테이블-간-관계는-어떤지">2. 테이블 간 관계는 어떤지</h3>
<ul>
<li>M:N 또는 상호 참조는 없는지. 있으면 중간에 Associative entity 추가.</li>
</ul>
</br>

<h3 id="3-그-후에">3. 그 후에</h3>
<p><img src="https://velog.velcdn.com/images/yohan9569/post/21a72425-be72-426c-a539-d24f6bf406d3/image.png" alt="">
(절대 한 번에 완성된 적이 없다..)</p>
<ul>
<li>비즈니스를 구현하는데 놓친 부분은 없는지</li>
<li>API 작성 시 어떻게 동작할지, 불편한 점은 없는지</li>
<li>성능, 최적화</li>
</ul>
<p></br></br></p>
<h3 id="-진짜-맨날-헷갈리는-erd-relationship-읽는-법">+ 진짜 맨날 헷갈리는 ERD Relationship 읽는 법!</h3>
<p><img src="https://velog.velcdn.com/images/yohan9569/post/af4d8ab1-5433-4cd2-aa1a-42bd3119a9a6/image.png" alt=""></p>
<p><img src="https://velog.velcdn.com/images/yohan9569/post/13f49651-6549-46d1-b288-b8c93756836a/image.png" alt=""></p>
<ol>
<li>이 테이블에서 저 테이블로 향하는 화살표를 읽는다고 생각하자.</li>
<li>꼭 양방향으로 읽어보자.</li>
</ol>
<p>출처: &quot;Data Modeling and Relational Database Design&quot; by Oracle, 2001</p>
