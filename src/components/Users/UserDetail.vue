<script setup lang="ts">
const props = defineProps({
  visible: {
    type: Boolean,
    required: true,
  },
  name: {
    type: String,
    required: true,
  },
  date: {
    type: String,
    required: true,
  },
  gender: {
    type: String,
    required: true,
  },
  country: {
    type: String,
    required: true,
  },
  email: {
    type: String,
    required: true,
  },
  closePopup: {
    type: Function,
    required: true,
  },
});

function closePopup() {
  props.closePopup();
}

function preventClosePopup(e: MouseEvent) {
  e.preventDefault();
  e.stopImmediatePropagation();
}
</script>

<template>
  <div class="popup" v-if="props.visible" @click="closePopup">
    <div class="popup-content" @click="(e) => preventClosePopup(e)">
      <button class="close-button" @click="closePopup">
        <svg
          width="17"
          height="17"
          viewBox="0 0 17 17"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            d="M10.0575 8.50013L16.6769 1.88033C17.1077 1.44974 17.1077 0.753533 16.6769 0.322943C16.2463 -0.107648 15.5501 -0.107648 15.1196 0.322943L8.4999 6.94274L1.88044 0.322943C1.44966 -0.107648 0.753669 -0.107648 0.323087 0.322943C-0.107696 0.753533 -0.107696 1.44974 0.323087 1.88033L6.94255 8.50013L0.323087 15.1199C-0.107696 15.5505 -0.107696 16.2467 0.323087 16.6773C0.537672 16.8921 0.819819 17 1.10176 17C1.38371 17 1.66565 16.8921 1.88044 16.6773L8.4999 10.0575L15.1196 16.6773C15.3343 16.8921 15.6163 17 15.8982 17C16.1802 17 16.4621 16.8921 16.6769 16.6773C17.1077 16.2467 17.1077 15.5505 16.6769 15.1199L10.0575 8.50013Z"
            fill="#C4C4C4"
          />
        </svg>
      </button>
      <div class="name">{{ props.name }}</div>
      <table>
        <tbody>
          <tr>
            <td>Date:</td>
            <td>{{ props.date }}</td>
          </tr>
          <tr>
            <td>Gender:</td>
            <td>{{ props.gender }}</td>
          </tr>
          <tr>
            <td>Country:</td>
            <td>{{ props.country }}</td>
          </tr>
          <tr>
            <td>Email:</td>
            <td>{{ props.email }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<style scoped>
.popup {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #000000aa;
  display: flex;
  align-items: center;
  justify-content: center;
}

.popup-content {
  background-color: white;
  width: fit-content;
  max-width: 80%;
  min-width: 40%;
  height: fit-content;
  padding: 32px 48px;
  border-radius: 8px;
  position: relative;
}

.popup-content .close-button {
  background: none;
  border: none;
  appearance: none;
  cursor: pointer;
  position: absolute;
  right: 48px;
  top: 48px;
}

.popup-content .name {
  font-size: 32px;
  font-weight: bold;
}

.popup-content table {
  margin-top: 24px;
  width: 100%;
  border-collapse: separate;
  border-spacing: 0 8px;
}

.popup-content td:first-child {
  color: #bcbcbc;
}
</style>
