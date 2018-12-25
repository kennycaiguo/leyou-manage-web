<template>
  <v-card>
      <v-flex xs12 sm10>
        <v-tree url="/item/category/list"
                :isEdit="isEdit"
                @handleAdd="handleAdd"
                @handleEdit="handleEdit"
                @handleDelete="handleDelete"
                @handleClick="handleClick"
        />
      </v-flex>
  </v-card>
</template>

<script>
  export default {
    name: "category",
    data() {
      return {
        isEdit:true
      }
    },
    methods: {
      handleAdd(node) {
        this.$http.post("/item/category", {
          isParent: node.isParent,
          name: node.name,
          parentId: node.parentId,
          sort: node.sort
        }).then(() => {
          location.reload();
        });
      },
      handleEdit(id, name) {
        this.$http.put("/item/category/" + id, {
          name
        });
      },
      handleDelete(id) {
        this.$http.delete("/item/category/" + id);
      },
      handleClick(node) {
        //console.log(node)
      }
    }
  };
</script>

<style scoped>

</style>
