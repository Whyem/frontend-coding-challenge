<template>
  <div v-for="(repo, index) in repos" :key="repo.id">
    <Repos  
    :img="repo.owner.avatar_url" 
    :name="repo.name" 
    :description="repo.description" 
    :stars="repo.stargazers_count" 
    :issues="repo.open_issues_count"
    :username="repo.owner.login"
    :userlink="repo.owner.url" 
    :index = "index"/>
  </div>
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
        }
    },  
  mounted () {
    axios
      .get('https://api.github.com/search/repositories?q=created:%3E'+this.getFormattedDate()+'&sort=stars&order=desc')
      .then( response => {
        this.repos = response.data.items;
        console.log(this.repos)
      })
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
  padding: 60px 0 60px 0;
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
</style>
