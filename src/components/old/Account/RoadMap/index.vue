<template>
  <div class="account-road_map">
    <div class="account-banner cl_purple-1 bg_white-purple-1">
      <globe class="globe" />
      <h5>Дорожная карта</h5>
      <exit class="exit" @click="$router.push('/old/account/about')" />
    </div>
    <div class="account-footer bg_white-purple-2">
      <div class="wrapper">
        <component
          v-bind="currentProperties"
          :is="currentComponent"
        ></component>
      </div>
    </div>
  </div>
</template>

<script>
import missions from "./missions";
import MissionDetails from "./MissionDetails";
import MissionsPage from "./MissionsPage";
export default {
  components: {
    globe: () => import("@/assets/images/old/account/globe-2.svg"),
    exit: () => import("@/assets/images/old/account/exit.svg"),
    MissionDetails,
    MissionsPage
  },
  data: () => ({
    currentComponent: "MissionsPage",
    missions: missions,
    details: []
  }),
  computed: {
    missionsList() {
      return this.missions.map(item => {
        return { name: item.name, id: item.id };
      });
    },
    currentProperties() {
      return this.currentComponent == "MissionsPage"
        ? { missionsList: this.missionsList }
        : { details: this.details };
    }
  },
  watch: {
    $route(to) {
      this.currentComponent = this.changePage(to.params.id);
    }
  },
  methods: {
    changePage(id) {
      if (id) {
        const currentId = id.split("_")[1];
        if (currentId > this.missions.length) {
          this.$router.push("/old/account/road_map");
          return;
        }

        if (id == `mission_${currentId}`) {
          this.details = this.missions.filter(item => item.id == currentId)[0];
          return "MissionDetails";
        }
      }

      this.details = [];
      return "MissionsPage";
    }
  },
  mounted() {
    this.currentComponent = this.changePage(this.$route.params.id);
  }
};
</script>

<style lang="scss" scoped>
.account-road_map {
  display: flex;
  flex-flow: column nowrap;
  width: 100%;
  height: inherit;
  border-radius: 5px;
}
.account-banner {
  display: flex;
  align-items: center;

  width: inherit;
  height: 32px;

  padding: 0 0.5rem 0 0.5rem;
  border-radius: 5px 5px 0 0;
  h5 {
    font-weight: normal;
    font-size: 14px;
    line-height: 19px;
    margin-left: 0.5rem;
  }
  .globe path {
    fill: $purple-1 !important;
  }
  .exit {
    margin-left: auto;
    cursor: pointer;
    &:hover {
      path {
        fill: $white-purple-2;
      }
    }
  }
}
.account-footer {
  display: flex;
  width: 100%;
  height: 461px;

  padding-right: 0.25rem;
}
.wrapper {
  overflow-y: scroll;
  width: inherit;
  padding-right: 0.5rem;
  &::-webkit-scrollbar {
    padding: 0.5rem 0;
    width: 0.5rem;
    background-color: $white-purple-1;
    border-radius: 21px;
  }

  &::-webkit-scrollbar-thumb {
    border-radius: 21px;
    background-color: $purple-1;
  }
}
@include mq($SmDesktopWidth, max) {
  .wrapper {
    padding: 0.5rem 0.5rem 0 0.5rem;
  }
}
</style>
