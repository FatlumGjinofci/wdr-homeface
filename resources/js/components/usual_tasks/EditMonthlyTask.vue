<template>
    <div class="py-6 px-2">
        <p>Edit Monthly task</p>
        <form @submit.prevent="updateTask" class="mt-6">
            <div class="flex justify-between gap-3">
                <span class="w-1/2">
                    <label for="first_name" class="block text-xs font-semibold text-gray-600 uppercase">Name</label>
                    <input id="first_name" type="text" name="first_name" class="block w-full p-3 mt-2 text-gray-700 bg-gray-200 appearance-none focus:outline-none focus:bg-gray-300 focus:shadow-inner" required />
                </span>
                <span class="w-1/2">
                    <label for="status" class="block text-xs font-semibold text-gray-600 uppercase">Status</label>
                    <div class="relative">
                        <select name="status" id="" class="block w-full p-3 mt-2 text-gray-700 bg-gray-200 appearance-none focus:outline-none focus:bg-gray-300 focus:shadow-inner"   
                        >
                            <option>K</option>
                            <option>P</option>
                            <option>0</option>
                        </select>
                        <div
                            class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-2 text-gray-700"
                        >
                            <svg
                            class="fill-current h-4 w-4"
                            xmlns="http://www.w3.org/2000/svg"
                            viewBox="0 0 20 20"
                            >
                            <path d="M9.293 12.95l.707.707L15.657 8l-1.414-1.414L10 10.828 5.757 6.586 4.343 8z" />
                            </svg>
                        </div>
                    </div>
                </span>
            </div>
             <!-- <div class="flex justify-between gap-3">
                <span class="w-1/2">
                    <label for="start" class="block mt-2 text-xs font-semibold text-gray-600 uppercase">Start</label>
                    <input id="start" v-model="task.start" @keydown="calculate" type="time" name="start" class="block w-full p-3 mt-2 text-gray-700 bg-gray-200 appearance-none focus:outline-none focus:bg-gray-300 focus:shadow-inner" required />
                </span>
                <span class="w-1/2">
                    <label for="finish" class="block mt-2 text-xs font-semibold text-gray-600 uppercase">Finish</label>
                    <input id="finish" v-model="task.finish" @keydown="calculate" type="time" name="finish" class="block w-full p-3 mt-2 text-gray-700 bg-gray-200 appearance-none focus:outline-none focus:bg-gray-300 focus:shadow-inner" required />
                </span>
            </div>
            <label v-if="diffTime>0" class="float-right block mt-2 text-xs font-semibold text-gray-600 uppercase">Duration: {{ diffTime | duration('humanize') }}</label>
            <label for="day" class="block mt-2 text-xs font-semibold text-gray-600 uppercase">Date</label>
            <input id="day" v-model="task.date" type="date" name="date" class="block w-full p-3 mt-2 text-gray-700 bg-gray-200 appearance-none focus:outline-none focus:bg-gray-300 focus:shadow-inner" required /> -->
            <!-- <div class="py-1 float-right inline-block" v-for="worker in task.workers" :key="worker.id">
                <p class="text-center mx-1 rounded-full bg-gray-500 text-white  w-5 h-5 justify-center text-xs font-semibold">
                    {{ worker.initials }} 
                </p>
            </div>
            <label for="workers" class="block mt-2 text-xs font-semibold text-gray-600 uppercase">Workers</label>
                <multiselect
                    v-model="task.workers"
                    :options="$store.state.workers.workers"
                    :multiple="true"
                    :group-select="true"
                    :close-on-select="false"
                    :clear-on-select="false"
                    :preserve-search="true"
                    :showLabels="true"
                    :preselectFirst="false"
                    :allowEmpty="true"
                    :showNoResults="true"
                    placeholder="Choose Workers"
                    label="name"
                    track-by="name"
                    :preselect-first="true"
                    class="block w-full text-gray-700 bg-gray-200 text-sm mb-2"
                    >
                <template slot="selection" slot-scope="{ values, isOpen }">
                    <span class="multiselect__single" v-if="values.length &amp;&amp; !isOpen">
                        {{ values.length }} options selected
                    </span>
                </template>
                <template slot="noResult">
                    No Workers
                </template>      
            </multiselect> -->
            <button class="w-1/3 float-right py-3 my-3 mt-6 font-medium tracking-widest text-white uppercase bg-gray-700 shadow-lg focus:outline-none hover:bg-gray-900 hover:shadow-none">
                Update
            </button>
        </form>
    </div>
</template>

<script>
// import Multiselect from 'vue-multiselect'
import moment from 'vue-moment'

export default {
    props: {
        id: Number
    },
    components: {
        // Multiselect,
    },
    data() {
        return {
            taskId: this.$props.id
        }
    },
    watch: {
        startTime: function(newValue, oldValue) {
            this.calculate()
        },
        finishTime: function(newValue, oldValue) {
            this.calculate()
        }    
    },
    mounted() {
        if (this.$store.state.workers.workers.length <= 0)
        {
            this.$store.dispatch('getWorkers')
        }
        this.getTask(id);
    },
    methods: {
        createUsualTask() {
            this.$store.dispatch('createUsualTask', {
                task: this.task
            })
            this.$modal.hide('add-usual-task-modal');
        },
        calculate() {
            // console.log('test')
            let start = new Date(this.$store.state.aw.task.date + " " + this.$store.state.aw.task.start);
            let finish = new Date(this.$store.state.aw.task.date + " " + this.$store.state.aw.task.finish);
            this.diffTime = Math.abs(finish - start);
            // this.diffTime = diffTime
            console.log(this.diffTime);
        },
        getTask(id) {
            this.$store.dispatch('getUsualTask', {
                id: this.taskId
            })
        },
    }
}
</script>

<style>

</style>