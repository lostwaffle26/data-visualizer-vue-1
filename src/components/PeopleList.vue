<template>
  <div>
    <h1>Tableau</h1>
    <div class="search">
      <input
        type="text"
        placeholder="Filter by department or employee"
        v-model="filter"
      />
    </div>
    <table id="table">
      <tr>
        <th>First Name</th>
        <th>Last Name</th>
        <th>Genre</th>
      </tr>
      <tr v-for="item in list" v-bind:key="item.id">
        <td>{{ item.firstname }}</td>
        <td>{{ item.lastname }}</td>
        <td>{{ item.gender }}</td>
      </tr>
    </table>

    <div class="pagination-container">
      <nav>
        <ul class="pagination">
          <li data-page="prev">
            <span><span class="sr-only">(current)</span></span>
          </li>
          <!--	Here the JS Function Will Add the Rows -->
          <li data-page="next" id="prev">
            <span><span class="sr-only">(current)</span></span>
          </li>
        </ul>
      </nav>
    </div>
  </div>
</template>

<script>
import { createApp } from "vue";
import axios from "axios";
import VueAxios from "vue-axios";

const app = createApp();

app.use(VueAxios, axios);



export default {
  name: "PeopleList",
  data() {
    return {
      list: undefined,
      current: 1,
      total: 10,
      //   filter: "",
      //   list: this.list.filter((row) => {
      //     const firstname = row.firstname.toLowerCase();
      //     const lastname = row.lastname.toLowerCase();
      //     const searchTerm = this.filter.toLowerCase();

      //     return firstname.includes(searchTerm) || lastname.includes(searchTerm);
      //   }),
    };
  },
  methods: {
    onPageChange(page) {
      this.current = page;
    },
  },
  mounted() {
    app.axios
      .get("https://run.mocky.io/v3/70e5b0ad-7112-41c5-853e-b382a39e65b7")
      .then((resp) => {
        this.list = resp.data.people;
        console.warn(resp.data.people);
      });
  },
};
</script>

<style lang="css" scoped>
#table {
  font-family: Arial, Helvetica, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

#table td,
#table th {
  border: 1px solid #ddd;
  padding: 8px;
}

#table tr:nth-child(even) {
  background-color: #f2f2f2;
}

#table tr:hover {
  background-color: #ddd;
}

#table th {
  padding-top: 12px;
  padding-bottom: 12px;
  text-align: left;
  background-color: #007acc;
  color: white;
}

.pagination li:hover{
    cursor: pointer;
}
		table tbody tr {
			display: none;
		}

</style>