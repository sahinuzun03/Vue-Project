<template>
  <div class="project" :class="{complete:project.complete}">
    <div class="actions">
        <h3 @click="showDetails = !showDetails">{{ project.title }}</h3>
        <div class="icons">
            <router-link :to="{name:'EditProject',params:{id:project.id}}"><span class="material-icons">
                edit
            </span></router-link>
            <span class="material-icons" @click="deleteProject">
                delete
            </span>
            <span class="material-icons tick" @click="toggleComplete">
                done
            </span>
        </div>
    </div>
    <div v-if="showDetails" class="details">
        <p>{{ project.details }}</p>
    </div>
  </div>
</template>

<script>
export default {
    props:['project'],
    data(){
        return{
            showDetails:false,
            uri:'http://localhost:3008/projects/' + this.project.id
        }
    },
    methods:{
        deleteProject(){
            //Bu metot bir istek yapacak
            //db.json içerisinde silme işlemi yapıyorum fakat üst component içerisinde bulunan projects dizisindende bu veriyi silmem gerekiyor. event olarak yukarıya bunu göndereceğim.
            fetch(this.uri,{method: "DELETE"}).then(() => 
            this.$emit("delete",this.project.id)
            ).catch(err => console.log(err));//event ile beraber id'yi gönderdim.
        },
        toggleComplete(){
            fetch(this.uri,{
                method:"PATCH",
                headers:{'Content-Type':'application/json'},//fetch json headers diye aratacağız
                body:JSON.stringify({complete:!'this.project.complete'})
            }).then(() => 
            this.$emit("complete",this.project.id)
            .catch(err => console.log(err)));
        }
    },
};
</script>

<style>
.project{
    margin:20px auto;
    background: #fff;
    padding: 10px 20px;
    border-radius: 5px;
    box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.05);
    border-left: 4px solid #ff5500;
}

h3{
    cursor:pointer;
}

.actions{
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.material-icons{
    font-size: 24px;
    margin-left: 10px;
    color:#bbb;
    cursor:pointe;
}

.material-icons:hover{
    color:#888;
}

.project.complete{
    border-left: 4px solid #76dd78;
}

.project.complete .tick{
    color:#76dd78;

}
</style>