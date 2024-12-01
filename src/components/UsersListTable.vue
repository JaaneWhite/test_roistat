<template>
  <div class="users-list-table">
    <div class="table-title-row">
      <div class="table-title-item name" @click="sortUsersList">
        Имя
      </div>

      <div class="table-title-item phone">Телефон</div>
    </div>

    <users-list-table-content
      :content-users-list="usersList"
      :level="1"
      @sortUserSubordinates="sortUserSubordinates($event)"
    />
  </div>
</template>

<script>


import UsersListTableContent from "./UsersListTableContent.vue";

export default {
  name: "UsersListTable",
  components: {UsersListTableContent},

  props: {
    usersList: {}
  },
  methods: {
    sortUserSubordinates: function (user) {
      this.$emit('sortUserSubordinates', user)
    },
    sortUsersList: function () {
      this.$emit('sortUsersList')
    }
  }

}
</script>

<style scoped>
.users-list-table {
  display: flex;
  flex-direction: column;
  .table-title-row {
    display: flex;
    flex-direction: row;
    border: 1px solid #A3D6B1;
    width: 100%;
    font-weight: 500;
    .table-title-item {
      width: 100%;
      padding: 10px;
      &.phone {
        border-left: 1px solid #A3D6B1;
      }
      &.name {
        &:hover {
          cursor: pointer;
        }
      }

    }
  }
}
</style>
