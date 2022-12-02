<script setup>
import CommentItem from "../components/CommentItem.vue"
import NewComment from "../components/NewComment.vue"

const props = defineProps({
  post: {
    type: Object,
    required: true,
  },
  count: {
    type: Number,
  }
});
</script>

<template>
  <div class="box">
   <span class="body">body post</span> :  {{post.body}}
    <h4 class="title">comments ({{count}})</h4>
    
    <h4 class="title">comments </h4>
    <CommentItem  :comment="post.comments" />

    <NewComment :id="post.id" :post="post" :count="count"/>
    
  </div>
</template>

<style scoped>
  .box {
    border: 1px solid grey;
    padding: 5px ;
    margin:  5px ;
    border-radius: 5px ;
  }
  .title {
    font-style: italic ;
    color: hsla(160, 100%, 37%, 1);
  }
  .body {
    color: rgb(193, 6, 162);
  }
</style>


