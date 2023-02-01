<template>
  <div class="home">
  <div v-if="projects.length">
    <div v-for="project in projects" :key="project.id">
      <!-- SingleProject vue tarafına bizim burada bulduğumuz project kısmını gönderdik. -->
      <SingleProject :project="project"></SingleProject>
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
    fetch('http://localhost:3008/projects')
    .then(res => res.json())
    .then(data => this.projects = data)
    .catch(err => console.log(err.message))
  }
}
</script>
