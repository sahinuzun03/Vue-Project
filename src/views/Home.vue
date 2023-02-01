<template>
  <div class="home">
  <div v-if="projects.length">
    <div v-for="project in projects" :key="project.id">
      <h2>{{ project.title }}</h2>
      <p>{{ project.details }}</p>
    </div>
  </div>
  </div>
</template>

<script>
export default {
  name: 'HomeView',
  components: {
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
