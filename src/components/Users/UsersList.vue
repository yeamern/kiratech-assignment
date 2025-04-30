<script setup lang="ts">
import { User } from "@/model/User";
import moment from "moment";
import { onMounted, Ref, watch } from "vue";
import { ref } from "vue";
import UserDetail from "./UserDetail.vue";

const users: Ref<User[]> = ref([]);
const filteredUsers: Ref<User[]> = ref([]);
const isLoading: Ref<boolean> = ref(true);
const searchText: Ref<string> = ref("");
const selectedUser: Ref<User> = ref();
const popupVisible: Ref<boolean> = ref(false);

onMounted(fetchData);

watch(searchText, (newSearchText) => {
  filteredUsers.value = users.value.filter((u) => {
    const sanitizedSearchText = newSearchText.trim().toLowerCase();
    if (getUserName(u).trim().toLowerCase().includes(sanitizedSearchText)) {
      return true;
    }
    if (u.gender.toLowerCase().includes(sanitizedSearchText)) {
      return true;
    }
    if (u.location.country.toLowerCase().includes(sanitizedSearchText)) {
      return true;
    }
    if (u.email.toLowerCase().includes(sanitizedSearchText)) {
      return true;
    }
    return false;
  });
});

watch(users, (newUsers) => {
  searchText.value = "";
  filteredUsers.value = newUsers;
});

function fetchData() {
  isLoading.value = true;
  users.value = [];
  fetch("https://randomuser.me/api/?results=20")
    .then((res) => res.json())
    .then((res) => {
      users.value = res.results;
    })
    .finally(() => (isLoading.value = false));
}

function displayUserDetails(user: User) {
  popupVisible.value = true;
  selectedUser.value = user;
}

function getUserName(user?: User) {
  if (!user) {
    return "";
  }
  return user.name.first + " " + user.name.last;
}

function getUserDate(user?: User) {
  if (!user) {
    return "";
  }
  return moment(user.registered.date).format("DD MMM YYYY");
}

function formatUserGender(user?: User) {
  if (!user) {
    return "";
  }
  return user.gender.charAt(0).toUpperCase() + user.gender.slice(1);
}

function closePopup() {
  popupVisible.value = false;
}
</script>

<template>
  <div class="container">
    <div class="search-container">
      <input type="text" placeholder="Search" v-model="searchText" />
    </div>
    <hr />
    <table>
      <thead>
        <tr>
          <th>Date</th>
          <th>Name</th>
          <th>Gender</th>
          <th>Country</th>
          <th>Email</th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="user of filteredUsers"
          @click="() => displayUserDetails(user)"
        >
          <td>
            {{ getUserDate(user) }}
          </td>
          <td>{{ getUserName(user) }}</td>
          <td>
            {{ formatUserGender(user) }}
          </td>
          <td>{{ user.location.country }}</td>
          <td>{{ user.email }}</td>
        </tr>
        <tr class="status-row" v-if="isLoading">
          <td colspan="5">Loading...</td>
        </tr>
        <tr class="status-row" v-if="!isLoading && !filteredUsers.length">
          <td colspan="5">No data found</td>
        </tr>
      </tbody>
    </table>
    <button class="button-refresh" @click="fetchData" :disabled="isLoading">
      <svg
        width="13"
        height="14"
        viewBox="0 0 13 14"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
      >
        <path
          d="M12.7884 8.35417C12.7884 8.38238 12.7856 8.40213 12.7799 8.41341C12.4188 9.92556 11.6628 11.1514 10.5117 12.0908C9.36068 13.0303 8.01215 13.5 6.46615 13.5C5.64236 13.5 4.84538 13.3448 4.0752 13.0345C3.30501 12.7242 2.61806 12.2812 2.01432 11.7057L0.922526 12.7975C0.815321 12.9047 0.688368 12.9583 0.541667 12.9583C0.394965 12.9583 0.268012 12.9047 0.160807 12.7975C0.0536024 12.6903 0 12.5634 0 12.4167V8.625C0 8.4783 0.0536024 8.35135 0.160807 8.24414C0.268012 8.13694 0.394965 8.08333 0.541667 8.08333H4.33333C4.48003 8.08333 4.60699 8.13694 4.71419 8.24414C4.8214 8.35135 4.875 8.4783 4.875 8.625C4.875 8.7717 4.8214 8.89865 4.71419 9.00586L3.55469 10.1654C3.9553 10.5378 4.40951 10.8255 4.91732 11.0286C5.42513 11.2318 5.95269 11.3333 6.5 11.3333C7.25608 11.3333 7.96137 11.15 8.61589 10.7832C9.2704 10.4165 9.79514 9.91146 10.1901 9.26823C10.2522 9.17231 10.4017 8.84223 10.6387 8.278C10.6838 8.14822 10.7684 8.08333 10.8926 8.08333H12.5176C12.5909 8.08333 12.6544 8.11013 12.708 8.16374C12.7616 8.21734 12.7884 8.28082 12.7884 8.35417ZM13 1.58333V5.375C13 5.5217 12.9464 5.64865 12.8392 5.75586C12.732 5.86306 12.605 5.91667 12.4583 5.91667H8.66667C8.51997 5.91667 8.39301 5.86306 8.28581 5.75586C8.1786 5.64865 8.125 5.5217 8.125 5.375C8.125 5.2283 8.1786 5.10135 8.28581 4.99414L9.45378 3.82617C8.61871 3.05317 7.63411 2.66667 6.5 2.66667C5.74392 2.66667 5.03863 2.85004 4.38411 3.2168C3.7296 3.58355 3.20486 4.08854 2.8099 4.73177C2.74783 4.82769 2.59831 5.15777 2.36133 5.72201C2.31619 5.85178 2.23155 5.91667 2.10742 5.91667H0.423177C0.349826 5.91667 0.28635 5.88987 0.232747 5.83626C0.179145 5.78266 0.152344 5.71918 0.152344 5.64583V5.58659C0.519097 4.07444 1.28082 2.84863 2.4375 1.90918C3.59418 0.969727 4.94835 0.5 6.5 0.5C7.32378 0.5 8.125 0.656576 8.90365 0.969727C9.68229 1.28288 10.3735 1.72439 10.9772 2.29427L12.0775 1.20247C12.1847 1.09527 12.3116 1.04167 12.4583 1.04167C12.605 1.04167 12.732 1.09527 12.8392 1.20247C12.9464 1.30968 13 1.43663 13 1.58333Z"
          fill="white"
        />
      </svg>
      <span>Refresh</span>
    </button>
  </div>
  <UserDetail
    :name="getUserName(selectedUser)"
    :date="getUserDate(selectedUser)"
    :gender="formatUserGender(selectedUser)"
    :country="selectedUser?.location.country"
    :email="selectedUser?.email"
    :visible="popupVisible"
    :closePopup="closePopup"
  />
</template>

<style scoped>
.container {
  padding: 0px 10%;
  margin-top: 64px;
}

.search-container {
  display: flex;
  justify-content: flex-end;
}

.search-container input {
  border: 1px solid lightgray;
  padding: 12px 16px;
  border-radius: 8px;
}

.container hr {
  margin: 32px 0;
  display: block;
  height: 1px;
  border: 0;
  border-top: 1px solid lightgray;
}

table {
  border-collapse: separate;
  border-spacing: 0 16px;
  width: 100%;
}

table th,
table td {
  text-align: left;
}

table th:last-child,
table td:last-child {
  text-align: right;
}

table td {
  padding: 24px 32px;
}

table th {
  padding: 4px 32px;
}

table th,
table td:first-child,
table td:nth-child(3),
table td:last-child {
  color: #bcbcbc;
}

table td:nth-child(2) {
  font-weight: 600;
}

table tbody tr {
  box-shadow: 0px 2px 10px 0px #0000001a;
  border-radius: 8px;
}

table tbody tr.status-row {
  box-shadow: none;
}

table tbody tr.status-row td {
  text-align: center;
}

table tbody tr:hover {
  box-shadow: 0px 0px 4px 0px #35bad8;
  cursor: pointer;
}

table tbody tr:hover td:nth-child(2) {
  color: #35bad8;
}

table tbody tr:hover td:nth-child(3) {
  color: black;
}

.button-refresh {
  display: flex;
  gap: 8px;
  padding: 16px 24px;
  justify-content: center;
  align-items: center;
  background-color: #35bad8;
  color: white;
  border: none;
  border-radius: 8px;
  margin: 16px auto;
  cursor: pointer;
}

.button-refresh span {
  font-weight: 600;
}

.button-refresh:hover {
  background-color: #55d9f6;
}

.button-refresh:disabled {
  background-color: darkgray;
  cursor: not-allowed;
}
</style>
