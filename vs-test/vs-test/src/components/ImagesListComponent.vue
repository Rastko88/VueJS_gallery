<template>  
  <div class="imagesList">

    <div class="errorMessage">{{ errorMessage }}</div>

    <div class="imagesPerPage">
      <p id="perPageTitle">Items per page:</p>
      <p class="pageItems active" id="18" v-on:click="changeItemsPerPage">18</p>
      <p class="pageItems" id="36" v-on:click="changeItemsPerPage">36</p>
      <p class="pageItems" id="54" v-on:click="changeItemsPerPage">54</p>
      <p class="pageItems" id="100" v-on:click="changeItemsPerPage">100</p>
    </div>
    <input type="text" class="inputField" v-model="search" placeholder="Search by author">

    <ul class="list">
      <li v-for="image in filteredImages" v-bind:key="image.id">
        <a v-bind:href="image.download_url" target="_blank">
          <img v-bind:src="image.download_url" alt="">
          <p class="authorName">{{ image.author }}</p>        
          <p class="imageId"># {{ image.id }}</p>
        </a>        
      </li>
    </ul>

    <!-- <div class="iframeHolder">
      <div>
        <button v-on:click="closeIframe">Close</button>
      </div>
      <iframe v-bind:src="currentImage" name="iframeImg" id="iframe"></iframe>
    </div> -->

    <p class="madeBy">by Rastko Prodanovic</p>
  </div>
</template>

<script>

import axios from 'axios';
import { bus } from '../main';

export default {
  name: 'imageList',

  data() {
    return {
      currentPage: 1,
      itemsPerPage: 18,
      errorMessage: "",
      images: [],
      search: "",
      //currentImage: ""
    }
  },  

  methods: {
    getImages(page, limit) {

      let _this = this;
      
      axios.get('https://picsum.photos/v2/list?page=' + page +'&limit=' + limit)
        .then((res) => {
          _this.images = res.data; 
        })
        .catch((err) => {
          _this.errorMessage = err;          
        })
    },

    changeItemsPerPage(e) {
      e.preventDefault();
      document.getElementsByClassName('active')[0].classList.remove("active");
      e.target.classList.add("active");
      this.itemsPerPage = e.target.id;
      this.getImages(this.currentPage, this.itemsPerPage);
    },

    // openImage(e) {
    //   //this.currentImage = "";

    //   let iframeDiv = document.getElementsByClassName("iframeHolder")[0];      
    //   this.currentImage = e.target.currentSrc;
    //   iframeDiv.style.display = "block";
    // },

    // closeIframe() {
    //   let iframeDiv = document.getElementsByClassName("iframeHolder")[0];    
    //   iframeDiv.style.display = "none";
    //   this.currentImage = "";
    // }
  },

  computed: {
    filteredImages() {
      return this.images.filter((image) => {
        return image.author.match(this.search);
      })
    }
  },

  mounted() {
    let inputField = document.getElementsByClassName("inputField")[0];
    let imagesPerPageDiv = document.getElementsByClassName("imagesPerPage")[0];
    let madeByDiv = document.getElementsByClassName("madeBy")[0]; 
    
    if(this.errorMessage !== "") {
      inputField.style.display = "none";
      imagesPerPageDiv.style.display = "none";
      madeByDiv.style.display = "none";      
    }    
  },  

  created() {
    this.getImages(this.currentPage, this.itemsPerPage);
    
    bus.$on('pageNumberChanged', (data) => {
      this.currentPage = data;

      this.getImages(data, this.itemsPerPage);
    });    
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

  .imagesList {
    position: relative;
    background-color: #44707B;
    min-height: 50vh;
  }  

  .errorMessage {
    color: red;
    font-size: 1.2em;
    padding-top: 10px;
    height: 30px;
    width: 300px;
    background-color: rgba(68,112,123,0);

    position: absolute;
    left: calc(50% - 150px);
  }

  .imagesPerPage {
    height: 250px;
    width: 100px;;
    border: 1px solid #FFBF00;
    border-radius: 8px;
    background-color: rgba(65, 116, 129, .1);
    position: absolute;
    top: 150px;
    left: 100px;
  }

  input {
    border: 1px solid #FFBF00;
    border-radius: 8px;
    background-color: rgba(65, 116, 129, .1);
    width: 200px;
    height: 20px;
    padding: 6px;
    position: absolute;
    top: 50px;
    left: 100px;
  }

  .pageItems {
    padding: 8px;
    color: #000;
    font-size: 1em;
    cursor: pointer;
  }

  .pageItems:hover {
    color: #FFBF00 ;
  }

  .active {
    color: #FFBF00 ;
  }

  #perPageTitle {
    font-size: 0.7em;
    color: #000;
    text-decoration: underline;
  }
  
  .list {
    list-style: none;
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: center;
    align-items: center;

    margin: 0 auto;
    padding: 100px 200px 200px 200px;
    background: rgb(255,255,255);
    background: linear-gradient(180deg, rgba(255,255,255,1) 0%, rgba(162,196,197,1) 35%, rgba(68,112,123,1) 100%);
  }

  li {
    width: 367px;
    margin: 16px;
    border-radius: 8px;
    box-sizing: border-box;
    background-color: whitesmoke;
    text-align: center;

    -webkit-box-shadow: 6px 9px 23px -1px rgba(0,0,0,0.57);
    -moz-box-shadow: 6px 9px 23px -1px rgba(0,0,0,0.57);
    box-shadow: 6px 9px 23px -1px rgba(0,0,0,0.57);
  }

  a, p {
    color: #2c3e50;
    text-decoration: none;
    font-size: 1em;

  }  

  img {
    border-top-right-radius: 8px;
    border-top-left-radius: 8px;
    width: 367px;
    height: 267px;
  }  

  .authorName {
    text-overflow: ellipsis;
  }

  /* .iframeHolder {
    display: none;
    position: absolute;    
    width: 80vw;
    height: 80vh;
    background-color: #FFBF00;
  }

  iframe {
    width: 100%;
    height: 100%;    
  } */

  .madeBy {
    color: antiquewhite;
    background-color: #44707B;
    font-size: .6em;
    padding-bottom: 20px;
    text-align: center;
  }

  @media (min-width:1280px) and (max-width:1366px),{
    li {
      width: 183px;;
      margin: 8px;      
    }

    a, p {      
      font-size: 1em;
    }  

    img {      
      width: 183px;
      height: 133px;
    }

    .imagesPerPage {
      height: 200px;
      width: 80px;;
      top: 110px;
      left: 50px;
    }

    .pageItems {
      padding: 6px;
      font-size: 0.8em;
    }

    input {
      width: 140px;
      height: 20px;
      padding: 3px;
      top: 50px;
      left: 50px;
      font-size: 0.8em;
    }
  }  

  @media (max-height:768px) and (max-width:1024px){
    li {
      width: 183px;;
      margin: 8px;      
    }

    a, p {      
      font-size: 0.9em;
    }  

    img {      
      width: 183px;
      height: 133px;
    }

    .imagesPerPage {
      height: 180px;
      width: 60px;;
      top: 110px;
      left: 50px;
    }

    .pageItems {
      padding: 6px;
      font-size: 0.6em;
    }

    input {
      width: 140px;
      height: 15px;
      padding: 3px;
      top: 50px;
      left: 50px;
      font-size: 0.6em;
    }
  }

  @media (max-width:768px) and (max-height:1024px){
    .list {      
      padding: 250px 80px 100px 80px;      
    }
    li {
      width: 183px;;
      margin: 8px;       
    }

    a, p {      
      font-size: 0.9em;
    }  

    img {      
      width: 183px;
      height: 133px;
    }

    .imagesPerPage {
      height: 200px;
      width: 60px;;
      top: 30px;
      left: 30px;
    }

    .pageItems {
      padding: 6px;
      font-size: 0.8em;
    }

    input {
      width: 140px;
      height: 15px;
      padding: 3px;
      top: 30px;
      left: 70vw;
      font-size: 0.8em;
    }
  }

  @media (max-width:360px) and (max-height:720px){
    .list {      
      padding: 250px 60px 100px 60px;      
    }
    li {
      width: 183px;;
      margin: 8px;      
    }

    a, p {      
      font-size: 0.9em;
    }  

    img {      
      width: 183px;
      height: 133px;
    }

    .imagesPerPage {
      height: 200px;
      width: 60px;;
      top: 30px;
      left: 20px;
    }

    .pageItems {
      padding: 6px;
      font-size: 0.7em;
    }

    input {
      width: 140px;
      height: 15px;
      padding: 3px;
      top: 30px;
      left: 50vw;
      font-size: 0.7em;
    }
  }


</style>
