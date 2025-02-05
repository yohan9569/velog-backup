# [React] 환경설정 : Vite 통한 React 초기앱 설정 - ESLint, Prettier, nvm

https://velog.io/@yohan9569/React-%ED%99%98%EA%B2%BD%EC%84%A4%EC%A0%95-Vite-%ED%86%B5%ED%95%9C-React-%EC%B4%88%EA%B8%B0%EC%95%B1-%EC%84%A4%EC%A0%95

<h3 id="배경">배경</h3>
<p>CRA 못 쓴다! (서드파티 라이브러리 관리해주기 넘 빡셈)
→ 리액트만 개발: Vite 로 번들링 등 기본 설정 + 여러 직접 설정
→ Next.js 로 개발:  훨씬 편해질 것임. (create-next-app)</p>
<p>그래서, Vite 통한 React 초기앱 설정하는 과정을 정리 (맥북 관점).</p>
<hr>
<h1 id="0-node-버전-관리-및-선택적-사용을-위한-nvm-설치">0. Node 버전 관리 및 선택적 사용을 위한 nvm 설치</h1>
<p>JS 런타임 두 가지 = 1. 웹 브라우저 2. Node.js
근데! 우리 브라우저로 개발할 수는 없잖아 → Node 로 개발, 브라우저로 디버깅
근데! 각자 로컬에 설치된 Node 버전 제각각, 프로젝트마다 필요한 Node 버전 제각각 → 그래서 필요한 게,</p>
<blockquote>
<p>NVM: Node Version Manager. Node 버전 맞춰주기. 예) nvm use 18</p>
</blockquote>
<ol>
<li><p>가장 먼저 nvm 설치
iterm (또는 터미널) 에서 아래 코드 실행.</p>
<pre><code class="language-bash">curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash</code></pre>
</li>
<li><p>그 다음에는 nvm 명령어 사용을 위한 환경설정이 필요
<code>vi ~/.zshrc</code> 편집기 열어서, 맨 밑에 아래 코드 추가.</p>
<pre><code class="language-bash">export NVM_DIR=&quot;$([ -z &quot;${XDG_CONFIG_HOME-}&quot; ] &amp;&amp; printf %s &quot;${HOME}/.nvm&quot; || printf %s &quot;${XDG_CONFIG_HOME}/nvm&quot;)&quot;
[ -s &quot;$NVM_DIR/nvm.sh&quot; ] &amp;&amp; \. &quot;$NVM_DIR/nvm.sh&quot; # This loads nvm</code></pre>
<p><code>source ~/.zshrc</code> 통해 설정 재로딩이 가능.</p>
</li>
</ol>
<p></br></br></br></p>
<h1 id="1-nvm-을-통한-node-설정-및-npm-사용">1. nvm 을 통한 Node 설정 및 npm 사용</h1>
<p>nvm 설치 완료 시 nvm 통해 로컬에 설치된 node 버전은 무엇들이 있으며, 현재 어떤걸 쓰고있는지 확인 가능</p>
<ul>
<li><p>자주 사용하는 nvm 명령어 모음</p>
<ul>
<li><code>nvm list</code> : 현재 내 로컬에 설치되어있는 node 버전 + 현재 사용 버전 <strong>확인</strong></li>
<li><code>nvm install 18</code> : 특정 버전 node <strong>설치</strong></li>
<li><code>nvm install --lts 18</code> : <strong>18</strong> 버전 중 가장 최신의 LTS 버전 node <strong>설치</strong></li>
<li><strong><code>nvm install</code></strong> : 현재 위치한 프로젝트의 <code>.nvmrc</code> 파일 내 명시되어있는 node 버전 자동 <strong>설치</strong><ul>
<li><code>.nvmrc</code> 에 개발자가 개발할 때 사용한 버전을 명시. 누가 clone하면, 이 버전 써야 됨! 하는 것.</li>
</ul>
</li>
<li><code>nvm use 18</code> : 특정 버전 node <strong>선택</strong></li>
<li><strong><code>nvm use</code></strong> : 현재 위치한 프로젝트의 <code>.nvmrc</code> 파일 내 명시되어있는 node 버전 자동 <strong>선택</strong></li>
</ul>
</li>
<li><p>Deeper Shell Integration : .nvmrc 기준의 install / use 자동 사용 위한 쉘 스크립트 등록
<code>~/.zshrc</code> 에 아래 스크립트 입력</p>
<pre><code class="language-bash"># place this after nvm initialization!
autoload -U add-zsh-hook

load-nvmrc() {
  local nvmrc_path
  nvmrc_path=&quot;$(nvm_find_nvmrc)&quot;

  if [ -n &quot;$nvmrc_path&quot; ]; then
    local nvmrc_node_version
    nvmrc_node_version=$(nvm version &quot;$(cat &quot;${nvmrc_path}&quot;)&quot;)

    if [ &quot;$nvmrc_node_version&quot; = &quot;N/A&quot; ]; then
      nvm install
    elif [ &quot;$nvmrc_node_version&quot; != &quot;$(nvm version)&quot; ]; then
      nvm use
    fi
  elif [ -n &quot;$(PWD=$OLDPWD nvm_find_nvmrc)&quot; ] &amp;&amp; [ &quot;$(nvm version)&quot; != &quot;$(nvm version default)&quot; ]; then
    echo &quot;Reverting to nvm default version&quot;
    nvm use default
  fi
}

add-zsh-hook chpwd load-nvmrc
load-nvmrc</code></pre>
</li>
<li><p>자바스크립트 프로젝트에서 라이브러리 설치 및 구동을 위해 필요로하는 Node 버전 명시를 위해 사용하는 설정 파일들</p>
<ul>
<li><strong><code>.nvmrc</code> 파일</strong> : <code>nvm install</code> / <code>nvm use</code> 명령어 호출 시 본 <code>.nvmrc</code> 파일 내 <strong>명시된 <code>node</code> 버전 선택</strong></li>
<li><strong><code>.npmrc</code> 파일</strong> : <code>engine-strict = true</code> 옵션 명시 시, <strong>어긋난 <code>node</code> / <code>npm</code> 버전 사용하면 에러 발생</strong>  (npmrc = node package manager 설정 파일)</li>
<li><strong><code>package.json</code> 내 <code>&quot;engine&quot;</code> 옵션</strong> : <strong>프로젝트 설치 및 구동을 위해 필요한 <code>node</code> / <code>npm</code> 버전 명시</strong></li>
</ul>
</li>
</ul>
<p>→ 그래서 뭐 하라고? 
nvm 설치 → Deeper Shell Integration → (프로젝트 .nvmrc 설정 있으면) → nvm install → nvm use</p>
<p></br></br></br></p>
<h1 id="2-vite-통한-react-초기앱-시작">2. Vite 통한 React 초기앱 시작</h1>
<p>강사님이 마련한 Vite 프로젝트에서 시작한다.
직접 Vite 를 통해 React 생성하지 않는 이유: 몇몇 라이브러리들이 너무 최신. (ESLint 같은 경우 현업에서는 최대한 8 버전을 많이 사용할텐데, 9 버전을 설치하는 등)</p>
<ol>
<li>실습 코드 클론</li>
<li>nvm install → nvm use</li>
</ol>
<p>만약 직접 Vite 를 통해 React 를 시작한다면, 아래의 명령어를 치기.</p>
<pre><code class="language-bash">npm create vite@latest react-tutorial -- --template react</code></pre>
</br>

<h3 id="21-vite-통한-react-초기-프로젝트-내-파일-설명-및-eslint-확인">2.1. Vite 통한 React 초기 프로젝트 내 파일 설명 및 ESLint 확인</h3>
<p>처음 Vite-React 프로젝트 시작하면 존재하는 + 설정한 파일들
(따라할 건 아니고, 개념 설명)</p>
<div style="display: flex; align-items: center;">
  <img src="https://velog.velcdn.com/images/yohan9569/post/f61d2587-bace-4b3c-8214-7ce59d7f5c62/image.png" alt="이미지 설명" style="width: 15%;">
  <div>
  <p>스크린샷에서 역순으로 파일 설명</p>
  <ul>
  <li>
    <strong>vite.config.js</strong>: 개발용 런타임 설정 + 운영용 번들링 설정
    <ul>
      <li>번들러 역할: 1. 배포할 때 진짜 번들링, 2. 개발 시 수정사항 웹소켓으로 브라우저에 바로 연결</li>
      <li>npm run dev → node 서버가 돌아갈 것. 개발 서버. dev server.</li>
    </ul>
  </li>
  <li>
    <strong>package.json</strong>: <em>개발용</em> + <em>운영용</em> 라이브러리 버전 + 스크립트
    <ul>
      <li>Semantic Version: 메이저.마이너.패치 + ^ 혹은 ~ 범위</li>
      <li>script: npm run ~~ {alias}.</li>
      <li>dependencies: 배포할 때 필요한 라이브러리들. → 가볍게 유지하는 게 좋다.</li>
      <li>devDependencies: 개발할 때만 필요한 라이브러리들.</li>
    </ul>
  </li>
  <li>
    <strong>package-lock.json</strong>: 디펜던시 트리 (라이브러리 설치 순서) → 디펜던시 때문. 삭제하면 클나..
  </li>
  <li>
    <strong>index.html</strong>: React에서 렌더하기 위한 시작 HTML
  </li>
  <li>
    <strong>.nvmrc</strong>: nvm 에서 자동 사용/설치하기 위한 node 버전 명시
  </li>
  <li>
    <strong>.npmrc</strong>: 프로젝트에서 npm 시 사용해야하는 node 버전 강제
  </li>
  <li>
    <strong>.gitignore</strong>: Git 사용 시 관리하지 않을 파일들의 예외 규칙
  </li>
  <li>
    <strong>.eslintrc.cjs</strong>: 컴파일 설정 = 문법 오류 규칙
  </li>
</ul>
</div>
</div>


<p>아래 진행할 Formatter &amp; Linter &amp; IntelliSense 설정 파일들</p>
<ul>
<li><code>jsconfig.json</code> : VSCode 내 IntelliSense 인식 설정</li>
<li><code>.pretterrc</code> : Formatter 중 하나인 Prettier 설정</li>
</ul>
</br>

<h3 id="211-vite-react-프로젝트-내-필수-설정--1-linter---eslint">2.1.1. Vite-React 프로젝트 내 필수 설정 : (1) Linter - ESLint</h3>
<p>ESLint : 문법 (컨벤션) 오류를 지적해주는 Linter 적용 (Vite 통해 React 시작 시 ESLint 는 자동으로 설치되어있음)</p>
<ul>
<li><p><strong>이유</strong> : JS 는 컴파일 과정이 없는 인터프리터 언어이기에 런타임 에러가 자주 발생해 사전 에러 확인 필요.
당장은 개발 속도가 느려지는것처럼 느껴지겠지만, 문법을 잘못써서 시간을 날리는 상황 많이 발생.</p>
</li>
<li><p>ESLint 8 버전 설정파일인 .eslintrc.cjs 살펴보기 (<strong>아래 내용 복붙</strong>).</p>
<pre><code class="language-bash">module.exports = {
  root: true,
  env: { browser: true, es2020: true },
  extends: [
    &quot;eslint:recommended&quot;,
    &quot;plugin:react/recommended&quot;,
    &quot;plugin:react/jsx-runtime&quot;,
    &quot;plugin:react-hooks/recommended&quot;,
  ],
  ignorePatterns: [&quot;dist&quot;, &quot;.eslintrc.cjs&quot;],
  parserOptions: { ecmaVersion: &quot;latest&quot;, sourceType: &quot;module&quot; },
  settings: { react: { version: &quot;18.3&quot; } },
  plugins: [&quot;react-refresh&quot;],
  rules: {
    &quot;react/jsx-no-target-blank&quot;: &quot;off&quot;,
    &quot;react-refresh/only-export-components&quot;: [
      &quot;warn&quot;,
      { allowConstantExport: true },
    ],
  },
};</code></pre>
<ul>
<li>extends : 직접 Rules 들을 추가해주기때문에, 원하는것들을 고를 수 없고 선택지없이 그대로 적용  (plugins + rules 바로 구매)</li>
<li>plugins : 추가할 수 있는 Rules 리스트를 제공하는 라이브러리, 원하는것들을 담기 (장바구
니)</li>
<li>rules : plugins 에서 제공하는 Rules 중 원하는것들을 선택하여 적용 (장바구니에서 구매)</li>
</ul>
</li>
<li><p>나중에 ESLint 이것저것 설정하는게 귀찮다면 Airbnb Style Guide 를 사용하던가, 원하는 규칙을 만들것</p>
</li>
<li><p>꼭, VSCode 확장 프로그램 중 ESLint 설치할 것</p>
</li>
</ul>
</br>

<h3 id="212-vite-react-프로젝트-내-필수-설정--2-formatter---prettier">2.1.2. Vite-React 프로젝트 내 필수 설정 : (2) Formatter - Prettier</h3>
<p>Prettier : 코딩 시 일관된 포맷을 갖게해주는 Formatter 적용</p>
<ul>
<li>이유 : 팀 프로젝트 진행 시 각자 로컬에서 공동 코드를 개발할텐데 각자 로컬의 포맷팅에 의한 오염 방지. → 각자 다른 prettier 규칙을 적용할 수 있으니, 맞춰야 한다!</li>
<li>vscode의 prettier 와 차이점? 프로젝트의 포매팅 설정이 있으면 그걸 사용, 없으면 default로 vscode의 prettier 사용.</li>
</ul>
<ol>
<li><p>Prettier 설치</p>
<pre><code class="language-bash">npm install -D -E prettier</code></pre>
<ul>
<li>-save-dev or -D : devDependencies 내 라이브러리 추가 및 배포가 아닌 개발시에만 사용</li>
<li>-save-exact or -E : 정확한 SemVer 버전 사용 시 (^5.1.3 이 아닌 5.1.3)</li>
</ul>
</li>
<li><p>ESLint 와 충돌하지 않게 추가 플러그인 설치 </p>
<pre><code class="language-bash">npm install -D eslint-config-prettier</code></pre>
<ul>
<li>eslint-config-prettier : ESLint 와 Prettier 가 충돌하지 않게 하는 ESLint 플러그인</li>
</ul>
</li>
</ol>
<ul>
<li>2.1. prettier-plugin-tailwindcss 설치<pre><code class="language-bash">npm install -D prettier-plugin-tailwindcss</code></pre>
<ul>
<li><code>prettier-plugin-tailwindcss</code> : 추후 Tailwind 사용 시 유틸리티 클래스명 순서로 나열하는 플러그인</li>
</ul>
</li>
</ul>
<ol start="3">
<li><p>8 버전 설정파일 .eslintrc.cjs extends 속성 마지막에, 앞서 설치한 ‘eslint-config-prettier&#39;, 플러그인 추가</p>
</li>
<li><p>package.json 내 &quot;format&quot;: &quot;prettier --write ./src&quot;, 추가 후 npm run format 실행</p>
</li>
<li><p>.prettierrc 파일 생성</p>
</li>
<li><p>Prettier 설정파일인 .prettierrc 살펴보기</p>
<p>.prettierrc : 위에 설치한 prettier-plugin-tailwindcss 등록 및 CSS 줄넘김 방지</p>
<pre><code class="language-bash">{
 &quot;printWidth&quot;: 100,
 &quot;tabWidth&quot;: 2,
 &quot;trailingComma&quot;: &quot;all&quot;,
 &quot;singleQuote&quot;: true,
 &quot;jsxSingleQuote&quot;: true,
 &quot;semi&quot;: false,
 &quot;plugins&quot;: [&quot;prettier-plugin-tailwindcss&quot;],
 &quot;overrides&quot;: [
     {
         &quot;files&quot;: &quot;*.css&quot;,
         &quot;options&quot;: {
               &quot;printWidth&quot;: 0,
         }
     }
 ]
}</code></pre>
</li>
<li><p>.eslintrc.json(.eslintrc.cjs) 파일 내 앞서 설치한 eslint-config-prettier 충돌 방지 플러그인 등록</p>
<pre><code class="language-bash">module.exports = {
 root: true,
 env: { browser: true, es2020: true },
 extends: [
   &#39;eslint:recommended&#39;,
   &#39;plugin:react/recommended&#39;,
   &#39;plugin:react/jsx-runtime&#39;,
   &#39;plugin:react-hooks/recommended&#39;,
   &#39;eslint-config-prettier&#39;, // == &#39;prettier&#39;, 항상 마지막에 위치.
 ],
 ignorePatterns: [&#39;dist&#39;, &#39;.eslintrc.cjs&#39;],
 parserOptions: { ecmaVersion: &#39;latest&#39;, sourceType: &#39;module&#39; },
 settings: { react: { version: &#39;18.3&#39; } },
 plugins: [&#39;react-refresh&#39;],
 rules: {
   &#39;react/jsx-no-target-blank&#39;: &#39;off&#39;,
   &#39;react-refresh/only-export-components&#39;: [&#39;warn&#39;, { allowConstantExport: true }],
 },
}</code></pre>
</li>
<li><p>VSCode 와 연동하기 (Prettier 확장 프로그램 설치 및 기본 설정)
(없어도 Prettier CLI 도구는 독립적으로 동작하므로 VSCode 확장 프로그램 없이도 적용 가능)</p>
<p>꼭, VSCode 확장 프로그램 중 Prettier 설치한 후 + 기본 포맷터 및 저장 시 자동 포맷팅 VSCode 설정</p>
<ul>
<li>(맥북 기준 Command + <code>,</code> 을 통해) VSCode 내 Prettier 설정에서 <code>Editor: **Format On Save**</code> 체크해야 저장 시 적용</li>
<li>또한, (맥북 기준 Command + <code>,</code> 을 통해) VSCode 내 Prettier 설정에서 <code>Editor: **Default Formatter**</code> 설정 필요</li>
</ul>
</li>
</ol>
</br>

<h3 id="213-vite-react-프로젝트-내-필수-설정--3-intellisense---jsconfig--tsconfig">2.1.3. Vite-React 프로젝트 내 필수 설정 : (3) IntelliSense - JSConfig / TSConfig</h3>
<p>jsconfig.json : 개발 언어 에디터(Sublime, VSCode)에서 사용하는 LSP 으로 intellisense 설정에 해당</p>
<ul>
<li>기존 타입스크립트 설정파일인 tsconfig.json 을 상속받은 설정파일 jsconfig.json (파일 생성 후 안에 아래 코드 넣기)<pre><code class="language-bash">{
  &quot;compilerOptions&quot;: {
    &quot;jsx&quot;: &quot;react-jsx&quot;,
    &quot;lib&quot;: [&quot;esnext&quot;, &quot;dom&quot;],
    &quot;checkJs&quot;: true,
    &quot;baseUrl&quot;: &quot;.&quot;,
    &quot;paths&quot;: {
      &quot;@/*&quot;: [&quot;src/*&quot;]
    },
    &quot;types&quot;: [&quot;vite/client&quot;]
  },
  &quot;include&quot;: [&quot;src/**/*.d.ts&quot;, &quot;src/**/*.js&quot;, &quot;src/**/*.jsx&quot;, &quot;src/**/*.ts&quot;, &quot;src/**/*.tsx&quot;],
  &quot;exclude&quot;: [&quot;node_modules&quot;, &quot;**/node_modules&quot;, &quot;dist&quot;]
}</code></pre>
<ul>
<li>compilerOptions : 원랜 TS 컴파일 옵션이지만 JS 에선 컴파일되진 않고 설정 그대로 쓰려고 이름만 차용<ul>
<li>jsx : 본 설정을 하지 않으면 React 파일 내 에러 발생</li>
<li>lib : ES(ECMA Script) 버전에 따른 문법들을 혼용하고 싶을때, 특정 버전들을 라이브러리처럼 추가</li>
<li>checkJs : 타입스크립트뿐만 아니라 자바스크립트에서도 에러를 표기하고 수정가능하도록 돕는다.</li>
<li>baseUrl : 프로젝트 루트, 가장 일반적으로 . 사용 (나머지 옵션서 .src/ 가 아닌 src/ 사용 가능)</li>
<li>paths : baseUrl 를 기준으로 절대경로 지정이 가능</li>
<li>types : Vite 는 Node.js API 기반의 타입 시스템을 차용하기에, 클라이언트 환경 위해 꼭 필요한 설정</li>
</ul>
</li>
<li>include : 타입스크립트에서 .jsx, .js 확장자를 사용하거나 일부 next-env.d.ts 선언파일 사용 시</li>
<li>exclude : 실제 우리가 작성하는데 사용되는 코드를 성능을 위해 intellisense 에서 제외</li>
</ul>
</li>
</ul>
</br>

<h3 id="214-반드시-절대-경로-사용하기">2.1.4. (반드시) 절대 경로 사용하기!!!</h3>
<ol>
<li><p>auto import 확장 프로그램 설치</p>
</li>
<li><p>VSCode 에도 설정 필요 : 사용자 설정 열기 후 typescript import module specifier 검색 </p>
<ul>
<li>TypeScript &gt; Preferences: Import Module Specifier: non-relative</li>
<li>JavaScript &gt; Preferences: Import Module Specifier: non-relative</li>
</ul>
</li>
<li><p>Vite 에도 설정 필요 : 절대경로 사용을 위해 jsconfig.json 뿐만 아니라 vite.config.js 수정 필수</p>
<ul>
<li>jsconfig.json 은 VSCode 를 위한 컴파일 설정이기에 vite.config.js 내 런타임 설정 필수<pre><code class="language-bash">npm install -D vite-tsconfig-paths</code></pre>
<pre><code class="language-bash">import { defineConfig } from &#39;vite&#39;
import react from &#39;@vitejs/plugin-react&#39;
import tsconfigPaths from &#39;vite-tsconfig-paths&#39;
</code></pre>
</li>
</ul>
<p>// <a href="https://vitejs.dev/config/">https://vitejs.dev/config/</a>
export default defineConfig({
 plugins: [react(), tsconfigPaths()],
})</p>
<pre><code>- App.jsx, main.jsx : 기존 파일의 상대 경로 → 절대 경로 로 바꿔주기.

</code></pre></li>
</ol>
<p></br></br></br></p>
<hr>
<h1 id="배운점">배운점</h1>
<p>늘, 개발 자체보다는 그 전에 세팅하는 부분이 더 어렵게 느껴진다.
하지만 그 어려운 부분을 해내야 한 사람의 개발자로서 역할을 온전히 할 수 있지 않나 생각해본다.</p>
<p>물론, 한번 해봤다고 다음부터 완벽히 할 수 있는 것은 아니겠지만, 적어도 아주 막막했던 처음보다는 더듬더듬~ 하면서 나아진 모습을 기대할 수 있을 것 같다.</p>
