<template>
    <div class="mt-10 max-w-xl mx-auto">
        <h1 class="font-roboto text-5xl text-center">EOD Updates</h1>
        <div class="max-w-lg mx-auto">
            <div class="w-full pt-6 space-x-3">
                <input id="title" type="text" class="w-3/5 border-1 border-black outline-none px-4 py-1"
                    placeholder="Enter Project Title" v-model="projTitle">
                <!-- <button
                    class="border-1 border-transparent bg-rose-600 text-white text-sm font-sans font-extrabold px-4 py-[6px]">Add
                    Title</button> -->
            </div>
            <div class="w-full pt-3 flex items-center space-x-3">
                <input id="task" type="text" class="w-[40%] border-1 border-black outline-none px-4 py-1"
                    placeholder="Enter Tasks" v-model="listField">
                <input id="hr" type="number" class="w-1/4 border-1 border-black outline-none px-4 py-1"
                    placeholder="Enter Hours" v-model="hour" @keyup.enter="addProjectList">
                <input type="checkbox" name="" id="" class="inline-block text-xl w-5 h-5" v-model="isComplete">
                <button @click="addProjectList"
                    class="border-1 border-transparent bg-rose-600 text-white text-sm font-sans font-extrabold px-4 py-[6px]">Add
                    Tasks</button>
            </div>
            <!-- <div class="w-full pt-3 flex items-center space-x-3">
                <button @click="eodSubmit"
                    class="border-1 border-transparent bg-rose-600 text-white text-sm font-sans font-extrabold px-4 py-[6px]">Done</button>
            </div> -->
        </div>
    </div>
    <div class="mt-5 max-w-xl mx-auto px-8" v-for="(eod, index) in projectArray" :key="index">
        <div>
            <h3 class="text-xl font-roboto font-bold mb-2">**{{ eod.projTitle }}**</h3>
            <ul class="ml-4 font-roboto list-disc font-medium">
                <li class="flex items-center space-x-2" v-for="(list, index) in eod.taskItems">*&nbsp;{{
                    list.desc
                }} {{ list.hr }}h<i class="fa-solid text-lg ml-1"
                        :class="{ 'text-green-500 fa-check': list.isDone, 'text-yellow-500 fa-clock': !list.isDone }"></i>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            projTitle: "",
            listField: "",
            hour: "",
            isComplete: false,
            projectObject: {
                projTitle: "",
                taskItems: []
            },
            projectArray: [],
            lists: []
        }
    },
    methods: {
        addProjectTitle() {
            if (this.projTitle) {
                this.projectObject = {
                    projTitle: "",
                    taskItems: []
                }
                this.projectObject.projTitle = this.projTitle
            }
        },
        addProjectList() {
            if (this.listField && this.hour) {
                this.addProjectTitle()
                if (this.projTitle) {
                    this.projectArray.push(this.projectObject)
                    this.projTitle = ""
                }
                if (!this.projTitle) {
                    this.projectArray[this.projectArray.length - 1].taskItems.push({
                        desc: this.listField,
                        hr: this.hour,
                        isDone: this.isComplete
                    })
                    this.listField = ""
                    this.hour = ""
                    this.isComplete = false
                }
                // console.log(this.projectArray);
            }
        }
    }
}
</script>

<style></style>
