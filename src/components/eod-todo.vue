<template>
    <div class="mt-10 max-w-xl mx-auto">
        <h1 class="font-roboto text-5xl text-center">EOD Updates</h1>
        <div class="max-w-lg mx-auto">
            <div class="w-full pt-6 space-x-3">
                <input id="title" type="text" class="w-3/5 border-1 border-black outline-none px-4 py-1"
                    placeholder="Enter Project Title" v-model="projTitle" />
                <!-- <button
                    class="border-1 border-transparent bg-rose-600 text-white text-sm font-sans font-extrabold px-4 py-[6px]">Add
                    Title</button> -->
            </div>
            <div class="w-full pt-3 flex items-center space-x-3">
                <input id="task" type="text" class="w-[40%] border-1 border-black outline-none px-4 py-1"
                    placeholder="Enter Tasks" v-model="listField" />
                <input id="hr" type="number" class="w-1/4 border-1 border-black outline-none px-4 py-1"
                    placeholder="Enter Hours" v-model="hour" @keyup.enter="addProjectList" />
                <input type="checkbox" name="" id="" class="inline-block text-xl w-5 h-5" v-model="isComplete" />
                <button @click="addProjectList"
                    class="border-1 border-transparent bg-rose-600 text-white text-sm font-sans font-extrabold px-4 py-[6px]">
                    Add Tasks
                </button>
            </div>
            <!-- <div class="w-full pt-3 flex items-center space-x-3">
                <button @click="eodSubmit"
                    class="border-1 border-transparent bg-rose-600 text-white text-sm font-sans font-extrabold px-4 py-[6px]">Done</button>
            </div> -->
        </div>
    </div>
    <div class="mt-5 max-w-xl mx-auto px-8" v-for="(eod, index) in projectArray" :key="index">
        <div>
            <div class="flex items-center mb-2 space-x-64">
                <h3 class="text-xl font-roboto font-bold">{{ eod.projTitle }}</h3>
                <button @click="copyText"
                    class="text-lg border border-gray-400 px-2 rounded-sm py-1 bg-gray-400 font-roboto font-bold">
                    Copy
                </button>
            </div>
            <ul class="ml-4 font-roboto list-disc font-medium">
                <li class="flex items-center space-x-2" v-for="(list, index) in eod.taskItems">
                    {{ list.desc }} {{ list.hr }}h <span v-if="list.isDone">✅</span> <span v-else>⏳</span>
                    <!-- <i class="fa-solid text-lg ml-1" :class="{
                        'text-green-500 fa-check': list.isDone,
                        'text-yellow-500 fa-clock': !list.isDone,
                    }"></i> -->
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
                taskItems: [],
            },
            projectArray: [],
            lists: [],
        };
    },
    methods: {
        addProjectTitle() {
            if (this.projTitle) {
                this.projectObject = {
                    projTitle: "",
                    taskItems: [],
                };
                this.projectObject.projTitle = this.projTitle;
            }
        },
        addProjectList() {
            if (this.listField && this.hour) {
                this.addProjectTitle();
                if (this.projTitle) {
                    this.projectArray.push(this.projectObject);
                    this.projTitle = "";
                }
                if (!this.projTitle) {
                    this.projectArray[this.projectArray.length - 1].taskItems.push({
                        desc: this.listField,
                        hr: this.hour,
                        isDone: this.isComplete,
                    });
                    this.isComplete = false
                    this.listField = "";
                    this.hour = "";
                    // this.isComplete = false;
                }
                // console.log(this.projectArray);
            }
        },
        copyText() {
            const div = document.createElement("div");
            const title = document.createElement("h3");
            title.innerText = "**" + this.projectObject.projTitle + "**"
            const ul = document.createElement("ul");
            this.projectObject.taskItems.forEach((item) => {
                const emoji = item.isDone ? "✅" : "⏳"
                const li = document.createElement("li");
                li.innerText = "* " + item.desc + " " + item.hr + "h " + emoji;
                ul.appendChild(li);
                div.appendChild(title);
                div.appendChild(ul);

                document.body.appendChild(div);

                const selection = window.getSelection();
                const range = document.createRange();
                range.selectNodeContents(div);
                selection.removeAllRanges();
                selection.addRange(range);

                document.execCommand('copy');

                document.body.removeChild(div);
            });
        },
    },
};
</script>

<style></style>
