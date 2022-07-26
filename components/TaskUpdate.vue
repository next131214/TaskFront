<template>
  <!-- Main modal -->
  <div
    id="defaultModal"
    tabindex="-1"
    class="overflow-y-auto overflow-x-hidden fixed top-0 right-0 left-0 z-50 w-full md:inset-0 h-modal md:h-full justify-center items-center flex"
    aria-modal="true"
    role="dialog"
  >
    <div class="relative p-4 w-full max-w-2xl h-full md:h-auto">
      <!-- Modal content -->
      <div class="relative bg-white rounded-lg shadow dark:bg-gray-700">
        <!-- Modal header -->
        <div
          class="flex justify-between items-start p-4 rounded-t border-b dark:border-gray-600"
        >
          <h3 class="text-xl font-semibold text-gray-900 dark:text-white">登録</h3>
          <button
            type="button"
            @click="$parent.modalshow = false"
            class="text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm p-1.5 ml-auto inline-flex items-center dark:hover:bg-gray-600 dark:hover:text-white"
            data-modal-toggle="defaultModal"
          >
            <svg
              aria-hidden="true"
              class="w-5 h-5"
              fill="currentColor"
              viewBox="0 0 20 20"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                fill-rule="evenodd"
                d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z"
                clip-rule="evenodd"
              ></path>
            </svg>
            <span class="sr-only">閉じる</span>
          </button>
        </div>
        <!-- Modal body -->
        <div class="p-6 space-y-6">
          <div class="mt-8">
            <form class="w-10/12 mx-auto md:max-w-md">
              <div class="mb-8">
                <label for="name" class="text-sm block">内容</label>
                <input
                  type="text"
                  id="text"
                  v-model="updatetodo.text"
                  class="w-full py-2 border-b focus:outline-none focus:border-b-2 focus:border-indigo-500 placeholder-gray-500 placeholder-opacity-50"
                  placeholder="タスク内容"
                />
              </div>
            </form>
          </div>
        </div>
        <!-- Modal footer -->
        <div
          class="flex items-center p-6 space-x-2 rounded-b border-t border-gray-200 dark:border-gray-600"
        >
          <button
            data-modal-toggle="defaultModal"
            type="button"
            @click="update(id)"
            class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800"
          >
            登録
          </button>
          <button
            data-modal-toggle="defaultModal"
            type="button"
            @click="$parent.modalshow = false"
            class="text-gray-500 bg-white hover:bg-gray-100 focus:ring-4 focus:outline-none focus:ring-blue-300 rounded-lg border border-gray-200 text-sm font-medium px-5 py-2.5 hover:text-gray-900 focus:z-10 dark:bg-gray-700 dark:text-gray-300 dark:border-gray-500 dark:hover:text-white dark:hover:bg-gray-600 dark:focus:ring-gray-600"
          >
            閉じる
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import axios from "axios";

export default Vue.extend({
  name: "ModalUpdate",
  data: () => ({}),
  created() {},
  props: ["updatetodo"],
  methods: {
    update: function () {
      if (this.updatetodo.id == "") {
        let api = axios({
          url: "/api1/query",
          headers: {},
          method: "POST",
          data: {
            query:
              `mutation createtodo{
                    createTodo(input:{
                            text:"` +
              this.updatetodo.text +
              `",
                            userId:"` +
              this.updatetodo.user_id +
              `"
                    }) 
                }`,
          },
        }).then((res) => {
          if (res.status == 200) {
            this.$parent.modalshow = false;
            this.$emit("GetTodoALL");
          } else {
          }
        });
      } else {
        let api = axios({
          url: "/api1/query",
          headers: {},
          method: "POST",
          data: {
            query:
              `mutation updateTodo{
                      updateTodo(input:{
                        text:"` +
                        this.updatetodo.text +
                         `",
                        userId:"` +
                        this.updatetodo.user_id +
                          `"
                        id:"` +
                        this.updatetodo.id +
                        `"
                        done:` +
                        this.updatetodo.done +
                        `
              })
            }`,
          },
        }).then((res) => {
          if (res.status == 200) {
            this.$parent.modalshow = false;
            this.$emit("GetTodoALL");
          } else {
          }
        });
      }
    },
  },
});
</script>
