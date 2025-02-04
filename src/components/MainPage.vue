<template>
  <div id="app">
    <h1>Форма выбора</h1>
    <form @submit.prevent="saveForm">
      <label for="city">Город:</label>
      <select id="city" v-model="selectedCity">
        <option value="">Выберите город</option>
        <option v-for="city in Object.keys(data)" :key="city" :value="city">
          {{ city }}
        </option>
      </select>
      <br /><br />

      <label for="workshop">Цех:</label>
      <select id="workshop" v-model="selectedWorkshop" :disabled="!selectedCity">
        <option value="">Выберите цех</option>
        <option v-for="workshop in cityWorkshops" :key="workshop" :value="workshop">
          {{ workshop }}
        </option>
      </select>
      <br /><br />

      <label for="employee">Сотрудник:</label>
      <select id="employee" v-model="selectedEmployee" :disabled="!selectedWorkshop">
        <option value="">Выберите сотрудника</option>
        <option v-for="employee in workshopEmployees" :key="employee" :value="employee">
          {{ employee }}
        </option>
      </select>
      <br /><br />

      <label for="team">Бригада:</label>
      <select id="team" v-model="selectedTeam">
        <option value="">Выберите бригаду</option>
        <option v-for="team in teams" :key="team" :value="team">
          {{ team }}
        </option>
      </select>
      <br /><br />

      <label for="shift">Смена:</label>
      <select id="shift" v-model="selectedShift">
        <option value="">Выберите смену</option>
        <option v-for="shift in shifts" :key="shift" :value="shift">
          {{ shift }}
        </option>
      </select>
      <br /><br />

      <button type="submit">Сохранить</button>
    </form>
  </div>
</template>

<script>
/* eslint-disable */
export default {
  data() {
    return {
      data: {
        Москва: {
          "Цех 1": ["Иванов", "Петров"],
          "Цех 2": ["Сидоров", "Кузнецов"],
        },
        "Санкт-Петербург": {
          "Цех 3": ["Смирнов", "Попов"],
          "Цех 4": ["Васильев", "Соколов"],
        },
      },
      selectedCity: "",
      selectedWorkshop: "",
      selectedEmployee: "",
      selectedTeam: "",
      selectedShift: "",
      teams: ["Бригада 1", "Бригада 2", "Бригада 3"],
      shifts: ["Дневная", "Ночная"],
    };
  },
  computed: {
    cityWorkshops() {
      return this.selectedCity ? Object.keys(this.data[this.selectedCity]) : [];
    },
    workshopEmployees() {
      return this.selectedWorkshop
        ? this.data[this.selectedCity][this.selectedWorkshop]
        : [];
    },
  },
  methods: {
    saveForm() {
      const formData = {
        city: this.selectedCity,
        workshop: this.selectedWorkshop,
        employee: this.selectedEmployee,
        team: this.selectedTeam,
        shift: this.selectedShift,
      };
      document.cookie = `formData=${JSON.stringify(formData)}; path=/;`;
      alert("Данные сохранены в Cookie");
    },
  },
};
</script>

<style scoped>
h1 {
  color: #333;
}

select {
  padding: 5px;
  margin: 5px 0;
}

button {
  padding: 8px 12px;
  background-color: #42b983;
  color: white;
  border: none;
  cursor: pointer;
}

button:hover {
  background-color: #369f74;
}
</style>
