<template>
 <v-main class="list">
   <h3 class="text-h3 font-weight-medium mb-5">To Do List</h3>

   <v-card>
     <v-card-title>
       <v-text-field
         v-model="search"
         append-icon="mdi-magnify"
         label="Search"
         single-line
         hide-details
       ></v-text-field>
      <v-spacer>             
       -
        <v-select
          :items="itemprior"
          label="Prioritas"
          dense
          outlined
        ></v-select>
      </v-spacer>
  
       <v-btn color="success" dark @click="dialog = true">
         Tambah
       </v-btn>
     </v-card-title>

     <v-data-table 
        v-model="selected"
        :headers="headers" 
        :items="todos" 
        
        :search="search" 
        show-select
        class="elevation-1">

    
       
       <template v-slot:[`item.actions`]="{ item }">
         <v-btn small class="mr-2" @click="editItem(item)">
           edit
         </v-btn>
         <v-btn small @click="deleteItem(item)">
           delete
         </v-btn>
       </template>
       <!-- <template v-slot:[`item.select`]="{ item }">
        <v-simple-checkbox v-model="item.select"></v-simple-checkbox>
      </template> -->
      
     </v-data-table>
   </v-card>


   <div class="text-xs-center pt-2">
      <v-btn color="primary" @click="deleteProject">Delete</v-btn>
    </div>

   <v-dialog v-model="dialog" persistent max-width="600px">
     <v-card>
       <v-card-title>
         <span class="headline"> {{ formTitle }} </span>
       </v-card-title>
       <v-card-text>
         <v-container>
           <v-text-field
             v-model="formTodo.task"
             label="Task"
             required
           ></v-text-field>

           <v-select
             v-model="formTodo.priority"
             :items="['Penting', 'Biasa', 'Tidak penting']"
             label="Priority"
             required
           ></v-select>

           <v-textarea
             v-model="formTodo.note"
             label="Note"
             required
           ></v-textarea>
         </v-container>
       </v-card-text>

       <v-card-actions>
         <v-spacer></v-spacer>
         <v-btn color="blue darken-1" text @click="cancel">
           Cancel
         </v-btn>
        <v-btn color="blue darken-1" text @click="resetForm">
            Reset
        </v-btn>
         <v-btn color="blue darken-1" text @click="save">
           Save
         </v-btn>
       </v-card-actions>
     </v-card>
   </v-dialog>
 </v-main>
</template>


<script>
export default {
 name: "List",
 data() {
   return {
     enabled: null,
     search: null,
     dialog: false,
     delCon: false,
     
     editedIndex: -1,
     editedItem: {
        task: "",
         priority: "",
         note: "",
      },
     itemprior: ['All Priority','Penting', 'Biasa', 'Tidak penting'],
     selected: [],
     singleSelect: true,
     headers: [
       {
         text: "Task",
         align: "start",
         sortable: true,
         value: "task",
       },
       { text: "Priority",sortable: true, value: "priority" },
       { text: "Note",sortable:false, value: "note" },
       { text: "Actions", value: "actions" },
     ],
     todos: [
       {
         task: "bernafas",
         priority: "Penting",
         note: "huffttt",
       },
       {
         task: "nongkrong",
         priority: "Tidak penting",
         note: "bersama tman2",
       },
       {
         task: "masak",
         priority: "Biasa",
         note: "masak air 500ml",
       },
     ],
     formTodo: {
       task: null,
       priority: null,
       note: null,
     },
   };
 },

 computed: {
      formTitle () {
        return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
      },
    },

watch: {
      dialog (val) {
        val || this.close()
      },
    },

 methods: {
   save() {
     if (this.editedIndex > -1) {
          Object.assign(this.todos[this.editedIndex], this.formTodo)
        } else {
          this.todos.push(this.formTodo)
        }
        this.dialog = false;
   },

  close () {
        this.dialog = false
        // this.$nextTick(() => {
          this.formTodo = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        // })
      },

   cancel() {
     this.resetForm();
     this.dialog = false;
   },
   resetForm() {
     this.formTodo = {
       task: null,
       priority: null,
       note: null,
     };
   },
   
   deleteItem(item) {
       const index = this.todos.indexOf(item)
        confirm('Are you sure you want to delete this item?') && this.todos.splice(index, 1)
   },

   editItem(item){
      this.editedIndex = this.todos.indexOf(item)
      this.formTodo = Object.assign({}, item)
      this.dialog = true
   },

    deleteProject(){
        if(confirm('Are you sure you want to delete this item?')){
            for(var i = 0; i <this.selected.length; i++){
                const index = this.todos.indexOf(this.selected[i]);
                this.todos.splice(index, 1);
            }
        }
    },
   
 },
};
</script>
