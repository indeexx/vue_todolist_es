<template>
  <div class="index">
    <a href="http://www.indeex.cc">
      <img src="../assets/logo.png" class="logo">
    </a>
    <div class="container">
      <div class="submit">
        <input type="text" v-model="inputValue" placeholder="Your add content">
        <button @click="onClickHandler">add</button>
      </div>
      <div class="seacher">
        <input type="text" v-model="seachValue" placeholder="Your seach keyword">
        <button @click="seachClickHandler">seach</button>
      </div>
      <div class="other">
        <button @click="sortClickHandler">a-z(chinese)</button>
        <button @click="timeClickHandler">time</button>
      </div>
      <ul>
        <Li v-for="(item, index) in list" v-bind:key="index" v-bind:index="index" v-bind:item="item" v-bind:delClick="delClickHandler" v-bind:modifyClick="modifyClickHandler" v-bind:modifyValue="modifyValue"></Li>
      </ul>
    </div>

  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";
import Li from "@/components/List.vue";

@Component({
  components: {
    Li
  }
})
export default class Index extends Vue {
  private list: object[] = [];
  private inputValue: string = "";
  private id: number = 0;
  private msg: string = "Hello Indeex";
  private modifyValue: string = "";
  private seachValue: string = "";
  private sortB: boolean = false;
  @Prop() private num!: number;

  private mounted(): void {
    console.log("mounted!");
    this.list = this.getList();
  }
  private onClickHandler(e: object): void {
    this.list = this.getList();
    this.id = this.list.length || 0;
    this.id =  this.id + 1;
    let obj: object = {
      id: this.id,
      timer: new Date().getTime(),
      title: this.inputValue
    };
    this.list.push(obj);
    this.inputValue = "";
    this.seachValue = "";
    window.localStorage.setItem("list", JSON.stringify(this.list));
  }
  private delClickHandler(index: number): void {
    this.list = this.getList();
    this.list.splice(index, 1);
    window.localStorage.setItem("list", JSON.stringify(this.list));
  }
  private modifyClickHandler(value: string, index: number): void {
    this.list.forEach((item: any, _index: number) => {
      if (_index === index) {
        item.title = value;
      }
    });
    window.localStorage.setItem("list", JSON.stringify(this.list));
  }

  private seachClickHandler(e: any): void {
    let list: object[] = this.getList();
    if (this.seachValue.length > 0) {
      this.list = list.filter((item: any, index: number) => {
        // if(item.title === this.seachValue){//精确搜索
        if (item.title.indexOf(this.seachValue) > -1) {
          return true;
        } else {
          return false;
        }
      });
    } else {
      this.list = list;
    }
  }
  private getList():object[] {
    let list: object[] = JSON.parse(
      window.localStorage.getItem("list") || "[]"
    );
    return list;
  }
  private sortClickHandler(e: any): void{
    this.sortB = !this.sortB;
    this.list = this.getList().sort((a: any, b:any): any => {
      if(this.sortB) return a.title.localeCompare(b.title,'zh');
      else b.title.localeCompare(a.title,'zh');
    });
  }
  private timeClickHandler(e: any): void{
    this.sortB = !this.sortB;
    this.list = this.getList().sort((a: any, b:any): any => {
      if(!this.sortB) return a.timer - b.timer;
      else return b.timer - a.timer;
    });
  }
}
</script>

<style lang="scss">
.index {
  input {
    &:focus {
      border: 1px solid #5fa1f1;
    }
    width: 100%;
    height: 2rem;
    border-radius: 5px;
    outline: none;
    border: 1px solid #cccccc;
    padding-left: 1rem;
  }
  button {
    &:hover {
      background: #66b1ff;
    }
    width: 100%;
    height: 2.1rem;
    border: none;
    background-color: #409eff;
    border-radius: 5px;
    color: #ffffff;
    cursor: pointer;
  }
  width: 100%;
  height: 100%;
  padding-top: 2rem;
  .logo {
    height: 2rem;
  }
  .container {
    text-align: center;
    width: 60%;
    margin: 0 auto;
    margin-top: 1rem;
    .seacher {
      display: inline-block;
      margin-top: 1rem;
      text-align: left;
      width: 50%;
      height: 100%;
      input {
        width: 60%;
      }
      button {
        width: 20%;
        margin-left: 0.5rem;
      }
      // ::-webkit-input-placeholder {
      //   color: #17918b;
      // }
      // :-moz-placeholder {
      //   color: #17918b;
      // }
      // ::-moz-placeholder {
      //   color: #17918b;
      // }
      // :-ms-input-placeholder {
      //   color: #17918b;
      // }
    }
    .submit {
      @extend .seacher;
    }
    .other{
      text-align: left;
      margin-top: 1rem;
      button{
        width: 20%;
        &:nth-child(2){
          margin-left: 1rem;
        }
      }
    }

    ul {
      position: relative;
      margin: {
        top: 2rem;
        right: 2rem;
        bottom: 2rem;
        left: 0rem;
      }
      padding: 0;
      list-style: none;
      text-align: center;
      border-top: 1px solid #67c23a;
      font-size: 0;
      li {
        display: inline-block;
        width: 100%;
        min-height: 1.5rem;
        font-size: 16px;
        overflow: hidden;
        border-bottom: 1px solid #cccccc;
        padding: 0.5rem 0rem;
      }
      button {
        display: inline-block;
        position: absolute;
        transform: translate(-100%, 10%);
        width: 15%;
        height: 2rem;
        color: #ffffff;
        border-radius: 5px;
        border: none;
        background-color: #f56c6c;
        &:hover {
          background-color: #f78989;
        }
      }
      .li {
        .modify {
          display: inline-block;
          width: 100%;
          input {
              width: 75%;
            }
          .btns {
            position: relative;
            display: inline-block;
            width: 20%;
            left: 10%;
            button {
              display: inline-block;
              position: relative;
              width: calc(50% - .5rem);
              margin-left: 0.5rem;
              top: -.2rem;
            }
          }
        }
      }
    }
  }
}
</style>
