<template>
  <div class="home">
    <Navbar></Navbar>

    <div class="container">
      <h1 class="title">Tarefas</h1>

      <div class="create">
        <input type="text" placeholder="titulo" v-model="title" />
        <input type="date" placeholder="data" v-model="date" />
        <input type="text" placeholder="descricao" v-model="description" />

        <button @click="cadatraTask">Cadastrar</button>
      </div>

      <div class="card" v-for="task in tasks" :key="task.id">
        <div class="check">
          <svg
            v-if="task.status == 'pending'"
            @click="changeStatus('completed', task.id)"
            style="cursor: pointer"
            xmlns="http://www.w3.org/2000/svg"
            width="20"
            height="20"
            viewBox="0 0 20 20"
            fill="none"
          >
            <circle cx="10" cy="10" r="9" stroke="#E5E5E5" stroke-width="2" />
          </svg>
          <svg
            v-if="task.status == 'completed'"
            @click="changeStatus('pending', task.id)"
            style="cursor: pointer"
            xmlns="http://www.w3.org/2000/svg"
            width="20"
            height="20"
            viewBox="0 0 20 20"
            fill="none"
          >
            <circle cx="10" cy="10" r="10" fill="black" />
            <path
              d="M15 7L8.125 14L5 10.8182"
              stroke="white"
              stroke-width="1.5"
              stroke-linecap="round"
              stroke-linejoin="round"
            />
          </svg>
        </div>
        <div class="content">
          <h6 class="title-task spacing">{{ task.title }}</h6>
          <p class="description-task spacing">
            {{ task.description }}
          </p>
          <div class="date">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="15"
              height="17"
              viewBox="0 0 15 17"
              fill="none"
            >
              <path
                d="M10.3889 1V3.88889M4.61111 1V3.88889M1 6.77778H14M2.44444 2.44444H12.5556C13.3533 2.44444 14 3.09114 14 3.88889V14C14 14.7977 13.3533 15.4444 12.5556 15.4444H2.44444C1.6467 15.4444 1 14.7977 1 14V3.88889C1 3.09114 1.6467 2.44444 2.44444 2.44444Z"
                stroke="#D31408"
                stroke-width="1.3"
                stroke-linecap="round"
                stroke-linejoin="round"
              />
            </svg>
            <div class="text-date">{{ task.created_at }}</div>
          </div>
        </div>
        <div class="actions">
          <svg
            @click="deleteTask(task.id)"
            xmlns="http://www.w3.org/2000/svg"
            width="17"
            height="18"
            viewBox="0 0 17 18"
            fill="none"
          >
            <path
              d="M1 4.2H2.66667M2.66667 4.2H16M2.66667 4.2V15.4C2.66667 15.8243 2.84226 16.2313 3.15482 16.5314C3.46738 16.8314 3.89131 17 4.33333 17H12.6667C13.1087 17 13.5326 16.8314 13.8452 16.5314C14.1577 16.2313 14.3333 15.8243 14.3333 15.4V4.2M5.16667 4.2V2.6C5.16667 2.17565 5.34226 1.76869 5.65482 1.46863C5.96738 1.16857 6.39131 1 6.83333 1H10.1667C10.6087 1 11.0326 1.16857 11.3452 1.46863C11.6577 1.76869 11.8333 2.17565 11.8333 2.6V4.2M6.83333 8.2V13M10.1667 8.2V13"
              stroke="#81858E"
              stroke-width="1.3"
              stroke-linecap="round"
              stroke-linejoin="round"
            />
          </svg>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";

export default {
  name: "HomeView",
  components: {
    Navbar,
  },
  data() {
    return {
      tasks: [],
      title: "",
      date: null,
      description: ""

    };
  },
  methods: {
    cadatraTask(){
      const someData = {
        title: this.title,
        date: this.date,
        description: this.description,
      };

      fetch("http://192.168.0.109:8001/task", {
        method: "POST",
        headers: {
          "Content-type": "application/json; charset=UTF-8",
        },
        body: JSON.stringify(someData),
      })
        .then((response) => response.json())
        .then((data) => {
          this.title = "";
          this.date = null;
          this.description = "";
          this.fetchTask();
        })
        .catch((err) => console.log(err));
    },
    deleteTask(id) {
      fetch("http://192.168.0.109:8001/task/" + id, {
        method: "DELETE",
        headers: {
          "Content-type": "application/json; charset=UTF-8",
        },
      })
        .then((response) => response.json())
        .then((data) => {
          this.fetchTask();
        })
        .catch((err) => console.log(err));
    },
    changeStatus(status, id) {
      const someData = {
        status: status,
      };

      fetch("http://192.168.0.109:8001/task/" + id, {
        method: "PUT",
        headers: {
          "Content-type": "application/json; charset=UTF-8",
        },
        body: JSON.stringify(someData),
      })
        .then((response) => response.json())
        .then((data) => {
          this.fetchTask();
        })
        .catch((err) => console.log(err));
    },
    fetchTask() {
      fetch("http://192.168.0.109:8001/task")
        .then((resp) => {
          return resp.json();
        })
        .then((resp) => {
          console.log(resp);
          this.tasks = resp.data;
          console.log(this.tasks);
        });
    },
  },
  mounted() {
    this.fetchTask();
  },
};
</script>

<style lang="scss">
.container {
  width: 1200px;
  margin: 0 auto;
  height: 100vh;
}

.title {
  margin-top: 50px;
  margin-bottom: 30px;
}

.card {
  height: 109px;
  width: 100%;
  border: 1px solid #e5e5e5;
  margin-bottom: 20px;
  padding: 15px;
  display: grid;
  grid-template-columns: 50px 1fr 50px;
}

.title-task {
  color: black;
  font-size: 16px;
  font-style: normal;
  font-weight: 400;
  line-height: normal;
}

.description-task {
  color: #81858e;
  font-size: 14px;
  font-style: normal;
  font-weight: 400;
  line-height: normal;
}

.date {
  height: 25px;
  background: rgba(211, 20, 8, 0.1);
  display: flex;
  width: fit-content;
  padding: 3px;
  align-items: center;
}

.text-date {
  padding-left: 10px;
  color: #d31408;
  font-size: 14px;
  font-style: normal;
  font-weight: 500;
  line-height: normal;
}

.spacing {
  margin-bottom: 10px;
}

.actions svg {
  cursor: pointer;
}

.create {
  border: 1px solid #e5e5e5;
  background: #fff;
  box-shadow: 2px 4px 10px 0px rgba(0, 0, 0, 0.05);
  width: 100%;
  padding: 20px;
  margin-bottom: 20px;

}

.create input {
  border: 1px solid #e5e5e5;
  height: 50px;
  color: #81858e;
  display: block;
  font-family: Montserrat;
  font-size: 14px;
  font-style: normal;
  font-weight: 400;
  line-height: normal;
  width: 250px;
  margin-bottom: 10px;
  padding: 0px 10px;
}

.create button {
  width: 122px;
  height: 40px;
  cursor: pointer;
  background: #000;
  color: #fff;
  border: none;
  font-family: Montserrat;
  font-size: 14px;
  font-style: normal;
  font-weight: 600;
  line-height: normal;
}
</style>
