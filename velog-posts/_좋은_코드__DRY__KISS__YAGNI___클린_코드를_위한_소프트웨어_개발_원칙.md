# [좋은 코드] DRY, KISS, YAGNI - 클린 코드를 위한 소프트웨어 개발 원칙

https://velog.io/@yohan9569/%EC%A2%8B%EC%9D%80-%EC%BD%94%EB%93%9C-DRY-KISS-YAGNI-%ED%81%B4%EB%A6%B0-%EC%BD%94%EB%93%9C%EB%A5%BC-%EC%9C%84%ED%95%9C-%EC%86%8C%ED%94%84%ED%8A%B8%EC%9B%A8%EC%96%B4-%EA%B0%9C%EB%B0%9C-%EC%9B%90%EC%B9%99

<h1 id="요약">요약</h1>
<ul>
<li>DRY: Don&#39;t Repeat Yourself(반복하지 말자) → 공통된 것은 묶자(중복 코드 제거).</li>
<li>KISS: Keep It Simple and Stupid(심플하고 멍청하게 유지하자) → 한 눈에 알 수 있게 하자 (가독성 &amp; 단일 책임).</li>
<li>YAGNI: You Ain&#39;t Gonna Need It(너 그거 필요 없어) → 지금 필요한 것만 만들자 (물론 확장성 고려).</li>
</ul>
<p><br/><br/><br/><br/></p>
<h1 id="본문">본문</h1>
<p>보편적으로 좋은 프로그래밍 코드란 변경이 용이하고, 유지보수와 확장이 쉬운 코드.
클린 코드의 기준은 단일 목적과 오용, 남용 방지.
그를 위한 아래의 세가지 원칙.</p>
<h3 id="dry-dont-repeat-yourself반복하지-말자">DRY: Don&#39;t Repeat Yourself(반복하지 말자)</h3>
<ul>
<li><p>개념
  &quot;같은 코드를 반복하지 마라&quot;
  동일한 로직이 반복될 경우, 함수 또는 모듈로 분리하여 재사용성을 높임.
  시스템 내에서 로직은 단 한 곳에서 명확하고 신뢰할 수 있도록 존재해야 한다.
  (상반) ↔ WET: Write Every Time, Write Everything Twice, Waste Everyone&#39;s time.</p>
</li>
<li><p>이유</p>
<ul>
<li>안 지킨다면: 
로직의 변경사항이 필요한 경우 반복적으로 업데이트 수행.
하나라도 빠트리면? 유지보수 악몽.</li>
<li>지킨다면:
재사용성 높임.
로직 변경사항 시 → 한번만 업데이트.</li>
</ul>
</li>
</ul>
<ul>
<li><p>예시 코드</p>
<ul>
<li><p>❌ DRY 위반 (중복 코드) </p>
<pre><code class="language-java">class User {
    String name;

    void printUser() {
        System.out.println(&quot;User: &quot; + name);
    }
}

class Admin {
    String name;

    void printAdmin() {
        System.out.println(&quot;Admin: &quot; + name);
    }
}</code></pre>
</li>
<li><p>✅ DRY 준수 (중복 제거) </p>
<pre><code class="language-java">class Person {
    String name;

    void print() {
        System.out.println(&quot;User: &quot; + name);
    }
}

class User extends Person {}
class Admin extends Person {}</code></pre>
</li>
</ul>
</li>
</ul>
<h3 id="kiss-keep-it-simple-and-stupid심플하고-멍청하게-유지하자">KISS: Keep It Simple and Stupid(심플하고 멍청하게 유지하자)</h3>
<ul>
<li>개념
&quot;코드는 단순하고 직관적으로 작성하라&quot;
불필요한 복잡성을 줄이고, 쉽게 이해할 수 있도록 작성.
하나의 함수는 하나의 역할만 해야한다.
대부분의 시스템은 복잡하게 보다는 심플하게 만들어졌을 때 최고로 잘 동작한다.</li>
<li>이유<ul>
<li>안 지킨다면:
본인도, 동료도 이해하기 어렵다. 
코드 복잡도가 증가하여 유지보수 어려움. 
여러 역할이 혼재된 경우, 한 기능 변경 시 다른 기능에 영향.
불필요한 로직이 많아지면 실수할 확률 높아짐.</li>
<li>지킨다면:
유지보수와 확장이 쉬워짐, 코드 변경이 빠름.
실수할 확률 낮아짐.</li>
</ul>
</li>
</ul>
<ul>
<li><p>예시 코드</p>
<ul>
<li><p>❌ KISS 위반 (단일 목적을 지키지 않은 코드)</p>
<pre><code class="language-java">class User {
    String name;

    void saveAndSendNotification() {  // ❌ 저장과 알림 전송을 동시에 처리 (책임 분리 안 됨)
        System.out.println(&quot;Saving user: &quot; + name);
        System.out.println(&quot;Sending notification to user: &quot; + name);
    }
}</code></pre>
</li>
<li><p>✅ KISS 준수 (단일 목적 원칙 적용)</p>
<pre><code class="language-java">class User {
    String name;

    void save() {
        System.out.println(&quot;Saving user: &quot; + name);
    }
}

class NotificationService {
    void sendNotification(String name) {
        System.out.println(&quot;Sending notification to user: &quot; + name);
    }
}</code></pre>
</li>
</ul>
</li>
</ul>
<h3 id="yagni-you-aint-gonna-need-it너-그거-필요-없어">YAGNI: You Ain&#39;t Gonna Need It(너 그거 필요 없어)</h3>
<ul>
<li>개념
&quot;미리 기능을 만들지 말고, 실제로 필요할 때 추가하라&quot;
불필요한 기능을 구현하면 코드가 복잡해지고 유지보수가 어려워짐.
목적이 미래를 위한것이어서는 안된다.
시스템에 불필요한 복잡성을 더하지 않는 선에서 확장성 있는 코드 작성해야 한다.</li>
<li>이유<ul>
<li>안 지킨다면:
개발 비용, 유지보수 비용, 수리 비용.. 모든 개발 비용은 다 발생.
시간 낭비. 정말 필요한 기능에 사용할 시간.
유지보수 어려워짐.
오류 가능성도 높이고, 그걸 해결하는 비용 발생.</li>
<li>지킨다면:
핵심 기능 개발에 집중 가능.
오류 위험 줄어듦.
가독성 증가.</li>
</ul>
</li>
</ul>
<ul>
<li><p>예시 코드</p>
<ul>
<li><p>❌ YAGNI 위반 (필요 없는 기능 미리 구현)</p>
<pre><code class="language-java">class User {
    String name;

    void printName() {
        System.out.println(&quot;User: &quot; + name);
    }

    void exportToCSV() {  // ❌ 아직 필요하지 않은 기능
        System.out.println(&quot;Exporting user data...&quot;);
    }

    void sendNotification() {  // ❌ 사용되지 않음
        System.out.println(&quot;Sending notification...&quot;);
    }
}</code></pre>
</li>
<li><p>✅ YAGNI 준수 (필요한 기능만 구현)</p>
<pre><code class="language-java">class User {
    String name;

    void printName() {
        System.out.println(&quot;User: &quot; + name);
    }
}</code></pre>
</li>
</ul>
</li>
</ul>
<p><br/><br/><br/><br/></p>
<h1 id="배운점">배운점</h1>
<p>좋은 코드를 짠다는 것은 좋은 글을 쓴다는 것과 닮은 것 같다는 생각을 하게 된다.
읽기 좋아야 하고, 목적이 뚜렷하고, 하나의 주제(역할)를 다루는 등 비슷한 점이 많다.
그래서 이제부터 블로그 글도 최대한 하나의 주제만 다루도록 해봐야겠다.
서로 닮은 만큼, 좋은 코드를 짜다보면 좋은 글이 써지고, 좋은 글을 쓰다보면 좋은 코드를 짤 수 있지 않을까?!</p>
<p><br/><br/><br/><br/></p>
<hr>
<p>참고자료</p>
<ul>
<li>ASAC 7기 Aaron 강의 자료</li>
<li><a href="https://www.youtube.com/watch?v=jafa3cqoAVM">https://www.youtube.com/watch?v=jafa3cqoAVM</a></li>
</ul>
