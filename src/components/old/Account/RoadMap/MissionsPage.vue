<template>
  <div class="missions cl_purple-1">
    <div class="missions-add_mission">
      <add class="add" />
      <h6>Добавить миссию</h6>
    </div>
    <div class="missions-list">
      <div
        class="missions-list_item bg_white"
        v-for="(item, index) in missionsList"
        :key="`mission_${index}`"
      >
        <div class="title" @click="getDetails(item.id)">
          <input
            v-if="editId == item.id"
            @blur="editId = -1"
            @keyup.enter="() => {editId = -1; item.name = $event.target.value }"
            :value="item.name"
            ref="input"
            class="cl_purple-1"
          />
          <h6 v-else>{{ item.name }}</h6>
        </div>
        <options class="options" :class="{ active: menuId == item.id }" @click="openMenu(item.id)" />
        <transition name="slide">
          <div v-if="menuId == item.id" class="options-menu bg_white">
            <div class="edit" @click="editItem(item.id)">
              <edit />
              <p>Редактировать</p>
            </div>
            <div class="timer">
              <timer />
              <p>Установить сроки</p>
            </div>
            <div class="graph">
              <graph />
              <p>Построить график</p>
            </div>
            <div class="remove">
              <remove />
              <p>Удалить</p>
            </div>
          </div>
        </transition>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    missionsList: Array
  },
  data: () => ({
    menuId: -1,
    editId: -1
  }),
  components: {
    add: () => import("@/assets/images/old/account/plus-circle.svg"),
    options: () => import("@/assets/images/old/account/options-2-fill.svg"),
    edit: () => import("@/assets/images/old/account/settings.svg"),
    remove: () => import("@/assets/images/old/account/trash.svg"),
    graph: () => import("@/assets/images/old/account/bar-chart-2.svg"),
    timer: () => import("@/assets/images/old/account/timer.svg")
  },
  methods: {
    openMenu(id) {
      this.menuId == id ? (this.menuId = -1) : (this.menuId = id);
    },
    editItem(id) {
      this.editId = id;
      this.menuId = -1;

      this.$nextTick(() => {
        this.$refs.input[0].focus();
      });
    },
    getDetails(id) {
      this.$router.push({
        path: `/old/account/road_map/mission_${id}`
      });
    }
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
h6 {
  font-weight: bold;
  font-size: 14px;
  line-height: 16px;
  letter-spacing: 0.05em;
}
.missions {
  display: flex;
  flex-flow: column nowrap;
  margin: 1rem 0 0 1rem;

  &-add_mission {
    display: flex;
    align-items: center;
    width: fit-content;
    cursor: pointer;
    margin: 0 0 1rem 2rem;

    .add {
      margin-right: 0.25rem;
      path {
        fill: $purple-1 !important;
      }
    }
  }
}
.missions-list {
  &_item {
    display: flex;
    position: relative;
    align-items: center;
    cursor: pointer;

    padding: 0.5rem 0.25rem 0.5rem 0.5rem;
    border-radius: 5px;
    height: 32px;

    .title {
      flex: 1;
      height: inherit;
      display: flex;
      align-items: center;
      overflow: hidden;

      h6 {
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
      }
    }

    &:hover {
      box-shadow: 0px 1px 8px rgba(0, 0, 0, 0.1),
        0px 2px 4px rgba(0, 0, 0, 0.12), 0px 1px 2px rgba(0, 0, 0, 0.16);
    }

    & + & {
      margin-top: 0.5rem;
    }
  }
}
.options.active {
  path {
    fill: $orange-2 !important;
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
@include mq-between($SmPhoneWidth, $SmDesktopWidth) {
  .missions {
    margin: 0;

    &-add_mission {
      margin: 0;
      margin-bottom: 0.5rem;
    }
  }
}
@include mq-between($SmPhoneWidth, $PhoneWidth) {
  .missions-list_item {
    border-radius: 61px;
  }
}

@include mq-between($PhoneWidth, $SmDesktopWidth) {
  .missions {
    &-add_mission {
      margin-left: 2rem;
    }
    &-list_item {
      border-radius: 5px;
    }
  }
}
</style>
