# [Github Actions] 주석으로 인한 Syntax Error

https://velog.io/@yohan9569/Github-Actions-%EC%A3%BC%EC%84%9D%EC%9C%BC%EB%A1%9C-%EC%9D%B8%ED%95%9C-Syntax-Error

<h1 id="요약">요약</h1>
<p>YAML의 # 주석과 script 내부의 # 주석을 혼동하지 않도록 주의해야 한다!</p>
<p></br></br></br></br></p>
<h1 id="질문">질문</h1>
<blockquote>
<p>요약: 왜 주석 때문에 문법 오류가 발생했을까?</p>
</blockquote>
<p>도커 실습 중 자동 배포 CD 부분에서의 질문.</p>
<p>Github Action 스크립트 중 Syntax Error 가 지속적으로 발생하는 문제가 있었다.
indentation 문제가 있을 수 있다는 경험자의 조언에 따라 이렇게 저렇게 바꿔봤는데도 안 됐다.
에이 설마~ 하는 마음으로 주석을 지웠더니 성공했다!</p>
<p>여기서 질문. 
저는 보통 주석으로 인해 문법 오류가 일어나는 것을 본 적이 없는데, 이번에는 왜 그런걸까?
혹시 다른 문제가 있었던 걸까?
(주석 때문이 아니라기엔 마지막 성공했을 때 변경사항이 주석 삭제 뿐이었다.)</p>
<p><img src="https://velog.velcdn.com/images/yohan9569/post/86c48fe3-91fc-4b91-aa3b-da6f521cdf46/image.png" alt=""></p>
<p><img src="https://velog.velcdn.com/images/yohan9569/post/95826b54-b423-4c2d-bcab-4d0cc85ffe33/image.png" alt=""></p>
<p><img src="https://velog.velcdn.com/images/yohan9569/post/22655f7b-d308-4a21-88af-1814eb2cb974/image.png" alt=""></p>
<p><img src="https://velog.velcdn.com/images/yohan9569/post/ec5787e1-7ef7-4e99-92d1-0a1145736956/image.png" alt=""></p>
<p></br></br></br></br></p>
<h1 id="가설">가설</h1>
<p>1.</p>
<blockquote>
<p>script 가 돌아가는 환경에서는 # 대신 다른 주석을 사용한다?</p>
</blockquote>
<p>→ script가 담긴 yml도, script가 실행될 AWS EC2의 bash도 # 주석을 사용한다. 
⇒ 폐기.</p>
</br>


<p>2.</p>
<p>주석이 있는데도 정상 동작한 동료의 사례가 있었다.
그럼 # 주석 자체가 문제같지는 않다.
그렇다면 조금 찜찜했던 부분</p>
<blockquote>
<p># 하고 간격이 좀 길잖아, 그게 문제 아녔을까?</p>
</blockquote>
<p></br></br></br></br></p>
<h1 id="해결">해결</h1>
<p>그래서 테스트 해봤다.</p>
<h3 id="결론">결론</h3>
<p>script 내부에서는 실제 실행될 코드와 동일한 들여쓰기로 주석을 작성해야 한다.
들여쓰기를 지키면 정상적으로 쉘 스크립트의 # 주석으로 인식되며, IDE에서도 색상이 달라지는 것을 확인할 수 있다.
들여쓰기를 지키지 않으면 YAML 파일의 # 주석으로 인식되며, IDE에서도 위의 주석들과 같은 색상으로 보여진다. 
YAML의 # 주석을 만나면 스크립트가 끝난 것으로 인식되는데, 그 후로 계속해서 스크립트 코드가 이어지니 문법 오류가 발생한다.</p>
<p>즉, YAML의 # 주석과 script 내부의 # 주석을 혼동하지 않도록 주의해야 한다.</p>
<p></br></br></p>
<h3 id="과정">과정</h3>
<ol>
<li>기존 문제가 된 주석<ul>
<li>IDE 화면: 위의 yml 주석들과 같이 초록색
<img src="https://velog.velcdn.com/images/yohan9569/post/99e412ee-6241-45ce-b9ff-0b9dc58ce2d1/image.png" alt=""></li>
</ul>
</li>
</ol>
<pre><code>- &quot;You have an error in your yaml syntax on line 75&quot;
![](https://velog.velcdn.com/images/yohan9569/post/3a98bbdf-6238-4b6d-8554-de33d51a865a/image.png)</code></pre></br>


<ol start="2">
<li>좀 더 들여쓰기 한 주석(&quot;script&quot; 와 같은 레벨)<ul>
<li>IDE 화면: 위의 yml 주석들과 같이 초록색
<img src="https://velog.velcdn.com/images/yohan9569/post/5ee6826f-6297-458b-9579-5b62905007cd/image.png" alt=""></li>
</ul>
</li>
</ol>
<pre><code>- &quot;You have an error in your yaml syntax on line 75&quot;
![](https://velog.velcdn.com/images/yohan9569/post/48b460ec-46cf-4de3-b084-ff6c1b3dd106/image.png)</code></pre></br>

<ol start="3">
<li><p>script 안쪽으로 들어간 주석</p>
<ul>
<li><p>IDE 화면: 위의 yml 주석들과 달리 흰색
<img src="https://velog.velcdn.com/images/yohan9569/post/1444c38d-ef96-4c5d-b73a-cb1ef3205866/image.png" alt=""></p>
</li>
<li><p>일단 동작! (했으나, 인스턴스를 종료시켜서 에러)
<img src="https://velog.velcdn.com/images/yohan9569/post/a805dbeb-5588-4537-b566-10f9088eef86/image.png" alt=""></p>
</li>
</ul>
</li>
</ol>
