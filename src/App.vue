<template>
  <v-app>
    <v-main>
      <v-container>
          <v-list>
            <v-list-item v-for="(val,idx) in studyIDs" :key=idx>
              {{val}}
            </v-list-item>
            </v-list>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

import { initializeApp } from "firebase/app";
import { getDatabase, ref, onValue, query, orderByValue} from "firebase/database";
// TODO: Add SDKs for Firebase products that you want to use
// https://firebase.google.com/docs/web/setup#available-libraries

// Your web app's Firebase configuration
const firebaseConfig = {
  apiKey: "AIzaSyC5m8Gq-hLpCGqWSvFcVmmy1PMcvtiBLNw",
  authDomain: "emotions-on-the-go.firebaseapp.com",
  databaseURL: "https://emotions-on-the-go-default-rtdb.asia-southeast1.firebasedatabase.app",
  projectId: "emotions-on-the-go",
  storageBucket: "emotions-on-the-go.appspot.com",
  messagingSenderId: "327339328201",
  appId: "1:327339328201:web:5893ea9908b950d4f00318"
};

// Initialize Firebase
const app = initializeApp(firebaseConfig);
const db = getDatabase(app);


export default {
  name: 'App',

  components: {
    HelloWorld,
  },

  data: function(){
    return{
      studyIDs : []
    }
  },
  computed:{
   
  },
  mounted()
  {
    this.getStudyIDs()
  },
  methods: {
      getStudyIDs()
      {
        const dataRef = query(ref(db,'EMOTION_RATING/'),orderByValue('study_ID'));
        onValue(dataRef,(snapshot)=>{
          var predictions = snapshot.val();
          var temp = []
          
          Object.entries(predictions).forEach(([key,val]) => {
              if(predictions[key].study_ID)
                temp.push(predictions[key].study_ID)
          });
          temp = [...new Set(temp)]
          this.studyIDS = temp
          console.log(temp)
          return temp
        })
      }
  }
}
</script>

<style scoped>

</style>
