<template>
    <div class="todo-wrap">
        <div class="container">
            <div class="row">
                <div class="col-md-12">
                    <h2 class="text-center mt-5">My Vue Todo App</h2>

                    <!-- Input -->
                    <div class="d-flex">
                        <input v-model="task" type="text" placeholder="Enter Task" class="form-control">
                        <button class="btn btn-warning rounded-0" @click="submitTask">Submit</button>
                    </div>

                    <!-- Table -->
                    <table class="table table-bordered mt-5">
                    <thead>
                        <tr>
                        <th scope="col">Taks</th>
                        <th scope="col">Status</th>
                        <th scope="col">#</th>
                        <th scope="col">#</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(task, index) of tasks" :key="index">
                            <th scope="row"><span :class="{'finished': task.status == 'finished'}">{{ task.name }}</span></th>
                            <td>
                                <div class="pointer" @click="changeStatus(index)" :class="{
                                    'text-danger':task.status == 'to-do',
                                    'text-warning':task.status == 'in-progress',
                                    'text-success':task.status == 'finished'

                                }">
                                    {{ firstLetterUpercase(task.status) }}
                                </div>
                                </td>
                            <td>
                                <div class="text-center" @click="editTask(index)">
                                    <span class="fa fa-pen"></span>
                                </div>
                            </td>
                            <td>
                                <div class="text-center" @click="deleteTask(index)">
                                    <span class="fa fa-trash"></span>
                                </div>
                            </td>
                        </tr>
                    </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'TodolistApp',
    data: function(){
        return{
            task: '',
            updateTask: null,
            availableStatuses: ['to-do', 'in-progress', 'finished'],
            tasks: [
                {
                    name: 'contact form 7 project',
                    status: 'to-do'
                },
                {
                    name: 'Elementor project about us page 1 hour',
                    status: 'in-progress'
                }
            ]
        }
    },
    methods: {
        submitTask: function(){
            if( this.task.length === 0 ) return
            if( this.updateTask === null ){
                this.tasks.push({
                    name: this.task,
                    status: 'to-do'
                })
            }else{
                this.tasks[this.updateTask].name = this.task
                this.updateTask = null
            }


            this.task = ''
        },
        deleteTask: function(index){
            this.tasks.splice(index, 1)
        },
        editTask: function(index){
            this.task = this.tasks[index].name
            this.updateTask = index
        },
        changeStatus: function(index){
            let newIndex = this.availableStatuses.indexOf(this.tasks[index].status)
            console.log(newIndex)
            if(++newIndex > 2) newIndex = 0
            this.tasks[index].status = this.availableStatuses[newIndex]
        },

        firstLetterUpercase: function(str){
            return str.charAt(0).toUpperCase() + str.slice(1)
        }

    }
}
</script>

<style>
    .todo-wrap{
        max-width: 1000px;
        width: 100%;
        margin: 0 auto;
    }
    .pointer{
        cursor: pointer;
    }

    .finished{
        text-decoration: line-through;
    }
</style>