# vue 기초강좌 (https://www.youtube.com/watch?v=TIHluPn05VY&list=PLcqDmjxt30RsdnPeU0ogHFMoggSQ_d7ao)
## vue nodebird 유료강좌 전 선수 무료 강좌.
* 강좌소스 (https://github.com/zerocho/vue-webgame)

## 강좌소개
* vue-cli사용하지 않고 프로젝트 생성하여 만들어보기.
  - 기본원리를 잘 알기 위함.
* 공식문서 먼저 읽어보기 (https://kr.vuejs.org/v2/guide/index.html)
* 먼저 cdn으로 html에서 불러와서 만들어보기
  - 우선 간단한 좋아요 버튼 클릭 html페이지를 만든다.
```
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>좋아요</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>
<div id="root">
    <div v-if="liked">좋아요 눌렀음</div>
    <button v-else v-on:click="onClickButton">Like</button>
</div>
</body>
<script>
  const app = new Vue({
    el: '#root',
    data: {
      liked: false,
    },
    methods: {
      onClickButton() {
        this.liked = true;
      },
    },
  });
</script>
</html>
```
## data와 v-if, v-else
* 데이터에 따라 이벤트, 조건문을 적용하는 방법을 설명
* v-on:
  - methods에 있는 함수와 여러가지 이벤트를 연결 하는 방법
  - v-on:click"함수명"
* v-if
  - if, else 는 동등한 형제태그이고 붙어있어야한다.
  - v-if="data값"
* v-else
  - v-if에서 data에 따라 아닐경우 else로 빠짐
## 보간법과 v-model
* 구구단 html을 사용하여 보간법과 v-model 을 알아보기
* 보간법
  - 중괄호 두번으로 data에 있는 값을 html에 표시할 수 있다. {{ }}
* v-model
  - data의 값과 태그상에 data를 연결해주는 방법 v-model
  - 아래 소스에서는 input에 걸려있는 v-model로 인하여 data의 value가 바뀌게 된다.
```
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>구구단</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>
<div id="root">
    <div>{{first}}곱하기 {{second}}는?</div>
    <form v-on:submit="onSubmitForm">
        <input type="number" ref="answer" v-model="value">
        <button type="submit">입력</button>
    </form>
    <div id="result">{{result}}</div>
</div>
<script>
  const app = new Vue({
    el: '#root',
    data: {
      first: Math.ceil(Math.random() * 9),
      second: Math.ceil(Math.random() * 9),
      value: '',
      result: '',
    },
    methods: {
      onSubmitForm(e) {
        e.preventDefault();
        if (this.first * this.second === parseInt(this.value)) {
          this.result = '정답';
          this.first = Math.ceil(Math.random() * 9);
          this.second = Math.ceil(Math.random() * 9);
          this.value = '';
          this.$refs.answer.focus();
        } else {
          this.result = '땡';
          this.value = '';
          this.$refs.answer.focus();
        }
      },
    },
  });
</script>
</body>
</html>
```
## ref와 구구단 완성
* data, methods등은 this로 접근가능
* 특정 tag element에 접근하는 방법 ref
  - 원하는 element에 ref="이름지정"
  - 사용시에는 this.$refs.이름지정 : 이런식으로 접근가능 
## 끝말잇기 만들며 복습
* v-on, v-if, v-else, v-model, ref, {{ }} 등 복습
```
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>끝말잇기</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>
<div id="root">
    <div>{{word}}</div>
    <form v-on:submit="onSubmitForm">
        <input type="text" ref="answer" v-model="value">
        <button type="submit">입력!</button>
    </form>
    <div>{{result}}</div>
</div>
<script>
  const app = new Vue({
    el: '#root',
    data: {
      word: '제로초',
      result: '',
      value: '',
    },
    methods: {
      onSubmitForm(e) {
        e.preventDefault();
        if (this.word[this.word.length - 1] === this.value[0]) {
          this.result = '딩동댕';
          this.word = this.value;
          this.value = '';
          this.$refs.answer.focus();
        } else {
          this.result = '땡';
          this.value = '';
          this.$refs.answer.focus();
        }
      },
    },
  });
</script>
</body>
</html>
```
## 컴포넌트의 필요성
* 끝말잇기를 통하여 컴포넌트를 생성하여 만들어보기.
  - Vue.component('컴포넌트명', {}) 이와같이 생성한다.
* 같은 모양의 UI를 여러개 만들어야 하는경우 중복하여 코딩하면 같은 데이터를 공유하는 문제도 있고해서 다양한 문제가 있음.
  - 그래서 반복되는 부분은 컴포넌트로 만든다.
  - 사용했던 data, methods들을 모두 컴포넌트로 옮긴다.
  - 컴포넌트의 tag들은 template에 넣어준다.
```
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>끝말잇기</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>
<div id="root">
    <word-relay start-word="제로초"></word-relay>
    <word-relay start-word="초밥"></word-relay>
    <word-relay start-word="바보"></word-relay>
</div>
<script>
    Vue.component('wordRelay', { // 전역 컴포넌트
      template: `
        <div>
            <div>{{word}}</div>
            <form v-on:submit="onSubmitForm">
                <input type="text" ref="answer" v-model="value">
                <button type="submit">입력!</button>
            </form>
            <div>{{result}}</div>
        </div>
      `,
      props: ['startWord'],
      data() {
        return {
          word: this.startWord,
          result: '',
          value: '',
        };
      },
      methods: {
        onSubmitForm(e) {
          e.preventDefault();
          if (this.word[this.word.length - 1] === this.value[0]) {
            this.result = '딩동댕';
            this.word = this.value;
            this.value = '';
            this.$refs.answer.focus();
          } else {
            this.result = '땡';
            this.value = '';
            this.$refs.answer.focus();
          }
        },
      }
    })
</script>
<script>
  const app = new Vue({
    el: '#root',
  });
</script>
</body>
</html>
```
## 컴포넌트의 특성
* **중요** 컴포넌트에서는 data를 함수로 만들어야 한다. data(){}
  - 컴포넌트는 여러번 많이 사용되므로 data가 달라야 하므로 함수로 만들어야 한다.
  - templete안의 최상위 태그는 하나의 태그로 되어있어야 한다. react의 React.Fragment 와 같이 (vue에서는 template : cdn에서는 안됨)로 감싸면 된다.
  - 컴포넌트는 vue 인스턴스보다는 상단에 있어야 한다.
  - Vue.component('컴포넌트명', {}) 이런 형식은 전역컴포넌트 형식이다.
  - new Vue({el:'#root', components:{컴포넌트명: {}}}) 이런 형식은 지역컴포넌트 형식이다. 이 컴포넌트는 root안에서만 사용 가능.

## props와 웹팩의 필요성
* 컴포넌트는 공통되는 부분 외에 달라지는 부분을 적용할때 props를 사용한다.
* 컴포넌트 tag에는 start-word="값" 이렇게 케밥케이스로 넣어주고 props: ['startWord'] 이와같이 카멜케이스 형식으로 text로 넣어준다.
  - 이와같이 되었으면 this.startWord 와 같이 가져와서 쓸 수 있다.
* 웹팩을 사용하지 않고 스크립트를 작성하면 위 소스와 같이 script가 많이지고 복잡해진다.
  - 그래서 웹팩을 사용하여 script를 파일로 분리해서 코딩하면 추 후 build과정에 하나로 합쳐준다.

## 웹팩 사용하기
* node(npm) 설치
* cmd창에서 프로젝트 폴더 생성 후 해당 폴더에서 npm init 
* npm i vue 설치
* npm i webpack webpack-cli -D 웹팩 설치 (개발) -D 와 --save-dev 같음.
* root폴더에 webpack.config.js를 생성
  - 크게 4개 중요 **entry, module, plugins, output**
```
const VueLoaderPlugin = require('vue-loader/lib/plugin');
const path = require('path');

module.exports = {      // node의 모듈을 만든다.
  mode: 'development',  // 개발버전, 실서비스(production)
  devtool: 'eval',      // 개발버전, 실서비스(hidden-source-map ? 확인해봐야함)
  resolve: {        // 확장자 처리
    extensions: ['.js', '.vue'],    // js, vue를 처리한다.(코딩시 .vue와 같이 확장자를 안써줘도 됨)
  },
  entry: {              // 하나로 합쳐줄 app.js파일의 진입시점 js파일 main.js파일 
    app: path.join(__dirname, 'main.js'),
  },
  module: {         // 웹팩의 핵심
    rules: [{       // 합칠때의 규칙을 작성 (webpack의 추가 처리 사항))
      test: /\.vue$/,
      use: 'vue-loader',
    }],
  },
  plugins: [      // 모듈들 처리 완료시 플러그인으로 한번 더 가공처리(후처리)한다고 생각하면됨
    new VueLoaderPlugin(),
  ],
  output: {                 // 나중에 합쳐줄 파일명, 위치 등등
    filename: '[name].js',      // 합쳐줄 파일(entry에 app이라고 적었기 때문에 [name] 와 같이 넣어줘도 된다. 아니면 app.js라고 해도 된다)
    path: path.join(__dirname, 'dist'), // 합쳐줄 폴더
  },
};
```
## 프로젝트 구조와 웹팩빌드
* main.js파일 코딩
  - vue를 import로 가져와서 new Vue와 같이 인스턴스를 만들어서 루트element'#root'를 넣어준다.
  - NumberBaseball는 vue 컴포넌트이다.
  - el 역할을 $mount가 대신 해줌.
```
import Vue from 'vue';
import NumberBaseball from './NumberBaseball';

new Vue(NumberBaseball).$mount('#root');
```
  - NumberBaseball.vue 컴포넌트
  - .vue 컴포넌트 파일은 template, script, style로 나눠져 코딩할 수 있게 되어있다.
  - 기존 컴포넌트 소스를 코딩한다.(zen코딩(현재는 emmet)으로 코딩하면 편리함)
```
<template>
  <div>
    <h1>{{result}}</h1>
    <form @submit.prevent="onSubmitForm">
      <input ref="answer" minlength="4" maxlength="4" v-model="value" />
      <button type="submit">입력</button>
    </form>
    <div>시도: {{tries.length}}</div>
    <ul>
      <li v-for="t in tries" :key="t.try">
        <div>{{t.try}}</div>
        <div>{{t.result}}</div>
      </li>
    </ul>
  </div>
</template>

<script>
  const getNumbers = () => {
    const candidates = [1, 2, 3, 4, 5, 6, 7, 8, 9];
    const array = [];
    for (let i = 0; i < 4; i += 1) {
      const chosen = candidates.splice(Math.floor(Math.random() * (9 - i)), 1)[0];
      array.push(chosen);
    }
    return array;
  };

  export default {
    data() {
      return {
        answer: getNumbers(), // ex) [1,5,3,4]
        tries: [], // 시도 수
        value: '', // 입력
        result: '', // 결과
      }
    },
    methods: {
      onSubmitForm() {
        if (this.value === this.answer.join('')) { // 정답 맞췄으면
          this.tries.push({
            try: this.value,
            result: '홈런',
          });
          this.result = '홈런';
          alert('게임을 다시 실행합니다.');
          this.value = '';
          this.answer = getNumbers();
          this.tries = [];
          this.$refs.answer.focus();
        } else { // 정답 틀렸을 때
          if (this.tries.length >= 9) { // 10번째 시도
            this.result = `10번 넘게 틀려서 실패! 답은 ${this.answer.join(',')}였습니다!`;
            alert('게임을 다시 시작합니다.');
            this.value = '';
            this.answer = getNumbers();
            this.tries = [];
            this.$refs.answer.focus();
          }
          let strike = 0;
          let ball = 0;
          const answerArray = this.value.split('').map(v => parseInt(v));
          for (let i = 0; i < 4; i += 1) {
            if (answerArray[i] === this.answer[i]) { // 숫자 자릿수 모두 정답
              strike++;
            } else if (this.answer.includes(answerArray[i])) { // 숫자만 정답
              ball++;
            }
          }
          this.tries.push({
            try: this.value,
            result: `${strike} 스트라이크, ${ball} 볼입니다.`,
          });
          this.value = '';
          this.$refs.answer.focus();
        }
      }
    }
  };
</script>

<style>

</style>
```
  - 만들어놓은 html을 실행하기 위해서는 package.json에 'scripts'부분에 'build'(webpack)를 셋팅한다. "scripts": {"build": "webpack"}
  - npm run build시 에러가 나면 하나씩 해결해 나간다.
  - path가 맞지 않으므로 const path = require('path'); 로 가져와서 경로 부분에 적절히 셋팅한다.(webpack.config.js)
  - main.js에서 컴포넌트를 불러와 연결(import NumberBaseball from './NumberBaseball';)해줄때 에러가 발생한다.
    - 이유는 webpack이 config에 설정되어있는 걸 읽을때 최초 main.js를 먼저 읽는다. 
    - main.js안에 .vue파일의 컴포넌트를 읽을때 javascript가 아닌 코딩파일이므로 webpack.config.js에 rules를 설정해준다.
      **module: { rules: [{test: /\.vue$/, use: 'vue-loader', }], },**
    - rules로 설정한 .vue를 읽는 **vue-loader**를 사용했으므로 설치를 해야한다. npm i vue-loader -D  그리고 webpack.config.js상단에 const VueLoaderPlugin = require('vue-loader/lib/plugin');불러와서 plugins부분에 plugins: [ new VueLoaderPlugin(),], 해줘야 함.
    - 앞으로 .vue파일은 설치한 vue-loader가 읽어주므로 에러없이 잘 됨.
    - 다음 에러 vue-template-compiler를 설치하라고 함. npm i vue-template-compiler -D 설치. (이것은 vue와 버전이 같아야 한다.)
    - 여기까지 진행시 에러가 없어야 함.
    - npm run build하면 dist/app.js 생김. 하지만 소스가 읽기 힘들게 합쳐짐. 그래서 webpack설정을 세부적으로 설정하여 적용 할 수 있다.
      - webpack.config.js 설정에서 mode: 'development', devtool:'eval', resolve 등 설정을 한다. 
      - 현재까지 설정은 파일 내용이 달라졌을때 build를 꼭 해줘야 함.
      - 현재까지 하면 dist/app.js파일이 없다고 에러가 남. 결과 app.js파일 경로를 바꿔야 할것으로 보임.

## webpack 로더 사용하기
* 위 에러 처리 과정에 vue-loader를 설치하는 과정 참조.

## v-for로 반복문 사용
* 숫자야구 관련 컴포넌트 코딩 시작. 소스는 위 참조.
* 숫자야구 시도한 숫자만큼의 배열을 v-for로 반복하여 보이게 간단한 코딩
  - &lt;li v-for="t in tries" :key="t.try"&gt;{{t}}&lt;/li&gt;
* 참고로 현재까지 작업 내용은 수정됬을때 마다 build를 계속 해줘야 함. (npm run build) - 불편함을 체험... 추 후 자동빌드 셋팅 해줄꺼임.
* v-on은 @로 바꿀수 있음 v-on:submit == @submit  또한 event.preventDefault()는 @submit.prevent 로 사용할 수 있다.

## 숫자야구 완성하기
* 위 완성 코딩 참조.
* let변수를 사용할때와 data에 넣을때의 차이는 화면에서 사용하느냐 아니냐의 차이.
* methods에는 현재 화면과 연관이 있는 함수들만 넣어주고 공통적인 함수들은 따로 빼서 관리한다.

## webpack watch 와 반응속도 체크
* javascript 아닌 것들을 추가 할때마다 webpack의 rules에 loader를 추가해주면 webpack이 알아서 js output으로 합쳐서 만들어줌. (css, image, html 등)
* webpack.config.js의 현재까지 큰 틀에서 추가 옵션들을 하나씩 추가해 나가면서 적용하면 됨.
* 반응속도체크 프로젝트를 새로 생성.
* 프로젝트 새로 생성하는 과정에 설정되어야 하는 여러가지에 대한 설명
  - git 에 올려질때는 node_modules, dist 등 올리지 말아야 하는 파일 폴더들은 .gitignore파일을 만들어 관리한다.
  - package.json파일에 들어있는 모듈들을 다시 설치 해야 하므로 프로젝트 생성 후 해당 폴더에서 npm i 를 해줌.
* 파일 수정될때마다 알아서 build해주는 설정은 webpack.config.js의 scripts설정에 "build": "webpack --watch", 이런식으로 watch기능을 사용할 수 있다.

## v-bind와 vue style 
* 각 컴포넌트의 style부분에 css소스를 넣고 화면tag에서는 class로 사용하여 적용할 수 있다.
  - 하지만 style을 사용 하려면 vue-loader만 가지고는 안되므로 npm i vue-style-loader css-loader -D 가 추가 되어야 한다. 그리고 webpack.config.js에 rules에 추가한다.
  - 또한 class는 단순 attribute이므로 class에 data값을 넣고 싶을때는 v-bind:class="data명" 으로 해서 넣어주면 해당 data를 사용할 수 있다.
  - v-bind:class=  이런 형식은 :class= 와 같은 방법(축약형)으로 사용할 수 있다.
  - 컴포넌트의 style태그에 **scoped**속성을 넣으면 해당 컴포넌트만 적용되는 style이 된다.
* 수정된 webpack.config.js 파일 참조
```
const VueLoaderPlugin = require('vue-loader/lib/plugin');
const path = require('path');

module.exports = {
  mode: 'development',
  devtool: 'eval',
  resolve: {
    extensions: ['.js', '.vue'],
  },
  entry: {
    app: path.join(__dirname, 'main.js'),
  },
  module: {
    rules: [{
      test: /\.vue$/,
      use: 'vue-loader',
    }, {
      test: /\.css$/,
      use: [
        'vue-style-loader',
        'css-loader',
      ]
    }],
  },
  plugins: [
    new VueLoaderPlugin(),
  ],
  output: {
    filename: '[name].js',
    path: path.join(__dirname, 'dist'),
    publicPath: '/dist',
  },
};
```

## webpack-dev-server 
* 소스 수정이 생기면 watch에서 바로 빌드 해주므로 화면에서는 새로고침만 해주면 됨.
* 새로고침도 자동으로 해줬으면 한다... 
  - npm i webpack-dev-server -D 설치 후 package.json의 scripts에 dev서버로 돌리는 코딩을 한다.
  - 또한 webpack.config.js에 output에 publicPath를 추가해서 시작 위치를 정해준다.
```
{
  "name": "response-check",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "build": "webpack --watch",
    "dev": "webpack-dev-server --hot"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "vue": "^2.6.10"
  },
  "devDependencies": {
    "css-loader": "^3.0.0",
    "vue-loader": "^15.7.0",
    "vue-style-loader": "^4.1.2",
    "vue-template-compiler": "^2.6.10",
    "webpack": "^4.35.2",
    "webpack-cli": "^3.3.5",
    "webpack-dev-server": "^3.7.2"
  }
}
```

## 반응속도체크 게임 완성하기
* ResponseCheck.vue 컴포넌트 소스
```
<template>
  <div>
    <div id="screen" :class="state" @click="onClickScreen">{{message}}</div>
    <template v-if="result.length">
      <div>평균 시간: {{average}}ms</div>
      <button @click="onReset">리셋</button>
    </template>
  </div>
</template>

<script>
  let startTime = 0;
  let endTime = 0;
  let timeout = null;
  export default {
    data() {
      return {
        result: [],
        state: 'waiting',
        message: '클릭해서 시작하세요.',
      };
    },
    computed: {
      average() {
        return this.result.reduce((a, c) => a + c, 0) / this.result.length || 0;
      }
    },
    methods: {
      onReset() {
        this.result = [];
      },
      onClickScreen() {
        if (this.state === 'waiting') {
          this.state = 'ready';
          this.message = '초록색이 되면 클릭하세요.';
          timeout = setTimeout(() => {
            this.state = 'now';
            this.message = '지금 클릭!';
            startTime = new Date();
          }, Math.floor(Math.random() * 1000) + 2000); // 2~3초
        } else if (this.state === 'ready') {
          clearTimeout(timeout);
          this.state = 'waiting';
          this.message = '너무 성급하시군요! 초록색이 된 후에 클릭하세요.'
        } else if (this.state === 'now') {
          endTime = new Date();
          this.state = 'waiting';
          this.message = '클릭해서 시작하세요.';
          this.result.push(endTime - startTime);
        }
      }
    },
  };
</script>

<style scoped>
  #screen {
     width: 300px;
     height: 200px;
     text-align: center;
     user-select: none;
   }
  #screen.waiting {
    background-color: aqua;
  }
  #screen.ready {
    background-color: red;
    color: white;
  }
  #screen.now {
    background-color: greenyellow;
  }
</style>
```

## computed와 v-show, template
* 계산하는 소스들 가공처리 할때는 tag쪽에서 하지말고  computed에서 처리한다.
  - 계산하는 소스들을 template tag에 넣으면 매번 binding된 data가 달라질때 마다 계속 다시 계산하고 소스가 다시 실행 되므로 비용이 많이 든다.
  - 이럴때 computed를 사용하면 연관이 없는 data가 변화 할때는 해당 computed값은 변화하지 않는다. 캐싱되어있으므로 성능상에 개선이 된다.
* v-show는 true, false냐에 따라 보이고 안보이고 처리가 된다.
  - v-show와 v-if의 차이는 해당 tag가 있으면서 display:none이면 v-show이고 아예 태그 자체가 없으면 v-if이다.
* 딱히 필요없는 div태그들(감싸주는 용도)은 template을 사용한다. template태그는 없는샘 치는 태그이다.
  - 가장 상위 template태그 바로 밑 자식 tag는 template을 또 사용할 수 없다.

## vue-devtools와 기타정보
* 확장프로그램에서 chrome vue devtools설치 (https://chrome.google.com/webstore/search/vue?hl=ko)
* 배포환경에서 vue-devtools를 못 보게 하기 셋팅 Vue.config.devtools = false;


# 기타 참조할만한 강의
## youtube
* 리엑트 고객관리 시스템 개발 (https://www.youtube.com/watch?v=_yEH9mczm3g&list=PLRx0vPvlEmdD1pSqKZiTihy5rplxecNpz&index=1)
* vue 무료 기본강좌 _ 제로초 (https://www.youtube.com/watch?v=TIHluPn05VY&list=PLcqDmjxt30RsdnPeU0ogHFMoggSQ_d7ao&index=1)