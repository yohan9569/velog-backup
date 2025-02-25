# [GIT] 특정 브랜치만 클론 + 받아오면서 이름 바꾸기

https://velog.io/@yohan9569/GIT-%ED%8A%B9%EC%A0%95-%EB%B8%8C%EB%9E%9C%EC%B9%98%EB%A7%8C-%ED%81%B4%EB%A1%A0-%EB%B0%9B%EC%95%84%EC%98%A4%EB%A9%B4%EC%84%9C-%EC%9D%B4%EB%A6%84-%EB%B0%94%EA%BE%B8%EA%B8%B0

<h1 id="1-특정-브랜치만-클론">1. 특정 브랜치만 클론</h1>
<pre><code class="language-bash">git clone --branch &lt;브랜치명&gt; --single-branch &lt;repository&gt;</code></pre>
<p></br></br></p>
<h1 id="2-클론-받아오면서-이름-바꾸기">2. 클론 받아오면서 이름 바꾸기</h1>
<pre><code class="language-bash">git clone &lt;repository&gt; &lt;directory&gt;</code></pre>
<p></br></br></p>
<h1 id="3-퓨전">3. 퓨전!</h1>
<pre><code class="language-bash">git clone --branch &lt;브랜치명&gt; --single-branch &lt;repository&gt; &lt;directory&gt;</code></pre>
<p></br></br></br></br></p>
<hr>
<p>출처: <a href="https://git-scm.com/docs/git-clone">https://git-scm.com/docs/git-clone</a></p>
