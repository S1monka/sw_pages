<template>
  <div class="current-people">
    <div class="current-people__title">ЛИЧНЫЙ РEЙТИНГ</div>
    <div class="current-people__avatar">
      <img src="/images/old/mentors/profile.jpg" alt />
    </div>
    <div class="current-people__name">Артём Грачёвский</div>
    <div class="current-people__count">1056858 очков</div>
    <div class="current-people__position-count">1089</div>
    <div class="current-people__position-name">место</div>
  </div>
</template>

<script>
export default {
  name: "PersonCurrent"
};
</script>

<style lang="scss" scoped>
.current-people {
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 100%;
  padding: 16px;
  border-radius: 5px;
  overflow: hidden;
  box-shadow: 0px 0px 5px 0px rgba(0, 0, 0, 1);
  &__title {
    color: $violet;
    font-size: 14px;
    font-weight: bold;
    line-height: 1em;
    text-transform: uppercase;
  }
  &__avatar {
    margin-top: 16px;
    width: 112px;
    height: 112px;
    border-radius: 50%;
    overflow: hidden;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 5px;
    position: relative;
    img {
      border-radius: 50%;
      position: relative;
      height: 100%;

      object-fit: cover;
    }
    &::before {
      z-index: 0;
      content: "";
      position: absolute;
      left: 0;
      top: 0;
      width: 100%;
      height: 100%;
      background: linear-gradient(
        0deg,
        rgba(255, 135, 33, 1) 0%,
        rgba(247, 233, 105, 1) 100%
      );
    }
  }
  &__name {
    max-width: 100px;
    margin-top: 14px;
    color: $orange;
    font-size: 18px;
    line-height: 1.28em;
    text-align: center;
  }
  &__count {
    margin-top: 3px;
    color: $violet;
    font-size: 12px;
    line-height: 1em;
  }
  &__position-count {
    margin-top: 11px;
    font-size: 36px;
    line-height: 1em;
    color: $orange;
  }
  &__position-name {
    font-size: 12px;
    line-height: 1em;
    color: $orange;
  }
}
@include mq($SmDesktopWidth, max) {
  .current-people {
    background-color: white;
  }
}
@include mq($PhoneWidth, max) {
  .chart-wraper .chart-wraper-current .current-people {
    margin-top: 16px;
    max-width: 176px;
    margin-left: auto;
    margin-right: auto;
    // background-color: white !important;
  }
}
@include mq($SmPhoneWidth, max) {
  .current-people {
    &__avatar,
    &__name,
    &__position-count {
      margin-top: 8px;
    }
  }
}
</style>
