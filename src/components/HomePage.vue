<template >
  <main class="todo-app">
    <div dir="rtl">
      <header class="text-center ">
        <h1 class="my-4 my-sm-5 fs-2">لیست انجام کارها</h1>
        <input ref="taskInput" class="style-input " v-model="usertext" type="text" @keyup.enter="addtext" placeholder="یک کار جدید وارد کنید" />
        <button  @click="addtext" class="btn btn-success me-2">اضافه کن</button>
      </header>


      <div class="row  p-lg-5 mt-5 ">

        <!-- لیست کارهای انجام نشده-->
        <section class="col-12 col-md-6   ">
          
          <h2 class="fs-4 text-blue text-center">کارهای انجام نشده</h2>
          <ul class="mt-4  p-2 p-lg-3 task-box ">
            <li
              class="d-flex responsive-text  justify-content-between mb-3"
              v-for="(item, index) in tasks"
              :key="index"
            >
              <span>{{ item }}</span>
              <!-- دکمه حذف کنار هر آیتم -->
              <div>
                <button
                  @click="moveTask(index)"  :aria-label="`علامت گذاری ${item} به انجام شده`"
                  class="btn btn-success btn-responsive ms-2"
                >
                  انجام شد
                </button>
                <button
                  @click="removeTask(index)" :aria-label="`حذف ${item}`"
                  class="btn btn-danger btn-responsive ms-2 "
                >
                  حذف
                </button>
              </div>
            </li>
          </ul>
        </section>
        
        <!-- لیست کارهای انجام شده-->
        <section class="col-12 col-md-6 mt-md-0 mt-4 ">
          <h2 class="fs-4 text-blue text-center">کارهای انجام شده</h2>
         
            <ul class="mt-4 task-box p-2 p-lg-3 ">
            <li
              class="d-flex justify-content-between responsive-text mb-3"
              v-for="(item, index) in doTasks"
              :key="index"
            >
              <span>{{ item }}</span>
              <!-- دکمه حذف کنار هر آیتم -->
              <div>
                <button
                  @click="removedoTasks(index)"  :aria-label="`حذف ${item}`"
                  class="btn btn-danger  ms-2 btn-responsive "
                >
                  حذف
                </button>
                 
              </div>
            </li>
          </ul>

       
          
        </section>
      </div>
    </div>
  </main>
</template>

<script>
import { ref, onMounted, watch } from "vue";

export default {
  setup() {
    const usertext = ref("");
    const tasks = ref([]);
    const doTasks = ref([]);
    const taskInput = ref(null);
      // بارگذاری داده‌ها از Local Storage هنگام شروع
    onMounted(() => {
      const savedTasks = localStorage.getItem("tasks");
      const savedDoTasks = localStorage.getItem("doTasks");
      if (savedTasks) tasks.value = JSON.parse(savedTasks);
      if (savedDoTasks) doTasks.value = JSON.parse(savedDoTasks);
    });

    // ذخیره خودکار تغییرات در Local Storage
    watch(tasks, (newTasks) => {
      localStorage.setItem("tasks", JSON.stringify(newTasks));
    }, { deep: true });

    watch(doTasks, (newDoTasks) => {
      localStorage.setItem("doTasks", JSON.stringify(newDoTasks));
    }, { deep: true });

    //تابع اضافه کردن کارها
    function addtext() {
      if(usertext.value.trim() != "") {
          tasks.value.push(usertext.value);
        
      }
      usertext.value = ""; // خالی کردن input
      taskInput.value.focus();
      

    
    }
    //تابع حذف کار انجام شده
    function removedoTasks(index) {
      doTasks.value.splice(index, 1);
    }
     //تابع حذف کار انجام نشده
    function removeTask(index) {
      tasks.value.splice(index, 1);
    }
    //تابع انتقال از انجام نشده به انجام شده
    function moveTask(index) {
      doTasks.value.push(tasks.value[index]);
      tasks.value.splice(index, 1);
    }

    return { addtext, usertext, tasks, removeTask, moveTask, doTasks,removedoTasks, taskInput };
  },
};
</script>

<style >

</style>
