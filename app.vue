<template>
  <div>
    <div class="container">
      <video ref="video" autoplay></video>
    <button @click="capturePhoto" style="margin-top: 4%;">Capture Photo</button>
    </div>
   <div class="capture">
    <canvas ref="canvas" style="display: none;"></canvas>
    <img :src="photo" v-if="photo"/>
   </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      photo: null,
    };
  },
  mounted() {
    this.initCamera();
  },
  methods: {
    async initCamera() {
      try {
        const stream = await navigator.mediaDevices.getUserMedia({ video: true });
        this.$refs.video.srcObject = stream;
      } catch (error) {
        console.error("Error accessing camera: ", error);
      }
    },
    capturePhoto() {
      const canvas = this.$refs.canvas;
      const video = this.$refs.video;
      const context = canvas.getContext('2d');
      
      canvas.width = video.videoWidth;
      canvas.height = video.videoHeight;
      context.drawImage(video, 0, 0, canvas.width, canvas.height);
      
      this.photo = canvas.toDataURL('image/png');
    }
  }
};
</script>

<style>
.container{
  width: 100%;
  height: 400px;
  border: 2px solid red;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column
}
video{
  width: 50%;
  height: 50%;
}
.capture{
  width: 100%;
  height: 400px;
  border: 2px solid red;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column
}
img{
  width: 500px;
  height: 500px;
}
</style>
