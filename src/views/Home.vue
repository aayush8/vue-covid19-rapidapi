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
              <a href="https://github.com/aayush8/vue-covid19-rapidapi">here</a>
            </h3>
          </li>
        </ul>
      </div>
      <h1 class="display-4 my-4">
        Covid-19 Cases
        <span v-if="!isEmptyObject(countryResult)">
          for {{ countryResult[0].country }}</span
        >
      </h1>
      <div class="input-group mb-3">
        <div class="input-group-prepend">
          <button
            class="btn btn-outline-secondary"
            type="button"
            id="button-addon1"
            v-on:click="fetchOnCondition(country)"
          >
            Search by country ...
          </button>
        </div>
        <input
          v-model="country"
          v-on:keyup.enter="fetchOnCondition(country)"
          type="text"
          class="form-control"
          placeholder=" eg. china"
          aria-label="Example text with button addon"
          aria-describedby="button-addon1"
        />
      </div>
      <template v-if="displayTable">
        <!-- displaying table  logic here -->
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
              <td class="text-center" colspan="2">
                {{ item.country }}
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
      </template>
      <template v-if="isEmptyObject(countryResult) && !displayTable">
        <div class="red-hint">
          <p class="lead">* usa for America</p>
          <p class="lead">* china for China</p>
          <p class="lead">* all for Worldwide</p>
          <p class="lead">* table to display table for all Locations</p>
        </div>
      </template>
      <template v-if="!isEmptyObject(countryResult) && !displayTable">
        <p class="lead red-hint">
          Last updated {{ getRelativeDate(countryResult[0].day) }}
        </p>
        <div class="row">
          <div class="col-sm-4">
            <p class="lead">New Cases: {{ countryResult[0].cases.new }}</p>
            <p class="lead">
              Active Cases: {{ countryResult[0].cases.active }}
            </p>
            <p class="lead">
              Critical Cases: {{ countryResult[0].cases.critical }}
            </p>
            <p class="lead">
              Recovered Cases: {{ countryResult[0].cases.recovered }}
            </p>
            <p class="lead">Total Cases: {{ countryResult[0].cases.total }}</p>
          </div>
          <div class="col-sm-4">
            <p class="lead">New Deaths: {{ countryResult[0].deaths.new }}</p>
            <p class="lead">
              Total Deaths: {{ countryResult[0].deaths.total }}
            </p>
          </div>
          <div class="col-sm-4">
            <p class="lead">Total Tests: {{ countryResult[0].tests.total }}</p>
          </div>
        </div>
      </template>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import 'bootstrap/dist/css/bootstrap.min.css'
import moment from 'moment'

export default {
  name: 'Home',

  async mounted() {
    const res = await fetch('https://covid-193.p.rapidapi.com/statistics', {
      method: 'GET',
      headers: {
        'x-rapidapi-host': 'covid-193.p.rapidapi.com',
        'x-rapidapi-key': 'a7614406e2msh59ee1345507dfb4p186de0jsn2bf93e973b3a',
      },
    })
    const response = await res.json()
    const res2 = await response.response
    await this.updateResult(res2)
  },
  data() {
    return {
      country: '',
      result: [],
      countryResult: {},
      displayTable: false,
    }
  },
  methods: {
    updateResult: function(val) {
      this.result = val
    },
    updateCountryResult: function(val) {
      this.countryResult = val
    },
    fetchCountryResult: async function(country) {
      const res = await fetch(
        'https://covid-193.p.rapidapi.com/statistics?country=' + country,
        {
          method: 'GET',
          headers: {
            'x-rapidapi-host': 'covid-193.p.rapidapi.com',
            'x-rapidapi-key':
              'a7614406e2msh59ee1345507dfb4p186de0jsn2bf93e973b3a',
          },
        }
      )
      const response = await res.json()
      const res2 = await response.response
      await this.updateCountryResult(res2)
    },
    isEmptyObject: function(obj) {
      return Object.keys(obj).length === 0
    },
    logForDebug: function(log) {
      console.log(log)
    },
    getRelativeDate: function(date) {
      return moment(date, 'YYYY-MM-DD').fromNow()
    },
    fetchOnCondition: function(country) {
      if (country === 'table') {
        if (this.result.length > 0) {
          this.displayTable = true
        }
        console.log('Ready to put table logic here ..')
      } else {
        this.displayTable = false
        this.fetchCountryResult(country)
      }
    },
  },
}
</script>

<style lang="scss">
table {
  border: 1px solid black;
}

input {
  max-width: 30em;
}

.red-hint {
  color: red;
  font-style: italic;
}
</style>
