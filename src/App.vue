<template>
  <div class="hello">
    <h1>Contact us</h1>

    <div v-if="loading">Loading...</div>
    <ul v-if="!loading">
      <li v-for="user in users" :key="user.id" class="card">
        <img :src="user.avatar" :alt="`${user.first_name} ${user.last_name}`" />
        <span>{{ user.first_name }} {{ user.last_name }}</span>
        <a :href="`mailto:${user.email}`" class="contact-link">Contact</a>
      </li>
    </ul>
    <div class="pagination" v-if="!loading">
      <button @click="prevPage" :disabled="page === 1" class="btn">
        Previous
      </button>
      <span>Page {{ page }}</span>
      <button @click="nextPage" :disabled="page >= totalPages" class="btn">
        Next
      </button>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "HelloWorld",
  data() {
    return {
      users: [],
      page: 1,
      totalPages: 1,
      loading: true,
    };
  },
  created() {
    this.fetchUsers();
  },
  methods: {
    fetchUsers() {
      this.loading = true;
      axios
        .get(`https://reqres.in/api/users`, {
          params: {
            page: this.page,
            per_page: 6,
          },
        })
        .then((response) => {
          this.users = response.data.data;
          this.totalPages = response.data.total_pages;
          this.loading = false;
          console.log(this.users);
        })
        .catch((error) => {
          console.error("There was an error!", error);
          this.loading = false;
        });
    },
    nextPage() {
      if (this.page < this.totalPages) {
        this.page++;
        this.fetchUsers();
      }
    },
    prevPage() {
      if (this.page > 1) {
        this.page--;
        this.fetchUsers();
      }
    },
  },
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Lexend:wght@100..900&display=swap');
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: antiquewhite;
}

body {
  background: linear-gradient(45deg, #000000, #2c3e50, #ffffff);
  margin: 0;
  background-size: 400% 400%;
  animation: gradient 13s ease-in-out infinite;
}
@keyframes gradient {
  0% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}

h1{
  margin: 0;
  padding-top: 2rem;
  font-family: "Lexend", sans-serif;
  font-optical-sizing: auto;
  font-weight: 100;
  font-style: normal;
  mix-blend-mode: difference;

}

ul {
  list-style-type: none;


  /* display: grid; */
  /* grid-template-columns: auto auto auto; */
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 3rem;
}
li {
  /* margin: 10px 0;
  display: flex;
  align-items: center;
  gap: 3rem;
  border-style: solid; */

  /* flex-wrap: wrap; */
  /* flex-direction: column; */
  display: flex;
  box-shadow: rgba(50, 50, 93, 0.25) 0px 50px 100px -20px,
    rgba(0, 0, 0, 0.3) 0px 30px 60px -30px,
    rgba(10, 37, 64, 0.35) 0px -2px 6px 0px inset;
  margin: 2rem;
  width: 300px;
  height: 300px;
  background-color: #0c0c0c62;
  background-image: linear-gradient(45deg, #050c0c8e 0%, #644f649c 100%);
  position: relative;
  border-radius: 4px;
  justify-content: center;
  align-items: center;
  transition: 0.5s ease;

  flex: 0 1 calc(33.333% - 7rem); /* Three columns with space between items */
  box-sizing: border-box; /* Ensure padding and border are included in the width calculation */

  padding: 20px;
  text-align: center;
  justify-content: space-between;
  animation: fade-down 0.5s
}

@keyframes fade-down {
  0% {
    opacity:  0;
    transform: translateY(-30px) scale(0.9);
  }
  100% {
    opacity: 1;
    transform: translate(0px) scale(1);
  }
}

img {
  border-radius: 50%;
  margin-right: 10px;
  width: 125px;
  height: 125px;
}
span {
  margin-right: 10px;

}
.contact-link {
  color: #007bff;
  text-decoration: none;
}
.contact-link:hover {
  text-decoration: underline;
}
.pagination {


  margin-top: 20px;
}


button {
  margin: 0 10px;
}

button.btn {
  background: linear-gradient(45deg, #6a11cb, #2575fc);
  border: none;
  border-radius: 25px;
  color: white;
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
  transition: background 0.3s ease, transform 0.3s ease;
  margin: 0 10px;
}

button.btn:hover {
  background: linear-gradient(45deg, #2575fc, #6a11cb);
  transform: translateY(-2px);
}

button.btn:disabled {
  background: #cccccc;
  cursor: not-allowed;
}

.card {

  transition: 0.5s ease;
}

.card::after {
  content: "";
  border-radius: 4px;
  position: absolute;
  z-index: -1;
  width: 100%;
  height: 100%;

  background-image: linear-gradient(45deg, #081616 0%, #673b689c 100%);
  filter: blur(20px);
  transition: 0.5s ease;
}

.card:hover {
  transform: scale(1.1);
}

.card:hover::after {
  transform: translateY(10%) scale(0.9);
}

@media (max-width: 600px) {
  li {
    flex-direction: column;
    align-items: flex-start;
  }
  img {
    margin-bottom: 10px;
  }
    ul{
    gap: 0;
  }
}
@media (max-width: 729px){
  ul{
    gap: 1.5rem;
  }
}
</style>
