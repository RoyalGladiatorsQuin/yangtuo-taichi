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
          {{introText}}
      </div>
      <div class="search-result">
          <textarea id="query" v-model="editingNote">

        </textarea>
        <div class="button" @click="query">
            <p>查询详情</p>
        </div>
        <div class="error" v-if="errorResponse">
          {{errorMsg}}
        </div>
        <div class="result">
            <div class="result-item" v-for="items in results" :key="items">
              <div class="result-title">
                <div class="result-text">{{items.title}}</div>
              </div>
              <div class="result-content">
                <div class="pair-qa" v-for="qas in items.qa" :key="qas">
                  <div class="content-text" v-html="heightLight(qas.q)"></div>
                  <div class="content-text" v-html="heightLight(qas.a)"></div>
                </div>
              </div>
              <div class="teleport" @click="to"><a :href="items.link">查看详情</a></div>
            </div>
        </div>
      </div>

    </div>

  </div>
</template>


<script>
import axios from 'axios'
export default {
  name: "Entry",
  data:function(){
    return{
      mainTitle: "羊驼打过的太极",
      subtitle:"看看羊驼都说了什么",
      results:[],
      introDisplay: true,
      editingNote:"",
      note: "",
      response: null,
      introText : "可以方便的查询羊驼在QA中答应的事情，最短三个字最长二十个字哦。遇到任何问题欢迎私信B站\n@ProjectASF",
      errorResponse: false,
      errorMsg:"没找到相应的QA哦"
    }
  },
  methods:{
    editNote(){
      this.note=this.editingNote;
      this.note.replace(/\r\n/g,"")
      this.note = this.note.replace(/\n/g,"");
      this.note = this.note.replace(/ /g,"")
      console.log(this.note)
    },
    fold(){
      this.introDisplay = !this.introDisplay
      if (this.introDisplay){
        this.$refs.intro.style.display = "none";
      }else{
        this.$refs.intro.style.display = "";
      }
    },
    query(){
      this.editNote();
      if(this.note.length < 3){
        this.errorResponse = true
        this.errorMsg = "QA太短了哦"
        this.results = []
        return
      }
      axios.get('http://43.135.162.140:8080/v1/spider/find?key='+ this.note).then((response)=>{
        if (response.data.code === 0){
          console.log(response)
          this.results = response.data.Data
          this.errorResponse = false
          return
        }
        this.results = []
        this.errorResponse = true
        this.errorMsg = "没找到对应的QA哦"
        console.log(response.data.status)
      })
    },
    heightLight(val){
      if(val.includes(this.note) && this.note !== ''){
        return val.replace(this.note,'<span style="color: red">'+this.note+'</span>')
      }
      return val
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
  font-size:20px;
  margin: 0 15px;
  padding: 15px
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
.teleport a{
  text-decoration: none;
}

.error{
  justify-content:center;
  text-align: center;
  font-size: 17px;
  color:#AA5555;
}

.error p{
  width:auto;
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