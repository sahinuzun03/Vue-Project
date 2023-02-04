<template>
  <div class="home">
    <FilterNav @filterChange="current = $event" :current="current"></FilterNav>
  <div v-if="projects.length">
    <!-- @delete ile eventi dinledim o event çalışınca handleMetotdunu çalıştıracağımı söylüyorum. -->
    <div v-for="project in filteredProjects" 
    :key="project.id" 
    >
      <!-- SingleProject vue tarafına bizim burada bulduğumuz project kısmını gönderdik. -->
      <SingleProject 
      :project="project"
      @delete="handleDelete"
      @complete="handleComplete">
      </SingleProject>
    </div>
  </div>
  </div>
</template>

<script>
import SingleProject from '@/components/SingleProject.vue';
import FilterNav from '../components/FilterNav.vue'

export default {
  name: 'Home',
  components: {
    SingleProject,
    FilterNav
  },
  data(){
    return{
      projects:[],
      current:'all'
    }
  },
  mounted(){
    //Proje ilk yüklendiği zaman çalışacak olan metot
    fetch('http://localhost:3008/projects')
    .then(res => res.json())
    .then(data => this.projects = data)
    .catch(err => console.log(err.message))
  },
  methods:{
    handleDelete(id){
      this.projects = this.projects.filter((project) => project.id !== id)
      //Btün projeleri dolaş eşit id'li veri yoksa ona göre işlem yap dedik
    },
    handleComplete(id){
      let p = this.projects.find(project => project.id == id);//ID leri çakışanları buldum
      if(p.complete == true){
        p.complete = false;
      }
      else
        p.complete = true;

    }
  },

  computed:{
    filteredProjects(){
      if(this.current === "completed"){
        return this.projects.filter((project) => project.complete);
      }
      if(this.current ==="ongoing"){
        return this.projects.filter((project) => !project.complete);
      }
      return this.projects;
    }
  }
}
</script>
