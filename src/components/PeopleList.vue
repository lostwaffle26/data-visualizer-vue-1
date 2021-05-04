<template>
  <div>
    <div class="container">
      <div class="search">
        <label>Filter par nom ou prénom:</label>
        <input type="text" placeholder="Nom ou prénom" v-model="filter" />
      </div>

      <div class="pagination">
        <div class="col">
          <label>Page actuelle : </label>
          <input v-model="currentPage" type="number" />
        </div>

        <div class="page">
          lignes à afficher
          <select v-model="pageSize" placeholder="nombre de lignes à afficher">
            <option>5</option>
            <option>15</option>
            <option>25</option>
            <option>50</option>
          </select>
          <button @click="prevPage()" class="btn-3">Précédant</button>
          <button @click="nextPage()" class="btn-3">Suivant</button>
        </div>
      </div>
    </div>
    <div class="export-field">
      <button class="btn-3 export-button" @click="saveFile()">
        Exporter au format CSV
      </button>
    </div>
  </div>

  <div class="table-style">
    <table id="table">
      <tr>
        <th>Prénom</th>
        <th>Nom</th>
        <th>Genre</th>
        <th>Email</th>
        <th>Adresse</th>
        <th>Ville</th>
        <th>Pays</th>
        <th>Téléphone</th>
        <th>Animal pref</th>
        <th>fruit pref</th>
        <th>couleur pref</th>
        <th>film pref</th>
      </tr>
      <tr v-for="(item, key) in pagedList" :key="key">
        <td>{{ item.firstname }}</td>
        <td>{{ item.lastname }}</td>
        <td>{{ item.gender }}</td>
        <td>{{ item.contact.email }}</td>
        <td>{{ item.contact.address }}</td>
        <td>{{ item.contact.city }}</td>
        <td>{{ item.contact.country }}</td>
        <td>{{ item.contact.phone }}</td>
        <td>{{ item.preferences.favorite_pet }}</td>
        <td>{{ item.preferences.favorite_fruit }}</td>
        <td>{{ item.preferences.favorite_color }}</td>
        <td>{{ item.preferences.favorite_movie }}</td>
      </tr>
    </table>
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
      list: [],
      currentPage: "1",
      pageSize: "5",
      filter: "",
    };
  },
  mounted() {
    app.axios
      .get("https://run.mocky.io/v3/70e5b0ad-7112-41c5-853e-b382a39e65b7")
      .then((resp) => {
        this.list = resp.data.people;
        console.warn(resp.data.people);
      });
  },
  methods: {
    nextPage() {
      if (this.currentPage * this.pageSize < this.list.length)
        this.currentPage++;
    },

    prevPage() {
      if (this.currentPage > 1) this.currentPage--;
    },

    saveFile() {
      let dataStr = JSON.stringify({ ...this.paginedList });
      let dataUri =
        "data:application/json;charset=utf-8," + encodeURIComponent(dataStr);

      let exportFileName = "list.json";

      let linkElement = document.createElement("a");
      linkElement.setAttribute("href", dataUri);
      linkElement.setAttribute("download", exportFileName);
      linkElement.click();
    },
  },
  computed: {
    pagedList() {
      return this.filteredRows.filter((row, index) => {
        let first = (this.currentPage - 1) * this.pageSize;
        let last = this.currentPage * this.pageSize;
        if (index >= first && index < last) return true;
      });
    },
    filteredRows() {
      return this.list.filter((row) => {
        const firstname = row.firstname.toLowerCase();
        const lastname = row.lastname.toLowerCase();
        const searchTerm = this.filter.toLowerCase();
        return firstname.includes(searchTerm) || lastname.includes(searchTerm);
      });
    },
  },
};
</script>

<style lang="css">
.container {
  display: flex;
  flex-direction: column;
  padding: 10px;
}

.pagination {
  padding: 10px;
}

input {
  padding: 10px;
}

select {
  padding: 10px;
}

button {
  background-color: #5200b1;
  border: none;
  color: white;
  padding: 16px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 10px;
  margin: 4px 2px;
  transition-duration: 0.4s;
  cursor: pointer;
  border: 1px solid white;
}

.btn-3:hover {
  background-color: #030303;
}

.search {
  display: flex;
  padding: 10px;
}

.table-style {
  display: flex;
  justify-content: center;
}

#table {
  font-family: Arial, Helvetica, sans-serif;
  border-collapse: collapse;
  width: 90%;
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
  background: linear-gradient(#5200b1, #120035);
  color: white;
}

select,
input {
  width: 100%;
  padding: 16px 20px;
  border: none;
  border-radius: 4px;
  background-color: #f1f1f1;
}
select {
  font-size: 15px;
  width: 10vh;
}

.col input {
  width: 80px;
}

.export-button {
  margin-bottom: 20px;
}
</style>
