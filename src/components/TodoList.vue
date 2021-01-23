<template>
  <div>
    <input type="text" class="todo-input" placeholder="What needs to be done" v-model="newTodo" v-on:keyup.enter="addTodo">
    <div 
    v-for="(todo, index) in todos" 
    v-bind:key="todo.id" 
    class="todo-item">
        <div class="todo-item-left">
            <input type="checkbox" v-model="todo.completed">
            <div
            v-if="!todo.editing"
            class="todo-item-label"
            :class="{ completed : todo.completed}"
            v-on:dblclick="editTodo(todo)"
            >
                {{ todo.title }}
            </div>
            <input 
            v-else
            type="text"
            class="todo-item-edit" 
            v-model="todo.title"
            v-on:blur="doneEdit(todo)"
            v-on:keyup.enter="doneEdit(todo)"
            v-on:keyup.esc="cancelEdit(todo)"
            v-focus
            >
        </div>
            <div class="remove-item" v-on:click="removeTodo(index)"> &times; </div>
    </div>
    <div class="extra-container">
        <div>
            <label>
                <input type="checkbox" :checked="!anyRemaining" @change="checkAllTodos($event)">
                Check All
            </label>
        </div>
        <div>{{ remaining}} items left</div>
    </div>
  </div>
</template>

<script>
export default {
    name: 'todo-list',
    data(){
        return{
            newTodo: '',
            idForTodo: 3,
            beforeEditCache: '',
            todos:[
                {
                    'id': 1,
                    'title': 'Finish Vue Screencast',
                    'completed': false,
                    'editing': false
                },
                {
                    'id': 2,
                    'title': 'Take over world',
                    'completed': false,
                    'editing': false
                },
                {
                    'id': 3,
                    'title': 'Save the world',
                    'completed': false,
                    'editing': false
                }
            ]
        }
    },
    computed:{
        remaining(){
            return this.todos.filter(todo => !todo.completed).length;
        },
        anyRemaining(){
            return this.remaining != 0
        }
    },
    /**
     * custom drievtive
     * tự động focus vào element
     * created by: NQDAT (23/01/2021)
     */
    directives:{
        focus: {
            inserted: function(el){
                el.focus()
            }
        }
    },
    methods:{
        /**
         * Hàm thêm một việc vào danh sách phải làm
         * created by: NQDAT(23/01/2021)
         */
        addTodo(){
            //Kiểm tra nếu việc nhập vào trống thì không thêm
            if (this.newTodo.trim() == ''){
                return
            }
            this.todos.push({
                'id': this.idForTodo,
                'title': this.newTodo,
                'completed': false,
                'editing': false
            })
            this.newTodo = ''
            this.idForTodo++
        },
        removeTodo(index){
            this.todos.splice(index, 1)
        },
        editTodo(todo){
            this.beforeEditCache = todo.title;
            todo.editing = true;
        },
        cancelEdit(todo){
            todo.title = this.beforeEditCache;
            todo.editing= false;
        },
        doneEdit(todo){
            if (todo.title.trim() == ''){
                todo.title = this.beforeEditCache;
            }
            todo.editing = false;
        },
        checkAllTodos(event){
            this.todos.forEach((todo) =>
                todo.completed =  event.target.checked
            ); 
        }
    }
}
</script>

<style>
.todo-input{
    width: 100%;
    height: 40px;
    margin: 12px 0;
    padding: 10px 16px;
}
.todo-item-left{
    display: flex;
    align-items: center;
}
.todo-item{
    display: flex;
    margin-bottom: 12px;
    justify-content: space-between;
    cursor: default;
}
.remove-item:hover{
    cursor: pointer;
}
input[type="checkbox"]{
    margin: 5px;
    outline: none;
}
.completed{
    text-decoration: line-through;
    color: #ccc;
}
.extra-container{
    display: flex;
    border-top: 1px solid #ccc;
    padding: 10px 0;
    justify-content: space-between;
}
</style>