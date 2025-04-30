<script setup></script>

<template>
  <div>
    <button class="add-button">
      <svg
        class="button-icon"
        width="13"
        height="13"
        viewBox="0 0 13 13"
        xmlns="http://www.w3.org/2000/svg"
      >
        <rect x="5.4165" y="0.5" width="2.16667" height="13" rx="1" />
        <rect
          x="13"
          y="5.91669"
          width="2.16667"
          height="13"
          rx="1"
          transform="rotate(90 13 5.91669)"
        />
      </svg>
      <div class="add-friend">Add Friend</div>
    </button>
  </div>
</template>

<style scoped>
.add-button {
  background-color: #35bad8;
  display: flex;
  justify-content: center;
  gap: 10px;
  border: 2px solid #ffffff;
  border-radius: 5px;
  padding: 16px 32px;
  cursor: pointer;
}
.add-friend {
  color: #ffffff;
  font-size: 14px;
  font-weight: 600;
}

.button-icon {
  fill: #ffffff;
}

.add-button:hover {
  background-color: #ffffff;
  border: 2px solid #35bad8;
  color: #35bad8;
}

.add-button:hover .button-icon rect {
  fill: #35bad8;
}
.add-button:hover .add-friend {
  color: #35bad8;
}
</style>
