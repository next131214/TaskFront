<template>
  <div>
    <h1>タスク一覧</h1>
    <TableView />
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import TableView from "~/components/TableView.vue";

export default Vue.extend({
  name: "TaskPage",
  components: { TableView },
});

</script>
