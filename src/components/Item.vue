<template>
  <li class="task">
    <h1 class="task__title">Название: {{ task.title }}</h1>
    <span class="task__price">Id: {{ task.id }}</span>
    <span class="task__price">Стоимость: {{ task.price }}</span>
    <span class="task__category">Категория: {{ task.category }}</span>
    <time
      v-if="task.dateBegin"
      :datetime="task.dateBegin"
      class="task__date-begin"
      >Дата начала: {{ getDateFormat(task.dateBegin) }}
    </time>
    <time v-if="task.dateEnd" :datetime="task.dateEnd" class="task__date-end"
      >Дата окончания: {{ getDateFormat(task.dateEnd) }}
    </time>
  </li>
</template>

<script>
export default {
  props: {
    task: {
      type: Object,
    },
  },

  methods: {
    getDateFormat(date) {
      const dateFormat = new Date(date);

      let dd = dateFormat.getDate();
      if (dd < 10) {
        dd = "0" + dd;
      }

      let mm = dateFormat.getMonth() + 1;
      if (mm < 10) {
        mm = "0" + mm;
      }

      let yy = dateFormat.getFullYear() % 100;
      if (yy < 10) {
        yy = "0" + yy;
      }

      const hours = dateFormat.getHours();
      const minutes = dateFormat.getMinutes();

      const resultDate =
        dd + "." + mm + "." + yy + "--" + hours + ":" + minutes;

      return resultDate;
    },
  },
};
</script>

<style>
.task {
  display: flex;
  flex-direction: column;
  row-gap: 10px;
  width: 300px;
  padding: 10px;
  box-sizing: border-box;
  border: 1px solid black;
  margin-bottom: 15px;
}

.task__title {
  font-size: 15px;
}
</style>
