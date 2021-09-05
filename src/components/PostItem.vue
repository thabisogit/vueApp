<template>
  <div class="post-item" @click="favouritePost(post.id)">
    <div class="user-profile-post">
        <div class="post-item-user">
            @{{username}}
        </div>
        <div class="post-item-content">
          {{post.content}}
        </div>
    </div>
  </div>

</template>

<script>
export default {
  name: "PostItem",
  props:{
    username:{
      type:String,
      required:true
    },
    post:{
      type:Object,
      required: true
    }
  },
  methods:{
    favouritePost(postId){
      this.$emit('favourite',postId)
    }
  }
}
</script>

<style scoped>
.post-item{
  padding: 20px;
  background-color: white;
  border-radius: 5px;
  border: 1px solid #dfe3e8;
  box-sizing: border-box;
  cursor: pointer;
  transition: all 0.25s ease;
  width: 80%;
}

.post-item:hover {
  transform: scale(1.1, 1.1);
}

.post-item-user{
  font-weight: bold;
}
</style>