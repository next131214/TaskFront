<template>
  <div>
    <table class="table-fixed">
      <thead>
        <tr>
          <th class="w-1/2 px-4 py-2">id</th>
          <th class="w-1/4 px-4 py-2">内容</th>
          <th class="w-1/4 px-4 py-2">実行/未実行</th>
        </tr>
      </thead>
      <tbody v-for="column in items" :key="column.id">
        <tr class="bg-gray-100">
          <td class="border px-4 py-2">
            {{ column.id }}
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
    <button @click="GetTodo">更新</button>
    <button @click="GetTodo">登録（未実装）</button>
  </div>
</template>

<script lang="ts">
import axios from "axios";
import Vue from "vue";

export type DefTodo = {
  name: string;
  id: number;
};

export default Vue.extend({
  name: "TableView",
  data: () => ({
    DoneType: {
      false: "未実行",
      true: "実行済み",
    },
    items: {},
  }),
  created() {
    this.GetTodo();
  },
  methods: {
    GetTodo: function (): void {
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
  },
});
</script>
