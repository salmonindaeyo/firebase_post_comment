<script setup>
import { RouterLink, RouterView } from 'vue-router'
import { ref, onMounted } from 'vue'
import { onSnapshot , query , collection , getDocs, deleteDoc , doc , setDoc, Timestamp , addDoc , updateDoc } from "firebase/firestore"
import db from './firebase/init.js'
import UserList from './components/UserList.vue'

const users = ref([])

async function getUsers(){
  // const querySnap = await getDocs(query(collection(db,'users')));

  //     querySnap.forEach((doc)=>{
  //       let data = doc.data()
  //       data.id = doc.id
  //       users.value.push(data)
  //     })

  const qry =  query(collection(db, 'users'))

  const un = await onSnapshot(qry, (querySnap) => {
    users.value=[]
  querySnap.forEach((doc) => {
      console.log(doc.data());
      let data = doc.data();
      data.id = doc.id;
      users.value.push(data);

    })

  })


}

    

onMounted(() => {
    getUsers() 
})
</script>

<template>
  <div>
    <div>
      <UserList :users="users" />
    </div>
  </div>
  <div class="content">
    <RouterView />
  </div>
</template>

<style scoped>

</style>

