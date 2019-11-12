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
  - 중괄호 두번으로 data에 있는 값을 html에 표한할 수 있다. {{ }}
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






# 기타 참조할만한 강의
## youtube
* 리엑트 고객관리 시스템 개발 (https://www.youtube.com/watch?v=_yEH9mczm3g&list=PLRx0vPvlEmdD1pSqKZiTihy5rplxecNpz&index=1)
* vue 무료 기본강좌 _ 제로초 (https://www.youtube.com/watch?v=TIHluPn05VY&list=PLcqDmjxt30RsdnPeU0ogHFMoggSQ_d7ao&index=1)