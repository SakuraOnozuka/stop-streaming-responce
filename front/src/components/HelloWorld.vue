<template>
  <div class="home">
    <img :src="src" alt="video image">
    <button @click="click">click</button>
  </div>
</template>
<script lang="ts">
import { Component, Vue, Prop } from 'vue-property-decorator';
import HelloWorld from "@/components/HelloWorld.vue";
import http from "http";

@Component({
  components:{
    HelloWorld
  }
})

export default class Home extends Vue {

  @Prop({ default:false })
  stop!: boolean;

  click(): void {
    this.$emit("click")
  }
  src = ''
  retrieveFrame(buff: any) {
    console.log('retrieveFrame')
    const boundary = new Buffer("--frame");
    const index = buff.indexOf(boundary);

    //The currently read buff has not reached the split point
    if (index === -1) {
      return buff;
    }

    //Divide the frame by boundary
    const frame = buff.slice(0, index);
    this.src = "data:image/jpeg;base64," + frame.toString()

    //Keep the rest of the parsed frame
    //For next analysis
    return buff.slice(index + boundary.length);
  }

  created() {
    const req = http.request("http://localhost:9001/video_feed", res => {
      let cache = new Buffer(0);
      res.on("data", part => {
        cache = Buffer.concat([cache, part]);
        cache = this.retrieveFrame(cache);
        if(this.stop === true){
          console.log('cancel')
          req.abort()
        }
      });
    });

    req.end();
  }
}
</script>
