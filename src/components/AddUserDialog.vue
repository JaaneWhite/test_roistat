<template>
 <dialog id="addUserDialog" v-if="isDialogOpen" class="add-user-dialog">
  <button class="dialog-close-btn" @click="closeDialog">X</button>

  <form method="dialog" @submit="addUser" class="add-user-form">
    <div class="add-user-form-row">
      <label for="nameInput" class="add-user-form-input-label">Имя:</label>
      <input id="nameInput" required v-model="userName" class="add-user-form-input" />
    </div>

    <div class="add-user-form-row">
      <label for="phoneInput" class="add-user-form-input-label">Телефон:</label>
      <input
        id="phoneInput"
        type="tel"
        required
        minlength="13"
        v-model="userPhone"
        @input="addPhoneMask"
        class="add-user-form-input" />
    </div>

    <div class="add-user-form-row">
      <label for="parentSelect" class="add-user-form-input-label">Начальник:</label>
      <select id="parentSelect" v-model="userParent" class="add-user-form-select">
        <option v-for="(option, index) in getUsersPlainList(usersList)" :key="index"  :value="option">{{option.name}}</option>
      </select>
    </div>

    <button type="submit" class="add-user-form-submit-btn">Сохранить</button>
  </form>
 </dialog>
</template>

<script>

export default {
  name: "AddUserDialog",
  props: {
    isDialogOpen: {
      type: Boolean,
      default: false
    },
    usersList: {
      type: Array
    }
  },
  data() {
    return {
      userName: null,
      userPhone: null,
      userParent: null,
    }
  },
  methods: {
    getUsersPlainList(usersList) {
      let usersPlainList = []
      usersList.forEach(user => {
        usersPlainList.push(user)
        if (user.subordinates.length > 0) {
          usersPlainList = [...usersPlainList, ...this.getUsersPlainList(user.subordinates)]
        }
      })
      return usersPlainList
    },
    addUser: function() {

      this.$emit('addUser', {name: this.userName, phone: this.userPhone, parent: this.userParent})
      this.userName = null;
      this.userPhone = null;
      this.userParent = null;
    },
    closeDialog: function () {
      this.$emit('closeDialog')
    },
    addPhoneMask: function() {
      let phoneNumber = this.userPhone
        .replace('+7-', '')
        .replace(/\D/g, '')
      this.userPhone = `+7-${phoneNumber.slice(0, 3)}-${phoneNumber.slice(3, 10)}`
    }
  }
}
</script>

<style scoped>
.add-user-dialog {
  background-color: #E0F1E3;
  border: 2px solid #A3D6B1;
  display: flex;
  flex-direction: column;
  color: #333333;
  .dialog-close-btn {
    width: 20px;
    height: 20px;
    margin-left: auto;
    margin-right: 0;
    background-color: transparent;
    border: none;
    font-weight: bold;
    color: #A3D6B1;
    &:hover {
      cursor: pointer;
    }
  }
  .add-user-form {
    margin-top: 30px;
    display: flex;
    flex-direction: column;
    .add-user-form-row {
      display: flex;
      flex-direction: row;
      justify-content: space-between;
      gap: 20px;
      margin-bottom: 15px;
      .add-user-form-input-label {
        font-weight: 500;
      }
      .add-user-form-input {
        border: 1px solid #A3D6B1;
        height: 30px;
        width: 200px;
      }
      .add-user-form-select {
        width: 200px;
        height: 30px;
        border: 1px solid #A3D6B1;
        background-color: white;
        box-sizing: border-box;
      }
    }
    .add-user-form-submit-btn {
      display: flex;
      margin-left: auto;
      width: 100%;
      height: 40px;
      border: 1px solid #A3D6B1;
      justify-content: center;
      align-items: center;
      background-color: transparent;
      font-size: 14px;
      font-weight: 500;
      color: #333333;
      &:hover {
        cursor: pointer;
        background-color: #A3D6B1;
      }
    }
  }
}
</style>
