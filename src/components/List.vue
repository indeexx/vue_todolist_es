<template>
  <div class="li">
    <li @click="modifyClickHandler(index)">
      {{item.title}}
      <div class="modify" v-if="!hideClass">
        <input type="text" v-model="modifyValue">
        <div class="btns">
          <button class="confim" @click.stop="confim">ok</button>
          <button class="cancel" @click.stop="cancel">cancel</button>
        </div>
      </div>
    </li>

    <button @click="delClick(index)" v-if="hideClass">delete</button>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";

@Component
export default class List extends Vue {
  @Prop() private item!: any;
  @Prop() private delClick!: Function;
  @Prop() private modifyClick!: Function;
  @Prop() private modifyConfimClick!: Function;
  @Prop() private modifyCancelClick!: Function;
  @Prop() private index!: number;
  private modifyValue: string = this.item.title;
  private hideClass: boolean = true;

  private modifyClickHandler(index: number): void {
    this.hideClass = false;
  }
  private confim(e: any): void {
    this.modifyClick(this.modifyValue, this.index);
    this.cancel(null);
  }
  private cancel(e: any = null): void {
    this.hideClass = true;
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.li {
  text-align: left;
}
</style>
