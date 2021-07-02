<template>
  <div class="about">
    <button @click="click">click</button>
  </div>
</template>
<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
@Component({
})
export default class About extends Vue {
  click():void{
    this.$router.push('/')
  }
}
</script>