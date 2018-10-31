<template>
<div class="video-player">
  <div class="video-container">
   
   <iframe width="640" height="360" :src="this.activeVideo.youtubeURL" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
          <h3>{{this.activeVideo.title}}</h3>
        <div class="row">
        <p>{{this.activeVideo.views}} views</p> 
        <p>{{this.activeVideo.likes}} <button @click="addLike">Like</button></p> 
        </div>
  </div>
    <div class="video-list">
      <div   @click="chooseVideo(video)" :key="video.id" v-for="video in videos" class="thumbnail">
          <div class="thumbnail-img">
            <img :src="video.thumbnail" />
          </div>
          <div class="thumbnail-info">
            <h3>{{video.title}}</h3>
            <p>{{video.creator}}</p>
            <p class="thumbnail-views">{{video.views}} Views</p>
          </div>
      </div>
  </div>
</div>
</template>

<script>
import axios from "axios";
let videos = "";
let activeVideo = "";

export default {
  name: 'VideoPlayer',
  data () {
    return {
      videos,
     activeVideo
    }
  },
  methods: {
    chooseVideo (video){
        this.activeVideo = video;
        video.views += 1;
    },
    addLike(){
      this.activeVideo.likes += 1;
    }

  },
  created() {
    const url = `https://gist.githubusercontent.com/gizmo49/ddf6677e7f0e3efc6069b3793a64fd39/raw/ad8ca3ba35af8e5d07ac9d0773ed18e3326781fc/jsondb`;
     axios.get(url)
    .then(response => {
     
     this.videos = response.data.videos;
     this.activeVideo = this.videos[0];
      //console.log(this.activeVideo);

    })
    .catch(e => {
     // this.errors.push(e)
     console.log(e);
    })


  }
   
}

</script>

<style scoped>
.thumbnail{
    display:flex;
}
.thumbnail:hover {
  cursor: pointer;
}
.thumbnail img{
    width:168px;
}

.thumbnail-info{
    margin-left:20px;
}

.thumbnail h3{
    font-size:16px;
}

h3,
p{
    margin:0;
    padding:0;
}

.thumbnail-views{
    font-size:14px;
}

.video-player{
    display:flex;
    width:1200px;
    margin:auto;
}

.video-container{
    margin-right:40px;
}

.row{
    display:flex;
    justify-content:space-between;
}

button{
    background:#D0021B;
    color:white;
    border:none;
    padding:10px 20px;
}
button:hover {
  cursor: pointer;
}
</style>