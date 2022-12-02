<script setup>
import { ref, onMounted, watch } from 'vue'
import { useRoute } from "vue-router"
import { onSnapshot , query , collection ,getDoc , getDocs, deleteDoc , doc , setDoc, Timestamp , addDoc , updateDoc , where ,getCountFromServer} from "firebase/firestore"
import db from "../firebase/init.js"
import PostItem from "../components/PostItem.vue"



const props = defineProps({
  id: {
    type: String,
    //required: true,
  },
  post: {
    type: Object,
    //required: true,
  } ,
  count: {
    type: Number,
    //required: true,
  }
  
});


const form = ref({star:0,name:"",comment:""})

async function addComment(){
  
    const docRef = await addDoc(collection(db, `posts/${props.id}/comments`), {
        star:form.value.star ,
  name: form.value.name,
   comment: form.value.comment,
   cmtdate: Timestamp.fromDate(new Date()) })

 

   console.log("add")

  //  const commentRef = collection(db,"posts",doc.id,"comments")
  //  const snapshot = await getCountFromServer(commentRef);
  //     const count = snapshot.data().count


  

//  await  window.location.reload();
setCount()
// watch( () => route.params.user, getPosts)
}

async function setCount(){


    await setDoc(doc(db, "posts", props.post.id), {
body: props.post.body,
countComment: props.count+1,
postdate: props.post.postdate,
user :  props.post.user
}); 

 await  window.location.reload();

// watch( () => route.params.user, getPosts)
}



     
onMounted(() => {
  

})

</script>

<template>
   <div>

    <form>
      {{count}}
  <label for="fname">name:</label><br>
  <input type="text" v-model="form.name"><br>

  <label for="lname">comment:</label><br>
  <input type="text" v-model="form.comment" ><br>

  <label for="lname">star:</label><br>
  <input type="number" v-model="form.star" ><br><br>

  <div  @click="addComment()"> add comment </div>
</form> 


   </div>
</template>

<style scoped>

</style>

