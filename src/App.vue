<template>
  <div class="header">
    <ul class="header-button-left">
      <li v-if="step == 1  || step == 2"  @click="step = 0">Cancel</li>
    </ul>
    <ul class="header-button-right">
      <li v-if="step == 1" @click="step++">Next</li>
      <li v-if="step == 2" @click="publish()">등록</li>
    </ul>
    <img src="./assets/logo.png" alt="" class="logo">
  </div>

  <h5>안녕 {{$store.state.name}}</h5>
  <button @click="$store.commit('이름변경') ">버튼</button>
  <h5>내 나이는 {{ $store.state.age }}</h5>
  <button @click="$store.commit('나이변경')">버튼2</button> 

  <Container :게시물="게시물" :step="step" :이미지="이미지" @write="작성한글 = $event" />

  <div class="buttonArea">
    <button v-if="step == 0" @click="more">더보기</button>
  </div>

  <div class="footer">
    <ul class="footer-button-plus">
      <input @change="upload" accept="image/*" type="file" id="file" class="inputfile" />
      <label @click="showStep" for="file" class="input-plus">+</label>
    </ul>
  </div>
</template>

<script>
import Container from "./components/Container.vue"
import postData from './assets/data'
import axios from 'axios'



export default {
  data() {
    return {
      step: 0,
      게시물 : postData,
      moreNum : 0,
      이미지 : '',
      작성한글: '',
      선택한필터 : ''
    }
  },
  mounted() {
    this.emitter.on('박스클릭함', (a) => {
      this.선택한필터 = a
    });
  },
  name: 'App',
  components: {
    Container
  },
  methods: {
    publish() {
      var 내게시물 = {
        name: "Kim Solo",
        userImage: "https://placeimg.com/100/100/arch",
        postImage: this.이미지,
        likes: 36,
        date: "May 15",
        liked: false,
        content: this.작성한글,
        filter: this.선택한필터
      };
      this.게시물.unshift(내게시물)
      this.step = 0
      console.log(this.게시물)
    },
    more() {
      axios.get(`https://codingapple1.github.io/vue/more${this.moreNum}.json`)
        .then((결과)=> {
          console.log(결과.게시물.likes);
          this.게시물.push(결과.게시물);
          this.moreNum ++;
        })
    },
    upload(e) {
      let 파일 = e.target.files
      console.log(파일[0])
      let url = URL.createObjectURL(파일[0])
      console.log(url)
      this.이미지 = url
      this.step++
    },
    showStep() {

      console.log(this.step)
    }
  }
}
</script>

<style>
body {
  margin: 0;
}
ul {
  padding: 5px;
  list-style-type: none;
}
.logo {
  width: 22px;
  margin: auto;
  display: block;
  position: absolute;
  left: 0;
  right: 0;
  top: 13px;
}
.header {
  width: 100%;
  height: 40px;
  background-color: white;
  padding-bottom: 8px;
  position: sticky;
  top: 0;
}
.header-button-left {
  color: skyblue;
  float: left;
  width: 50px;
  padding-left: 20px;
  cursor: pointer;
  margin-top: 10px;
}
.header-button-right {
  color: skyblue;
  float: right;
  width: 50px;
  cursor: pointer;
  margin-top: 10px;
}
.footer {
  width: 100%;
  position: sticky;
  bottom: 0;
  padding-bottom: 10px;
  background-color: white;
}
.footer-button-plus {
  width: 80px;
  margin:  auto;
  text-align: center;
  cursor: pointer;
  font-size: 24px;
  padding-top: 12px;
}
.sample-box {
  width: 100%;
  height: 600px;
  background-color: bisque;
}
.inputfile {
  display: none;
}
input-plus {
  cursor: pointer;
}
.buttonArea {
  display: flex;
  justify-content: center;
}
#app {
  box-sizing: border-box;
  font-family: "consolas", sans-serif;
  margin-top: 60px;
  width: 100%;
  max-width: 460px;
  margin: auto;
  position: relative;
  border-right: 1px solid #eee;
  border-left: 1px solid #eee;
}

</style>
