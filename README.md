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
* 



# 기타 참조할만한 강의
## youtube
* 리엑트 고객관리 시스템 개발 (https://www.youtube.com/watch?v=_yEH9mczm3g&list=PLRx0vPvlEmdD1pSqKZiTihy5rplxecNpz&index=1)
* vue 무료 기본강좌 _ 제로초 (https://www.youtube.com/watch?v=TIHluPn05VY&list=PLcqDmjxt30RsdnPeU0ogHFMoggSQ_d7ao&index=1)