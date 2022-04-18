<template>
  <div class="home">
    <label for="choose-img">
      <input type="file" id="choose-img" @change="setImageView">
    </label>
    <canvas id="canvas" ref="canvasEle" />
    <img :src="base64" alt="">
  </div>
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component';
import HelloWorld from '@/components/HelloWorld.vue';
import toBase64 from '@/utils/toBase64';
import loadImage from '@/utils/loadImage';

@Options({
  components: {
    HelloWorld,
  }
})

export default class HomeView extends Vue {
  canvasContext: CanvasRenderingContext2D | undefined | null;

  declare $refs: {
    canvasEle: HTMLCanvasElement
  }

  declare $data: {
    base64: string;
  }

  data() {
    return {
      base64: ''
    }
  }

  mounted() {
    console.log(this);
    console.log('vue mounted', this.$refs.canvasEle);
    this.canvasContext = this.$refs.canvasEle.getContext('2d');
    console.log(this.canvasContext)

  }

  async setImageView(e: any) {
    console.log(e.target.files)
    this.$data.base64 = await toBase64(e.target.files[0]);
    const image = await loadImage(this.$data.base64);
    this.$refs.canvasEle.width = image.width;
    this.$refs.canvasEle.height = image.height;
    this.canvasContext?.drawImage(image, 0, 0);
  }
}
</script>
