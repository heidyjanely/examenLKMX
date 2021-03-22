<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <div>
      <div class="card">
        <div class="card-body">
          <h5 class="card-title">To Do List</h5>
          <div class="form-group row">
              <div class="col-sm-10">
                <input type="text" class="form-control" v-model="newTask" @keyup.enter="addTask" placeholder="Escribre una tarea..."/>
              </div> 
              <div class="col-sm-1">
                <button class="btn btn-primary" type="submit" @click="addTask">Agregar</button>
              </div>
            </div>


          <div v-for="(task) in list1" :key="task.id" >
            <div class="alert alert-dark" role="alert" v-if="vacio==true">
              No has agregado tareas.
            </div>
            <div class="form-row" v-else-if="!task.edit && vacio==false && task.title!=''" @dblclick="editTask(task)">
              <div class="col-9">• {{task.title}}</div>
              <div class="col-3"> 
              <button type="button" class="btn btn-outline-dark" @click="editTask(task)"><img src="../assets/icon-pen.svg"/></button>
              <button type="button" class="btn btn-outline-dark"  @click="deleteTask(task)"><img src="../assets/icon-trash.svg"/></button>
              </div>
            </div>
            <div class="form-row" v-if="task.edit" @dblclick="editTask(task)">
               <div class="col-9">
              <input type="text" class="form-control" v-model="task.title" @blur="endEdit(task)" @keyup.enter="endEdit(task)" v-focus>
              </div>
              <div class="col-3"> 
              <button type="button" class="btn btn-outline-dark"  @click="addTask"><img src="../assets/icon-disk.svg"/></button>
              </div>
            </div>
          </div>
          
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data() {
    return {
      newTask: '',
      save: '',
      idForTask: 1,
      vacio: true,
      list1: [
        {
          'id': '',
          'title': '',
          'edit': false,
          
        },
      ],
    };
  },
  directives: {
        focus: {
            inserted: function (el) {
            el.focus()
            }
        }
    },
  methods: {
    addTask() {
      
      this.list1.push({
        id: this.idForTask,
        title: this.newTask,
        edit: false,
        
      })

      this.newTask = ''
      this.idForTask++
      this.vacio = false
    },
    editTask(task){
      this.save = task.title
      task.edit=true
    },
    endEdit(task){
      if(task.title.trim()==''){
        task.title = this.save
      }
      task.edit = false
    },
    deleteTask(task){
      this.list1.splice(this.list1.findIndex(function(t){
        return t.id == task.id
      }),1)
      if(this.list1.length==null){
        this.vacio==true
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1 {
  text-align: justify;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.hello {
  position: relative;
  min-height: 1px;
}
</style>
