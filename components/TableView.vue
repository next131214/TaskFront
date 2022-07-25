<template>
  <div>
    <table class="table-fixed border-collapse border-2 border-gray-500">
      <thead>
        <tr>
          <th class="w-1/2 px-4 py-2">更新</th>
          <th class="w-1/4 px-4 py-2">内容</th>
          <th class="w-1/4 px-4 py-2">実行/未実行</th>
        </tr>
      </thead>
      <tbody v-for="column in items" :key="column.id">
        <tr class="bg-gray-100">
          <td class="border px-4 py-2">
            <button
              @click="ShowUpdateModal(column.id)"
              class="bg-blue-400 hover:bg-blue-300 text-white rounded px-4 py-2"
            >
              更新
            </button>
          </td>
          <td class="border px-4 py-2">
            {{ column.text }}
          </td>
          <td class="border px-4 py-2">
            {{ DoneType[column.done] }}
          </td>
        </tr>
      </tbody>
    </table>
    <button
      class="bg-gray-300 hover:bg-gray-200 text-white rounded px-4 py-2"
      @click="GetTodoALL"
    >
      表示更新
    </button>
    <button
      class="bg-blue-600 hover:bg-blue-500 text-white rounded px-4 py-2"
      @click="ShowUpdateModal(0)"
    >
      登録
    </button>
    <!-- <ModalConfirm v-show="false"  /> -->
    <TaskUpdate
      v-show="modalshow"
      v-on:GetTodoALL="GetTodoALL"
      :updatetodo="updatetodo"
    />
  </div>
</template>

<script lang="ts">
import axios from "axios";
import Vue from "vue";
import TaskUpdate from "~/components/TaskUpdate.vue";

export type DefTodo = {
  name: string;
  id: number;
};

export default Vue.extend({
  name: "TableView",
  components: { TaskUpdate },

  data: () => ({
    DoneType: {
      false: "未実行",
      true: "実行済み",
    },
    items: {},
    modalshow: false,
    updatetodo: {
      user_id: "1",
      text: "",
      id: "",
    },
  }),
  created() {
    this.GetTodoALL();
  },
  methods: {
    GetTodoALL: function (): void {
      let api = axios({
        url: "/api1/query",
        headers: {},
        method: "POST",
        data: {
          query: `query{
          todos{
            id,
            text,
            done
          }
          }`,
        },
      }).then((res) => {
        this.items = res.data.data.todos;
      });
    },

    ShowUpdateModal: function (id: number) {
      if (id != 0) {
        let api = axios({
          url: "/api1/query",
          headers: {},
          method: "POST",
          data: {
            query:
              `query{
          todo(id:"` +
              id +
              `"){
            id,
            text,
            done
          }
          }`,
          },
        }).then((res) => {
          this.updatetodo = res.data.data.todo;
        });
      } else {
        this.updatetodo = {
          user_id: "1",
          text: "",
          id: "",
        };
      }
      this.modalshow = true;
    },
  },
});
</script>
