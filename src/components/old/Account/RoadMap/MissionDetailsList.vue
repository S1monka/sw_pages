<template>
  <div class="list" ref="list">
    <div
      class="list-item"
      :class="{ active: currentIndex == index }"
      v-for="(item, index) in list"
      :key="`item_${item.id}`"
    >
      <div class="title" @click="getData(index)">
        <star v-if="currentListId == 'goals'" class="star" />
        <height v-if="currentListId == 'tasks'" class="height" />
        <info v-if="currentListId == 'objectives'" class="info" />
        <input
          v-if="editId == item.id"
          @blur="editId = -1"
          @keyup.enter="
            () => {
              editId = -1;
              item.name = $event.target.value;
            }
          "
          :value="item.name"
          ref="input"
        />
        <p v-else>{{ item.name }}</p>
      </div>
      <options
        class="options"
        :class="{ active: menuId == item.id }"
        @click="openMenu(item.id)"
      />
      <transition name="slide">
        <div v-if="menuId == item.id" class="options-menu bg_white">
          <div class="edit" @click="editItem(item.id)">
            <edit />
            <p>Редактировать</p>
          </div>
          <div v-if="currentListId != 'objectives'" class="add">
            <add />
            <p>
              Добавить
              <span v-if="currentListId == 'goals'">задачу</span>
              <span v-else>задание</span>
            </p>
          </div>
          <div class="timer">
            <timer />
            <p>Выставить время</p>
          </div>
          <div class="remove">
            <remove />
            <p>Удалить</p>
          </div>
        </div>
      </transition>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    list: Array,
    currentIndex: Number
  },
  data: () => ({
    currentListId: "",
    menuId: -1,
    editId: -1
  }),
  components: {
    star: () => import("@/assets/images/old/account/star.svg"),
    height: () => import("@/assets/images/old/account/height.svg"),
    info: () => import("@/assets/images/old/account/alert-circle.svg"),
    options: () => import("@/assets/images/old/account/options-2-fill.svg"),
    edit: () => import("@/assets/images/old/account/settings.svg"),
    remove: () => import("@/assets/images/old/account/trash.svg"),
    add: () => import("@/assets/images/old/account/plus-circle.svg"),
    timer: () => import("@/assets/images/old/account/timer.svg")
  },
  methods: {
    getData(index) {
      if (this.currentListId == "objectives") return;
      this.$emit("getData", index);
    },
    openMenu(id) {
      this.menuId == id ? (this.menuId = -1) : (this.menuId = id);
    },
    editItem(id) {
      this.editId = id;
      this.menuId = -1;

      this.$nextTick(() => {
        this.$refs.input[0].focus();
      });
    }
  },
  mounted() {
    this.currentListId = this.$refs.list["id"];
  }
};
</script>

<style lang="scss" scoped>
.slide-enter-active {
  transition: all 0.3s ease-out;
}

.slide-leave-active {
  transition: all 0.3s ease-in;
}

.slide-enter-to,
.slide-leave {
  opacity: 1;
  overflow: hidden;
}
.slide-enter,
.slide-leave-to {
  overflow: hidden;
  opacity: 0;
}

.list {
  display: flex;
  flex-flow: column nowrap;
  width: 200px;

  &-item {
    display: flex;
    flex-flow: row nowrap;
    align-items: center;
    position: relative;
    width: auto;
    height: 32px;

    border-radius: 5px;

    padding: 0 0.25rem 0 0.25rem;

    cursor: pointer;

    background-color: $orange-3;
    color: white;

    svg path {
      fill: $white;
    }

    & + & {
      margin-top: 0.5rem;
    }

    &.active {
      background-color: $white !important;
      box-shadow: inset 3px 3px 4px rgba(94, 84, 191, 0.52);
      svg path {
        fill: $orange-2;
      }
      color: $orange-2;
    }
  }

  .title {
    display: flex;
    align-items: center;

    height: 100%;
    flex: 1;
    overflow: hidden;

    p {
      font-size: 14px;
      letter-spacing: 0.05em;
      margin-left: 0.25rem;
      overflow: hidden;
      flex: 1;
      white-space: nowrap;

      text-overflow: ellipsis;
    }
  }
}

.list#tasks {
  .list-item {
    background-color: $blue;
  }
}

.list#objectives {
  .list-item {
    background-color: $golden-yellow;
    color: $blue;
  }
  svg path {
    fill: $blue;
  }
}

.options.active {
  path {
    fill: $orange-2;
  }
  z-index: 2;
}
.options-menu {
  display: flex;
  position: absolute;
  flex-flow: column nowrap;

  right: 0;
  top: 0;

  z-index: 1;

  width: 189px;

  box-shadow: 0px 1px 32px rgba(0, 0, 0, 0.12),
    0px 12px 24px rgba(0, 0, 0, 0.12), 0px 8px 16px rgba(0, 0, 0, 0.12);

  border-radius: 5px;

  padding: 3rem 0 1rem 1.5rem;

  & > div {
    display: flex;
    align-items: center;
    margin-bottom: 1rem;
    cursor: pointer;

    p {
      color: $purple-12;
    }

    svg {
      margin-right: 1rem;
      path {
        fill: $purple-12 !important;
      }
    }

    &:hover {
      p {
        color: $purple-1;
      }
      svg path {
        fill: $purple-1 !important;
      }
    }
  }
}
</style>
