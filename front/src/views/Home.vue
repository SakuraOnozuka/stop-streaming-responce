<template>
  <div class="home">
    <img src="http://localhost:9001/video_feed">
    <button @click="click">click</button>
  </div>
</template>
<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import axios from 'axios'
const CancelToken = axios.CancelToken;
@Component({
})
export default class Home extends Vue {
  click():void{
    axios.get(`http://localhost:9001/video_feed`, {
      cancelToken: CancelToken.source().token
    })
    CancelToken.source().cancel('Operation canceled by the user.');
    this.$router.push('/about')
  }
}
</script>
