<template>
  <button class="app__btn" @click="this.modalVisible = true" type="button">
    Создать задание
  </button>
  <Modal v-model:show="modalVisible">
    <AddItem @createTask="createTask" />
  </Modal>
  <h1 class="app__title">Список заданий</h1>
  <List v-bind:tasks="tasks" v-if="!isTaskLoading" />
  <div v-else>Loading...</div>
  <div ref="observer" class="observer"></div>
</template>

<script>
import List from "@/components/List.vue";
import AddItem from "@/components/AddItem.vue";
import Modal from "@/components/Modal.vue";
import axios from "axios";

export default {
  components: {
    List,
    AddItem,
    Modal,
  },
  data() {
    return {
      tasks: [],
      isTaskLoading: false,
      startIndex: 0,
      modalVisible: false,
    };
  },

  methods: {
    async createTask(task) {
      try {
        const PROXY_URL = "https://cors-anywhere.herokuapp.com/";
        const URL = "http://api.staging.umeu.app/test/task/create";
        axios({
          method: "post",
          url: PROXY_URL + URL,
          data: task,
        })
          .then(() => {
            this.startIndex = 0;
            this.fetchTasks(false);
          })
          .catch((err) => alert(err));
      } catch {
        console.log("error");
      } finally {
        this.modalVisible = false;
      }
    },

    async fetchTasks(loadMore) {
      try {
        if (this.startIndex === 0) {
          this.isTaskLoading = true;
        }
        const response = await axios.get(
          `http://api.staging.umeu.app/test/tasks/search?pagingCount=10&pagingAfter=${this.startIndex}`
        );
        if (loadMore && this.startIndex > 0) {
          this.tasks = [...this.tasks, ...response.data.result.offers];
        } else {
          this.tasks = response.data.result.offers;
        }

        this.startIndex += 10;

        if (this.startIndex === 10) {
          this.isTaskLoading = false;
        }
      } catch {
        console.log("error");
      }
    },
  },
  mounted() {
    const options = {
      rootMargin: "0px",
      threshold: 1.0,
    };
    const callback = (entries, observer) => {
      if (entries[0].isIntersecting) {
        this.fetchTasks(true);
      }
    };
    const observer = new IntersectionObserver(callback, options);
    observer.observe(this.$refs.observer);
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
}

#app {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.app__btn {
  margin-top: 20px;
  margin-bottom: 30px;
  background-color: inherit;
  border: 1px solid black;
  padding: 7px;
}

.app__title {
  margin-bottom: 30px;
}

.observer {
  height: 30px;
  background-color: inherit;
}
</style>
