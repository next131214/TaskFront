<template>
  <div>
    <table class="table-fixed border-collapse border-2 border-gray-500">
      <thead>
        <tr>
          <th class="w-1/2 px-4 py-2">操作</th>
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
              変更
            </button>
            <button
              @click="ShowConfirm(column.id, 'delete')"
              class="bg-red-400 hover:bg-red-300 text-white rounded px-4 py-2"
            >
              削除
            </button>
            <button
              @click="completeTodo(column.id, 'change')"
              class="bg-green-800 hover:bg-green-700 text-white rounded px-4 py-2"
            >
              実行/未実行
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
      @click="ShowUpdateModal('')"
    >
      登録
    </button>
    <!-- <ModalConfirm v-show="false"  /> -->
    <TaskUpdate
      v-show="modalshow"
      v-on:GetTodoALL="GetTodoALL"
      :updatetodo="updatetodo"
    />
    <Confirm v-show="confirm" @emitFromChild="deleteTodo" :message="confirmmessage" />
  </div>
</template>

<script lang="ts">
import axios from "axios";
import Vue from "vue";
import TaskUpdate from "~/components/TaskUpdate.vue";
import Confirm from "~/components/Confirm.vue";

export type DefTodo = {
  name: string;
  id: number;
};

export default Vue.extend({
  name: "TableView",
  components: { TaskUpdate, Confirm },

  data: () => ({
    DoneType: {
      false: "未実行",
      true: "実行済み",
    },
    items: {},
    modalshow: false,
    confirm: false,
    updatetodo: {
      user_id: "1",
      text: "",
      id: "",
      done: "false",
    },
    confirmmessage: "",
    key: {},
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
    ShowUpdateModal: function (id: string) {
      if (id != "") {
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
          done: "",
        };
      }
      this.modalshow = true;
    },
    deleteTodo: function () {
      let api = axios({
        url: "/api1/query",
        headers: {},
        method: "POST",
        data: {
          query:
            `mutation deleteTodo{
                 deleteTodo(input:{
                          id:"` +
            this.key +
            `"
                           })
              }`,
        },
      }).then((res) => {
        if (res.status == 200) {
          this.GetTodoALL();
          this.confirm = false;
        } else {
        }
      });
    },
    completeTodo: function (id:string) {
      let api0 = axios({
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
        let api = axios({
          url: "/api1/query",
          headers: {},
          method: "POST",
          data: {
            query:
              `mutation updateTodo{
                      updateTodo(input:{
                        text:"` +
              res.data.data.todo.text +
              `",
                        userId:"` +
              res.data.data.todo.user_id +
              `"
                        id:"` +
              id +
              `"
                        done:` +
              !(res.data.data.todo.done) +
              `
              })
            }`,
          },
        }).then((res) => {
          if (res.status == 200) {
            this.modalshow = false;
            this.GetTodoALL();
          } else {
          }
        });
      });
    },
    ShowConfirm: function (id: string, mode: string) {
      if (mode == "delete") {
        this.confirmmessage = "削除します。";
      }
      this.key = id;
      this.confirm = true;
    },
  },
});
</script>
