<script setup lang="ts">
import { User } from "@/model/User";
import moment from "moment";
import { onMounted, Ref, watch } from "vue";
import { ref } from "vue";
import UserDetail from "./UserDetail.vue";
import RefreshButton from "./RefreshButton.vue";

type SortBy = "nameAsc" | "nameDes" | "dateAsc" | "dateDes";

const users: Ref<User[]> = ref([]);
const filteredUsers: Ref<User[]> = ref([]);
const isLoading: Ref<boolean> = ref(true);
const searchText: Ref<string> = ref("");
const selectedUser: Ref<User> = ref();
const popupVisible: Ref<boolean> = ref(false);
const sortBy: Ref<SortBy> = ref(null);

onMounted(fetchData);

watch([searchText, sortBy], ([newSearchText, newSortBy]) => {
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

  if (newSortBy == "nameAsc") {
    filteredUsers.value = [...filteredUsers.value].sort((a, b) =>
      a.name.first.localeCompare(b.name.first)
    );
  } else if (newSortBy == "nameDes") {
    filteredUsers.value = [...filteredUsers.value].sort((a, b) =>
      b.name.first.localeCompare(a.name.first)
    );
  } else if (newSortBy == "dateAsc") {
    filteredUsers.value = [...filteredUsers.value].sort((a, b) =>
      moment(a.registered.date).diff(b.registered.date)
    );
  } else if (newSortBy == "dateDes") {
    filteredUsers.value = [...filteredUsers.value].sort((a, b) =>
      moment(b.registered.date).diff(a.registered.date)
    );
  }
});

watch(users, (newUsers) => {
  searchText.value = "";
  filteredUsers.value = newUsers;
});

function fetchData() {
  isLoading.value = true;
  users.value = [];
  sortBy.value = null;
  fetch("https://randomuser.me/api/?results=20")
    .then((res) => res.json())
    .then((res) => {
      users.value = res.results;
    })
    .finally(() => (isLoading.value = false));
  window.scrollTo({ top: 0, left: 0, behavior: "smooth" });
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
      <div class="sort-container">
        <select v-model="sortBy" class="sort">
          <option disabled :value="null">
            <span>Sort By </span>
          </option>
          <option value="nameAsc">
            <span>Name — Ascending</span>
          </option>
          <option value="nameDes">
            <span>Name — Descending</span>
          </option>
          <option value="dateAsc">
            <span>Date — Ascending</span>
          </option>
          <option value="dateDes">
            <span>Date — Descending</span>
          </option>
        </select>
      </div>
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
    <RefreshButton :isLoading="isLoading" :fetchData="fetchData" />
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
  justify-content: space-between;
}

.sort-container {
  border: 1px solid lightgray;
  border-radius: 8px;
  overflow: hidden;
  position: relative;
}

.sort {
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  cursor: pointer;
  border: none;
  outline: none;
  padding: 12px 36px 12px 24px;
  color: gray;
}

.sort-container::after {
  content: "▾";
  color: lightgray;
  position: absolute;
  top: 50%;
  right: 12px;
  transform: translateY(-50%);
  pointer-events: none;
}

.sort option {
  font-size: 14px;
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
</style>
