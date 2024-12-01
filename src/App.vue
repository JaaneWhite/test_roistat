<template>
  <div id="app">
    <div class="main-content">
      <AddUserButton @openAddUserDialog="isAddUserDialogOpen = true" />
      <AddUserDialog
        :users-list="usersList"
        :is-dialog-open="isAddUserDialogOpen"
        @addUser="addUser"
        @closeDialog="isAddUserDialogOpen = false"
      />
      <UsersListTable
        :users-list="usersList"
        @sortUsersList="sortUsersList"
        @sortUserSubordinates="sortUserSubordinates($event)"
      />
    </div>
  </div>
</template>

<script>
import AddUserButton from "./components/AddUserButton.vue";
import UsersListTable from "./components/UsersListTable.vue";
import AddUserDialog from "./components/AddUserDialog.vue";

export default {
  name: 'App',
  components: {
    AddUserDialog,
    UsersListTable,
    AddUserButton
  },
  data() {
    return {
      isAddUserDialogOpen: false,
      usersList: []
  }
  },
  mounted() {
    if(localStorage.getItem('users')) {
      this.usersList = JSON.parse(localStorage.getItem('users'))
    }
  },
  methods: {
    addUser: function (user) {
      if (!user.parent) {
        this.usersList.push({
          id: Symbol('id'),
          name: user.name,
          phone: user.phone,
          subordinates: []
        })
      } else {
        user.parent.subordinates.push({
          id: Symbol('id'),
          name: user.name,
          phone: user.phone,
          subordinates: []
        })
      }

      localStorage.setItem('users', JSON.stringify(this.usersList))
      this.isAddUserDialogOpen = false
    },
    sortUsersList: function () {
      this.usersList.sort((a, b) => a.name.localeCompare(b.name))
    },
    sortUserSubordinates: function (user) {
      user.subordinates.sort((a, b) => a.name.localeCompare(b.name))
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,100;0,300;0,400;0,500;0,700;0,900;1,100;1,300;1,400;1,500;1,700;1,900&family=Ubuntu:ital,wght@0,300;0,400;0,500;0,700;1,300;1,400;1,500;1,700&display=swap');
body {
  padding: 0;
  margin: 0;
}
#app {
  font-family: "Ubuntu", sans-serif;
  color: #333333;
  background-color: #E0F1E3;
  width: 100vw;
  height: 100vh;
  box-sizing: border-box;
}
.main-content {
  width: 100%;
  max-width: 600px;
  margin-left: auto;
  margin-right: auto;
  padding-top: 60px;
  display: flex;
  flex-direction: column;
}
</style>
