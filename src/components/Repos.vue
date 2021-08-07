<template>
  <div class="repo"  v-bind:class="{ gold: index == 0 , silver: index == 1, bronze: index == 2}">
            <div class="fd-bm flex--ai">
                <div class="repo__img">
                    <img :src="img" class="avatar" alt="avatar">
                </div>
    
                <div class="repo__text">  
                    <div class="text_block">
                        <h1 class="repo__title">{{ name }}</h1>
                        <p class="repo__description">{{ description }}</p>
                    </div>
    
                    <div class="fd-bm flex--ai">
                        <div class="tags flex flex--ai flex--jc">
                            <div class="tag flex flex--ai">
                                <img src="../assets/star.svg" class="tag__icon yellow" alt="star">
                                <p class="tag__text">Stars</p>
                                <p class="tag__number yellow">{{ stars }}</p>
                            </div>
    
                            <div class="tag flex flex--ai">
                                <img src="../assets/issue.svg" class="tag__icon" alt="star">
                                <p class="tag__text">Issues</p>
                                <p class="tag__number">{{ issues }}</p>
                            </div>
                        </div>
    
                        <div class="submitted">
                            Submitted 30 days ago by <a :href="userlink" class="user_link">{{ username }}</a>
                        </div>
                    </div>
                </div>
            </div>
        </div>
</template>

<script>
export default {
  name: 'Repos',
  props: {
    index: Number,
    img: String,
    name: String,
    description: String,
    stars: String,
    issues: String,
    username: String,
    userlink: String,
  },
  data () {
    return {
      star: require('../assets/star.svg'),
      issue: require('../assets/issue.svg')
    }
  }
}
</script>

<style scoped>
.repo {
  background: #F2F2F2;
  padding: 22px 15px 22px 15px;
  border-radius: 5px;
  display: inline-block;
  box-shadow: 0px 5px 9px #8d8d8ddb;
  text-align: left;
  margin-bottom: 26px;
}

@media (max-width: 768px) {
  .repo {
    text-align: center;
    padding: 22px 50px;
    width: 100%;
  }
}

@media (min-width: 768px) {
  .repo {
    width: 650px;
  }
}

@media (max-width: 376px) {
  .repo {
    text-align: center;
    padding: 22px 15px;
    width: 310px;
  }
}

.repo__title {
  text-transform: uppercase;
  margin-bottom: 8px;
  overflow-wrap: anywhere;
}

.repo__img {
  border-radius: 7px;
  margin-right: 36px;
  width: 105px;
}

@media (max-width: 768px) {
  .repo__img {
    margin: 0 auto;
    margin-bottom: 26px;
  }
}

.repo__description {
  font-weight: 500;
  color: #838383;
}

.text_block {
  margin-bottom: 30px;
}

.avatar {
  width: 105px;
  border-radius: 7px;
}

.tag {
  font-weight: bold;
  color: white;
  background: black;
  padding: 9px 7px;
  border-radius: 4px;
}

.tag__icon {
  width: 12px;
  margin-right: 5px;
}

.tag__text {
  margin-right: 15px;
}

.tag:first-child {
  margin-right: 12px;
}

.tag:last-child {
  margin-right: 14px;
}


@media (max-width: 768px) {
  .tags {
    margin: 0 auto;
    margin-bottom: 16px;
  }
}

.yellow {
  color: #FFE532;
}

.user_link {
  color: #EF2D2D;
}

.submitted {
  font-weight: 400;
  color: #838383;
}

.gold{
  background:#FFE532;
}

.silver{
  background: #B8B8B8;
}

.bronze{
  background: #D46900;
}
</style>
