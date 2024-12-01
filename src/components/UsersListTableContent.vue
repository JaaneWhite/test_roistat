<template>
  <div class="users-list-table-content">
    <template v-for="user in contentUsersList" >
      <div
        :key="user.id"
        class="users-table-row"
      >
        <div
          class="users-table-col name"
          :class="`level-${level}`"
          @click.stop="sortUserSubordinates(user)">
          <span :style="{'margin-left': `${level * 20}px` }">{{user.name}}</span>

        </div>
        <div class="users-table-col phone">{{user.phone}}</div>
      </div>

      <template v-if="user.subordinates.length > 0">
        <users-list-table-content
          :content-users-list="user.subordinates"
          :level="level + 1"
        />
      </template>
    </template>
  </div>
</template>

<script>
export default {
  name: "UsersListTableContent",
  props: {
    contentUsersList: {},
    level: null
  },
  data() {
    return {
      sortableUsersList: []
    }
  },
  mounted() {
    this.sortableUsersList = JSON.parse(JSON.stringify(this.contentUsersList))
  },

  methods: {
    sortUserSubordinates: function (user) {
      this.$emit('sortUserSubordinates', user)
    }
  }

}
</script>

<style scoped>
.users-list-table-content {
  display: flex;
  flex-direction: column;
  width: 100%;
  box-sizing: border-box;
  .users-table-row {
    display: flex;
    flex-direction: row;
    width: 100%;
    border: 1px solid #A3D6B1;
    border-top: none;
    .users-table-col {
      padding: 10px;
      flex-basis: 50%;
      &.name {
        &:hover {
          cursor: pointer;
        }
      }
      &.phone {
        border-left: 1px solid #A3D6B1;
      }
    }
  }
}
</style>
