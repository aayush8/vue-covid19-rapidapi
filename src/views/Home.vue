<template>
  <div class="home">
    <div class="container-fluid mt-4">
      <div class="alert alert-info" role="alert">
        <ul>
          <li>
            <h3>
              used
              <a href="https://rapidapi.com/api-sports/api/covid-193">api</a>
            </h3>
          </li>
          <li>
            <h3>
              code
              <a href="https://github.com/aayush8/vue-covid19-rapidapi/tree/prod1">here</a>
            </h3>
          </li>
        </ul>
      </div>
      <h1 class="display-4">
        Covid-19 Cases => Last updated:
        {{ this.result.length > 0 ? this.result[0].day : '' }}
      </h1>
      <table class="table table-responsive-sm table-striped table-dark">
        <thead>
          <tr>
            <th scope="col" colspan="2" class="text-center">Location</th>
            <!-- <th scope="col"></th> -->
            <th scope="col">Total Cases</th>
            <th scope="col">New Cases</th>
            <th scope="col">Active Cases</th>
            <th scope="col">Critical Cases</th>
            <th scope="col">Recovered Cases</th>
            <th scope="col">Total Deaths</th>
            <th scope="col">New Deaths</th>
            <th scope="col">Total Tests</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in result" :key="item.country">
            <td class="onecountry text-center" colspan="2">
              <!-- <router-link :to="{ path: `/country/${item.country}` }">{{ -->
              <a href="#">{{item.country}}</a>
              <!-- }}</router-link> -->
            </td>
            <td>{{ item.cases.total }}</td>
            <td>{{ item.cases.new }}</td>
            <td>{{ item.cases.active }}</td>
            <td>{{ item.cases.critical }}</td>
            <td>{{ item.cases.recovered }}</td>
            <td>{{ item.deaths.total }}</td>
            <td>{{ item.deaths.new }}</td>
            <td>{{ item.tests.total }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import "bootstrap/dist/css/bootstrap.min.css";

export default {
  name: "Home",
  async mounted() {
    const res = await fetch("https://covid-193.p.rapidapi.com/statistics", {
      method: "GET",
      headers: {
        "x-rapidapi-host": "covid-193.p.rapidapi.com",
        "x-rapidapi-key": "a7614406e2msh59ee1345507dfb4p186de0jsn2bf93e973b3a"
      }
    });
    const response = await res.json();
    const res2 = await response.response;
    await this.updateResult(res2);
    // await console.log(res2)
  },
  data() {
    return {
      result: []
    };
  },
  methods: {
    updateResult: function(val) {
      this.result = val;
    }
  }
};
</script>

<style lang="scss">
table {
  border: 1px solid black;
}
.onecountry {
  a {
    color: white;
  }
  cursor: pointer;
  &:hover {
    background-color: #fff;
    a {
      color: black;
    }
  }
}
</style>
