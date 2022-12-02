<script setup>
defineProps({
  comment: {
    type: Object,
    required: true,
  }
});
</script>

<template>
    <div class="comment">
      <p class="commenter" v-for="comment in comment">
        <span class="label">Name : </span><span>{{comment.name}}</span>
        <span class="label">Stars : </span><span>{{comment.star}}</span>
        <span class="label">Date : </span><span>{{comment.cmtdate.toDate()}}</span>
        <p ><span class="label">comment</span> :{{comment.comment}}</p>
      </p>
      
    </div>
</template>

<style scoped>
  .comment {
    margin-left: 10px ;
    padding: 5px ;
  }
  .commenter {
    display: grid ;
    grid-template-columns: 1fr 1fr 1fr 1fr 1fr 5fr ;
    font-size: smaller ;
    font-style: italic ;
    border: 0.5px solid grey;
    border-radius: 5px ;
    padding: 5px ;
    margin:  12px ;
    margin-top: 5px;
    margin-bottom: 15px;


  }
  .content {
    background-color: rgb(100,100,100) ;
  }
  .label {
    color: hsla(160, 100%, 37%, 1);
  }
</style>