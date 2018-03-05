<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card card-default">
                    <div class="card-header">My Tasks</div>

                    <div class="card-body">
                        <div class="input-group">
                            <input type="text" class="form-control" v-model="task.name" @keydown.enter="create">
                            <span class="input-group-btn">
                                <button class="btn btn-success" @click="create">Add Task</button>
                            </span>
                        </div>

                        <div class="tasks-list">
                            <ul class="list-unstyled">
                                <li v-for="task in tasks" :key="task.id">
                                    <div class="checkbox">
                                        <label><input type="checkbox" v-model="task.completed" @click="done(task)">{{ task.name }}</label>
                                    </div>
                                </li>
                            </ul>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        mounted() {
            this.fetchData()
        },
        data () {
            return {
                tasks: [],
                task: {
                    name: ''
                }
            }
        },
        methods: {
            fetchData () {
                axios.get('/api/tasks').then((res) => {
                    this.tasks = res.data
                }).catch((err) => {
                    console.log(err)
                })
            },

            create () {
                axios.post('/api/tasks', this.task).then((res) => {
                    this.tasks.unshift(res.data)
                    this.task.name = ''
                }).catch((err) => {
                    console.log(err)
                })
            },

            done (task) {
                axios.put('/api/tasks/${task.id}', {
                    completed: task.completed
                }).then((res) => {
                    console.log('Task Updated')
                }).catch((err) => {
                    console.log(err)
                })
            }
        }
    }
</script>

<style>
    body, .tasks-list {
        padding-top:20px
    }
</style>
