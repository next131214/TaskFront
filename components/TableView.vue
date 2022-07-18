<template>
  <div>
    <table class="table-fixed">
      <thead>
        <tr>
          <th class="w-1/2 px-4 py-2">タスク名</th>
          <th class="w-1/4 px-4 py-2">内容</th>
          <th class="w-1/4 px-4 py-2">締切</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td class="border px-4 py-2">aaaa to CSS</td>
          <td class="border px-4 py-2">Adam</td>
          <td class="border px-4 py-2">858</td>
        </tr>
        <tr class="bg-gray-100">
          <td class="border px-4 py-2">
            A Long and Winding Tour of the History of UI Frameworks and Tools and the
            Impact on Design
          </td>
          <td class="border px-4 py-2">Adam</td>
          <td class="border px-4 py-2">112</td>
        </tr>
        <tr>
          <td class="border px-4 py-2">Intro to JavaScript</td>
          <td class="border px-4 py-2">Chris</td>
          <td class="border px-4 py-2">1,280</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script lang="ts">
import Vue from "vue";

export default Vue.extend({
  name: "TableView",
});
</script>
