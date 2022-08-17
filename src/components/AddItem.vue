<template>
  <form class="form" @submit.prevent="addTask">
    <h1>Добавление задачи</h1>
    <input
      v-bind:value="task.title"
      @input="task.title = $event.target.value"
      class="form__input-title"
      type="text"
      placeholder="Введите название"
      required
    />
    <input
      v-bind:value="task.price"
      @input="task.price = $event.target.value"
      class="form__input-price"
      type="number"
      placeholder="Введите цену"
      required
    />
    <label for="input-date-begin">Введите дату начала выполнения</label>
    <input
      v-bind:value="task.dateBegin"
      @input="task.dateBegin = $event.target.value"
      class="form__input-date-begin"
      id="input-date-begin"
      name="input-date-begin"
      type="datetime-local"
    />
    <label for="input-date-end">Введите дату окончания выполнения</label>
    <input
      v-bind:value="task.dateEnd"
      @input="task.dateEnd = $event.target.value"
      class="form__input-date-end"
      id="input-date-end"
      name="input-date-end"
      type="datetime-local"
    />
    <select
      class="form__select-category"
      v-bind:value="task.categoryId"
      @change="task.categoryId = $event.target.value"
      required
    >
      >
      <option disabled value="">Выберите из списка</option>
      <option
        v-for="categorie in categories"
        :key="categorie.id"
        :value="categorie.id"
      >
        {{ categorie.title }}
      </option>
    </select>
    <button class="form__button-submit" type="submit">Добавить задачу</button>
  </form>
</template>

<script>
import axios from "axios";
export default {
  props: {
    task: {
      type: Object,
    },
  },
  data() {
    return {
      categories: [],
      task: {
        title: "",
        price: null,
        dateBegin: "",
        dateEnd: "",
        categoryId: null,
      },
    };
  },

  methods: {
    validateDate() {
      if (this.dateBegin !== "" && this.dateEnd !== "") {
        const start = new Date(this.task.dateBegin) / 1000;
        const end = new Date(this.task.dateEnd) / 1000;
        if (end <= start) {
          return false;
        } else {
          return true;
        }
      }
    },

    addTask() {
      const isValid = this.validateDate();
      if (isValid) {
        this.$emit("createTask", this.task);
        this.task = {
          title: "",
          price: null,
          dateBegin: "",
          dateEnd: "",
          categoryId: null,
        };
      } else {
        alert("Дата окончания не может быть раньше даты начала");
      }
    },
    async getCategories() {
      try {
        const response = await axios.get(
          "http://api.staging.umeu.app/test/categories"
        );
        this.categories = response.data.result.categories;
      } catch {
        console.log("error");
      }
    },
    async submitTask() {
      try {
        const response = await axios.post(
          "http://api.staging.umeu.app/test/task/create"
        );
        this.categories = response.data.result.categories;
      } catch {
        console.log("error");
      }
    },
  },
  mounted() {
    this.getCategories();
  },
};
</script>

<style>
.form {
  display: flex;
  flex-direction: column;
  width: 300px;
  row-gap: 10px;
}
</style>
