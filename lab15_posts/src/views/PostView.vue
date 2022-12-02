<script setup>
import { ref, onMounted, watch } from 'vue'
import { useRoute } from "vue-router"
import { onSnapshot , query , collection ,getDoc , getDocs, deleteDoc , doc , setDoc, Timestamp , addDoc , updateDoc , where , getCountFromServer} from "firebase/firestore"
import db from "../firebase/init.js"
import PostItem from "../components/PostItem.vue"

const user = ref("")
const username = ref("")
const posts = ref([])
const route = useRoute() 

async function getPosts(){
  user.value = route.params.user 
  console.log("user.value" + "" + user.value)
  const userRef = doc(db,"users",user.value)
  const userSnap = await getDoc(userRef)
  // console.log("lol" + userSnap.data().firstname)

    if(userSnap.exists()) {
      username.value = userSnap.data().firstname+" "+userSnap.data().lastname
    }

    /////////////////////

    const postRef = collection(db,"posts")
    const postsQry = query(postRef,where("user","==",user.value))
    const querySnapshot = await getDocs(postsQry)

    posts.value = []

    querySnapshot.forEach(async (doc) => {
      let data = doc.data()
      data.id = doc.id
      const commentRef = collection(db,"posts",doc.id,"comments")
      const commentSnapshot = await getDocs(commentRef)
      data.comments = []
      commentSnapshot.forEach((comment)=> {
        let cmtdata = comment.data()
        cmtdata.id = comment.id
        data.comments.push(cmtdata)
      })
      posts.value.push(data)

      const snapshot = await getCountFromServer(commentRef);
      console.log('count: ', snapshot.data().count);

      count.value =   snapshot.data().count


      const snapshot2 = await getCountFromServer(postsQry);
      countPost.value =   snapshot2.data().count
    })


      
    

}

const count =ref()
const countPost =ref()

watch( () => route.params.user, getPosts)

onMounted(() => {
  getPosts() 
})

</script>

<template>
  
    <h3>Posts : {{username}} ({{countPost}})</h3> 
    <PostItem v-for="post in posts" :post="post" :key="post.id" :count="count" />
</template>

<style scoped>

</style>

