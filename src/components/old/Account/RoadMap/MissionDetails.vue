<template>
  <div class="mission">
    <div class="mission-name bg_white cl_purple-1">
      <edit class="edit" />
      <p>
        <strong>Миссия №{{ missionData.id }}</strong>
        - {{ missionData.name }}
      </p>
      <exit class="exit" @click="$router.push('/old/account/road_map')" />
    </div>
    <div class="mission-content">
      <div class="lists-block">
        <mission-details-list
          @getData="getTasks"
          :list="goalsData"
          :currentIndex="goalIndex"
          id="goals"
        />
        <mission-details-list
          v-if="tasks"
          @getData="getObj"
          :list="tasksData"
          :currentIndex="taskIndex"
          id="tasks"
        />
        <mission-details-list v-if="obj" :list="objData" id="objectives" />
      </div>
      <div class="tools cl_orange-2 bg_white">
        <p>Инструменты</p>
        <img src="@/assets/images/old/account/tools.png" alt="tools image" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    details: Object
  },
  data: () => ({
    tasks: [],
    obj: [],
    goalIndex: -1,
    taskIndex: -1
  }),
  computed: {
    missionData() {
      return { name: this.details.name, id: this.details.id };
    },
    goalsData() {
      return this.details.goals.map(item => {
        return { name: item.name, id: item.id };
      });
    },
    tasksData() {
      return this.tasks.map(item => {
        return { name: item.name, id: item.id };
      });
    },
    objData() {
      return this.obj.map(item => {
        return { name: item.name, id: item.id };
      });
    }
  },
  methods: {
    getTasks(index) {
      if (this.goalIndex == index) {
        this.goalIndex = -1;
        this.tasks = [];
      } else {
        this.goalIndex = index;
        this.tasks = this.details.goals[index].tasks;
      }

      this.taskIndex = -1;
      this.obj = [];
    },
    getObj(index) {
      if (this.taskIndex == index) {
        this.taskIndex = -1;
        this.obj = [];
      } else {
        this.taskIndex = index;
        this.obj = this.tasks[index].objectives;
      }
    }
  },
  components: {
    exit: () => import("@/assets/images/old/account/exit.svg"),
    edit: () => import("@/assets/images/old/account/edit.svg"),
    MissionDetailsList: () => import("./MissionDetailsList")
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
  max-height: 461px;
  overflow: hidden;
}
.slide-enter,
.slide-leave-to {
  overflow: hidden;
  max-height: 0;
}

.mission {
  margin: 0.5rem 0 0 0.75rem;
}

.mission-name {
  display: flex;
  align-items: center;

  padding: 0 0.25rem 0 0.25rem;
  border-radius: 5px;
  height: 32px;

  margin-bottom: 1rem;

  p {
    font-size: 14px;
    letter-spacing: 0.05em;
    padding-left: 0.25rem;
  }
  .exit {
    margin-left: auto;
    cursor: pointer;
    &:hover {
      path {
        fill: $white-purple-1 !important;
      }
    }
  }
}
.mission-content {
  display: flex;
  flex-flow: row wrap;
}
.lists-block {
  display: flex;
  flex-flow: row wrap;
  justify-content: space-between;

  width: 632px;
}
.tools {
  display: flex;
  align-items: center;
  flex-flow: column nowrap;
  margin-left: auto;

  width: 152px;
  height: 152px;

  box-shadow: 0px 1px 8px rgba(0, 0, 0, 0.1), 0px 2px 4px rgba(0, 0, 0, 0.12),
    0px 1px 2px rgba(0, 0, 0, 0.16);
  border-radius: 5px;

  &:hover {
    cursor: pointer;
    p {
      text-decoration: underline;
    }
  }

  p {
    padding: 0.5rem 0 1rem 0;
    letter-spacing: 0.05rem;
  }
}
@include mq-between($SmPhoneWidth, $SmDesktopWidth) {
  .mission {
    margin-left: 0;
  }
  .tools {
    order: -1;
    margin: 0 0 1rem 0;
  }
}
@include mq-between($SmPhoneWidth, $PhoneWidth) {
  .mission-content {
    justify-content: center;
  }
  .mission-name {
    border-radius: 61px;
    overflow: hidden;
    p {
      overflow: hidden;
      text-overflow: ellipsis;
      flex: 1;
      font-size: 10px;
    }
  }
  .lists-block {
    margin-bottom: 1rem;
    justify-content: center;
    .list {
      width: 94%;
    }
  }
  .list + .list {
    margin-top: 0.5rem;
  }
}
@include mq-between($PhoneWidth, $SmDesktopWidth) {
  .mission {
    margin-left: 0;

    &-name {
      margin-bottom: 0.5rem;
    }
  }
  .lists-block {
    .list {
      width: 32%;
    }
  }
}
</style>
