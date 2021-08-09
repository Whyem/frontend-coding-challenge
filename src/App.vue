<template>

  <div>
    <div v-if="err" class="error">There seems to be a problem. Please try again later!</div>

    <div v-if="isLoading" class="spinner_container">
      <div class="semipolar-spinner" :style="spinnerStyle">
        <div class="ring"></div>
        <div class="ring"></div>
        <div class="ring"></div>
        <div class="ring"></div>
        <div class="ring"></div>
      </div>
    </div>
    

    <a v-for="(repo, index) in repos" :key="repo.id" class="card_link" :href="repo.html_url">
      <Repos  
      :img="repo.owner.avatar_url" 
      :name="repo.name" 
      :description="repo.description" 
      :stars="repo.stargazers_count" 
      :issues="repo.open_issues_count"
      :username="repo.owner.login"
      :userlink="repo.owner.html_url" 
      :index = "index"/>
    </a>

  </div>

  <div v-if="page > 34" class="nores">No more results!</div>
  
</template>

<script>
import Repos from './components/Repos.vue'
import axios from 'axios'


export default {
  name: 'App',
  components: {
    Repos
  },
  data () {
    return {
      repos: [],
      isLoading: true,
      page: 1,
      err: false
    }
  },
  
  methods: {
    getFormattedDate() {
      var date = new Date();
      date.setDate(date.getDate() - 30);
      let year = date.getFullYear();
      let month = (1 + date.getMonth()).toString().padStart(2, '0');
      let day = date.getDate().toString().padStart(2, '0');
    
      return year + '-' + month + '-' + day;
    },

    getRepos(page){
      axios
        .get('https://api.github.com/search/repositories?q=created:%3E'+this.getFormattedDate()+'&sort=stars&order=desc&page='+page.toString())
        .then( response => {
          this.err = false;
          this.repos = this.repos.concat(response.data.items);
          this.isLoading = false;
          this.page += 1;
        }).catch(error=>{
          console.log(error);
          this.err = true;
        })
    },
      
    onScroll() {
      let bottomOfWindow = Math.floor(Math.max(window.pageYOffset, document.documentElement.scrollTop, document.body.scrollTop) + window.innerHeight) === document.documentElement.offsetHeight
      
      if(this.page <= 34){
        if (bottomOfWindow) {
          this.isLoading = true;
          setTimeout(() => this.getRepos(this.page) , 1000);
        }
      }
    },

  },  
  
  created() {
    window.addEventListener('scroll', this.onScroll);
  },

  mounted () {
    this.getRepos(this.page)
  },

  unmounted() {
    window.removeEventListener('scroll', this.onScroll);
  }
}
</script>

<style>
html {
  font-size: 100%;
  box-sizing: border-box;
}

*, *::after, *::before {
  box-sizing: border-box;
}

body {
  padding: 0;
  margin: 0;
  font-size: 12px;
  font-weight: 400;
  min-height: 100vh;
  font-family: 'Spartan', sans-serif;
  background-image: url("./assets/git.png");
  background-repeat: space;
  background-size: 35px;
  position: relative;
  line-height: 1;
  padding: 60px 0 0 0;
  text-align: center;
}

@media (max-width: 768px) {
  body {
    padding: 60px 10px 80px 10px;
  }
}

@media (min-width: 768px) {
  .fd-bm {
    display: flex;
  }
}

@media (max-width: 768px) {
  .fd-bm {
    display: block;
  }
}

.flex {
  display: flex;
}

.flex--jc {
  justify-content: center;
}

.flex--jcfs {
  justify-content: flex-start;
}

.flex--jcfe {
  justify-content: flex-end;
}

.flex--jsb {
  justify-content: space-between;
}

.flex--jsa {
  justify-content: space-around;
}

.flex--ai {
  align-items: center;
}

h1, h2, p, small {
  margin: 0;
}

.spinner_container{
  text-align: center;
  background-color:#000000e8;
  padding: 10px 0;
  position: fixed;
  bottom: 0;
  width: 100%
}

.error{
  text-align: center;
  background-color:#d1002d;
  color: white;
  padding: 10px 0;
  position: fixed;
  top: 0;
  width: 100%
}

.nores{
  text-align: center;
  background-color:#000000;
  color:white;
  font-size:20px;
  font-weight:500;
  padding: 10px 0;
  margin-top: 50px;
  width: 100%
}

.card_link{
  display: block;
}


.semipolar-spinner, .semipolar-spinner * {
      box-sizing: border-box;
    }

    .semipolar-spinner {
      height: 65px;
      width: 65px;
      position: relative;
      margin: 0 auto;
    }

    .semipolar-spinner .ring {
      border-radius: 50%;
      position: absolute;
      border: calc(65px * 0.05) solid transparent;
      border-top-color: #ff1d5e;
      border-left-color: #ff1d5e;
      animation: semipolar-spinner-animation 2s infinite;
    }

    .semipolar-spinner .ring:nth-child(1) {
      height: calc(65px - 65px * 0.2 * 0);
      width: calc(65px - 65px * 0.2 * 0);
      top: calc(65px * 0.1 * 0);
      left: calc(65px * 0.1 * 0);
      animation-delay: calc(2000ms * 0.1 * 4);
      z-index: 5;
    }

    .semipolar-spinner .ring:nth-child(2) {
      height: calc(65px - 65px * 0.2 * 1);
      width: calc(65px - 65px * 0.2 * 1);
      top: calc(65px * 0.1 * 1);
      left: calc(65px * 0.1 * 1);
      animation-delay: calc(2000ms * 0.1 * 3);
      z-index: 4;
    }

    .semipolar-spinner .ring:nth-child(3) {
      height: calc(65px - 65px * 0.2 * 2);
      width: calc(65px - 65px * 0.2 * 2);
      top: calc(65px * 0.1 * 2);
      left: calc(65px * 0.1 * 2);
      animation-delay: calc(2000ms * 0.1 * 2);
      z-index: 3;
    }

    .semipolar-spinner .ring:nth-child(4) {
      height: calc(65px - 65px * 0.2 * 3);
      width: calc(65px - 65px * 0.2 * 3);
      top: calc(65px * 0.1 * 3);
      left: calc(65px * 0.1 * 3);
      animation-delay: calc(2000ms * 0.1 * 1);
      z-index: 2;
    }

    .semipolar-spinner .ring:nth-child(5) {
      height: calc(65px - 65px * 0.2 * 4);
      width: calc(65px - 65px * 0.2 * 4);
      top: calc(65px * 0.1 * 4);
      left: calc(65px * 0.1 * 4);
      animation-delay: calc(2000ms * 0.1 * 0);
      z-index: 1;
    }

    @keyframes semipolar-spinner-animation {
      50% {
        transform: rotate(360deg) scale(0.7);
      }
    }
</style>
