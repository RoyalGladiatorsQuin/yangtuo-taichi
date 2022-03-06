<template>
  <div class="main">
    <div class="titles">
      <h1>{{ mainTitle }}</h1>
      <p>{{subtitle}}</p>
      <div class="fold" @click="fold">
        <p>折叠</p>
      </div>
    </div>
    <div class="textbox">
      <div ref="intro" class="intro">

      </div>
      <div class="search-result">
          <textarea id="query">

        </textarea>
        <div class="button">
            <p>查询详情</p>
        </div>
        <div class="result">
            <div class="result-item" v-for="items in results" :key="items">
              <div class="result-title">
                <div class="result-text">{{items.date}}</div>
              </div>
              <div class="result-content">
                <div class="content-text">{{items.content.q}}</div>
                <div class="content-text">{{items.content.a}}</div>
              </div>
              <div class="teleport" @click="query">查看详情</div>
            </div>
        </div>
      </div>

    </div>

  </div>
</template>


<script>
import axios from 'axios'
import VueAxios from 'vue-axios'
export default {
  name: "Entry",
  data:function(){
    return{
      mainTitle: "羊驼打过的太极",
      subtitle:"看看羊驼都说了什么",
      results:[{date:"每周QA1.12",content:{q:"123",a:"234"}},{date:"每周QA1.12",content:{q:"123",a:"234"}}],
      introDisplay: true
    }

  },
  methods:{
    fold(){
      this.introDisplay = !this.introDisplay
      if (this.introDisplay){
        this.$refs.intro.style.display = "none";
      }else{
        this.$refs.intro.style.display = ""
      }
    },
    query(){
      this.axios.get()
    }
  },
}
</script>

<style scoped>
.main{
  height: 100%;
  padding: 2vh 5.83vw;
  display: flex;
  flex-direction: column;
  overflow: hidden;
}
.titles{
  height: 200px;
  display: flex;
  flex-direction: column;
}
.titles h1{
  font-size:34px;
}
.titles p{
  font-size: 17px;
}

.fold{
  align-self: flex-end;
  height: 30px;
  display: none;
}
.intro{
  background-color:#f3f4f6;
  width: 500px;
  height: 300px;
  margin: 0 15px;
}

.textbox{
  display: flex;
  flex-direction: row-reverse;
  position: relative;
}

.search-result{
  width: 100%;
  overflow: hidden;
  display:flex;
  flex-direction: column;
}

.button{
  background-color:#4b5563;
  height:40px;
  width: 138px;
  margin:5px 0;
  align-self:flex-end;
  display:flex;
  justify-content:center;
  align-items:center;
}
.button p{
  color:white;
}
.search-result textarea{
  width: 100%;
  padding:10px 10px 40px;
  border: 1px solid #d1d5db;
  resize: none;
  outline: none;
  font-size: 18px;
  min-height:260px;
  max-height:600px;
  box-sizing : border-box
}
.search-result textarea:focus{
  border: 1px solid #2d2d2d;
}

.result-item{
  background-color:#f8f8f8;
  display: flex;
  flex-direction: column;
  width: 100%;
  margin: 15px 0;
  overflow:hidden;
}
.result-title{
  margin: 2px 15px;
  font-size: 16px;
}

.result-text{
  margin: 15px 0;
}
.result-content{
  margin: 0 15px;
  font-size: 16px;
}
.content-text{
  margin: 15px 0;
}

.teleport{
  align-self: flex-end;
  width:auto;
  margin: 10px
}




@media screen and (max-width: 780px){
  .titles{
    height: 150px;
  }
  .titles h1{
    margin: 10px 0;
    font-size:34px
  }
  .titles p{
    margin: 10px 0;
    font-size: 17px;
  }
  .textbox{
    flex-direction:column;
  }
  .intro{
    width: 100%;
    margin: 10px 0;
  }
  .fold{
    display: block;
  }
}

</style>