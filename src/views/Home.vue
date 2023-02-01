<template>
  <div class="home">
  <div v-if="projects.length">
    <!-- @delete ile eventi dinledim o event çalışınca handleMetotdunu çalıştıracağımı söylüyorum. -->
    <div v-for="project in projects" 
    :key="project.id" 
    >
      <!-- SingleProject vue tarafına bizim burada bulduğumuz project kısmını gönderdik. -->
      <SingleProject :project="project" @delete="handleDelete"></SingleProject>
    </div>
  </div>
  </div>
</template>

<script>
import SingleProject from '@/components/SingleProject.vue';

export default {
  name: 'Home',
  components: {
    SingleProject
  },
  data(){
    return{
      projects:[]
    }
  },
  mounted(){
    //Proje ilk yüklendiği zaman çalışacak olan metot
    fetch('http://localhost:3008/project')
    .then(res => res.json())
    .then(data => this.projects = data)
    .catch(err => console.log(err.message))
  },
  methods:{
    handleDelete(id){
      this.projects = this.projects.filter((project) => project.id !== id)
      //Btün projeleri dolaş eşit id'li veri yoksa ona göre işlem yap dedik
    }
  }
}
</script>
