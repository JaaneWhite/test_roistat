<template>
  <button class="add-user-button" @click="openAddUserDialog">Добавить</button>
</template>

<script>
export default {
  name: "AddUserButton",
  methods: {
    openAddUserDialog: function() {
      this.$emit("openAddUserDialog")
    }
  }
}
</script>

<style scoped>
 .add-user-button {
   display: flex;
   margin-left: auto;
   margin-bottom: 30px;
   width: 100px;
   height: 40px;
   border: 1px solid #A3D6B1;
   justify-content: center;
   align-items: center;
   background-color: transparent;
   font-size: 16px;
   &:hover {
     cursor: pointer;
     background-color: #A3D6B1;
   }
 }
</style>
