<template>
  <div class="user-profile">
    <div class="user-profile_user-panel">
      <h1 class="user-profile_username">@{{user.username}}</h1>
      <div class="user-profile-badge" v-if="user.isAdmin">
        Admin
      </div>
      <div>
        <strong>Followers:</strong>{{followers}}
      </div>
      <form class="user-profile-create-post" @submit.prevent="createNewPost" :class="{'exceeded': newPostCharCount > 180}">
        <label for="newPost"><strong>New Post</strong>({{newPostCharCount}}/180)</label>
        <textarea id="newPost" rows="4" v-model="newPostContent"/>
        <br>
        <div class="user-profile-post-type">
          <label for="postType"><strong>Post Type</strong></label>
          <select id="postType" v-model="selectedPostType">
            <option :value="option.value" v-for="(option, index) in postTypes" :key="index">
              {{option.name}}
            </option>
          </select>
        </div>
<br>
        <button>
          Post!
        </button>
      </form>
    </div>
    <div class="user-profile-posts-wrapper">
      <PostItem
          v-for="post in user.posts"
          :key="post.id"
          :username="user.username"
          :post="post"
          @favourite="toggleFavourite"
      />
    </div>
  </div>
</template>

<script>
// import {reactive} from "vue";
import PostItem from "./PostItem";
export default {
  name: 'UserProfile',
  components: {PostItem},
  data(){
    return {
      newPostContent: '',
      selectedPostType: 'instant',
      postTypes:[
        {value:'draft', name:'Draft'},
        {value:'instant', name:'Instant Post'}
      ]
      ,
      followers: 0,
      user:{
        id:1,
        username:'BlackT',
        firstName:'Thabiso',
        lastName:'Ngubane',
        email:'thabisongubane1992@gmail.com',
        isAdmin: true,
        posts:[
          {id:1, content:'My first post'},
          {id:2, content:'My second post testing'},
        ]
      }
    }
  },
  watch: {
    followers(newFollowerCount, oldFollowerCount){
      if(oldFollowerCount < newFollowerCount){
        console.log(`${this.user.username} has gained a follower!`);
      }
    }
  },
  computed: {
    newPostCharCount(){
      return this.newPostContent.length;
    }
  },
  methods: {
    followUser(){
      this.followers ++
    },
    toggleFavourite(id){
        console.log(`Favoured post #${id}`);
    },
    createNewPost(){
      if(this.newPostContent && this.selectedPostType !=='draft'){
        this.user.posts.unshift({
          id:this.user.posts.length + 1,
          content: this.newPostContent
        })
        this.newPostContent = '';
      }
    }
  },
  mounted() {
    this.followUser()
  }
}
</script>

<style scoped>
.user-profile {
  display: grid;
  grid-template-columns: 1fr 3fr;
  width: 100%;
  padding: 50px 5%;
}


.user-profile_user-panel{
  display: flex;
  flex-direction: column;
  margin-right: 50px;
  padding: 20px;
  background-color: white;
  border-radius: 5px;
  border: 1px solid #dfe3e8;
  max-height: 250px;
}

h1{
  margin: 0;
}

.user-profile-badge{
  background : rebeccapurple;
  color: white;
  margin-right: auto;
  border-radius: 5px;
  padding: 3px 10px;
  font-weight: bold;
}

.user-profile-create-post{
  display: flex;
  flex-direction: column;
  padding: 10px 0;
}

.exceeded{
  color: red;
}
</style>
