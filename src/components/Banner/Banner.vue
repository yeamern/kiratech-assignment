<script setup>
import ProfilePicture from "./../../assets/provided-icons/Avatar.png";
import UserInfo from "./UserInfo.vue";
import SendMessageButton from "./SendMessageButton.vue";
import AddFriendButton from "./AddFriendButton.vue";
</script>

<template>
  <div class="banner">
    <div class="sections">
      <div class="section">
        <img :src="ProfilePicture" class="profile" />
      </div>
      <div class="section"><UserInfo /></div>
      <div class="section">
        <SendMessageButton />
      </div>
      <div class="section">
        <AddFriendButton />
      </div>
    </div>
  </div>
</template>

<style scoped>
.banner {
  background-color: #35bad8;
  gap: 1rem;
  padding: 0px 10%;
  align-items: center;
}
.sections {
  display: flex;
  align-items: end;
  height: 100%;
  padding-top: 16px;
}
.section {
  margin: 16px 0px 24px;
  margin-right: 16px;
}
.section:nth-child(2) {
  margin-right: 32px;
}
.section:first-child {
  margin-bottom: 0;
}
.profile {
  height: 148px;
  margin-bottom: -40px;
}
</style>
