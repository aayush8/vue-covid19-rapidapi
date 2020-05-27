<template>
  <div>
    <div class="d-flex justify-content-between">
      <h1 class="display-4 p-3">Covid-19 for {{ this.$route.params.id }}</h1>
      <h1 class="display-4 p-3">
        <router-link to="/">Go Back</router-link>
      </h1>
    </div>
    <!-- <form action>
      <div class="form-group p-5">
        <input required type="date" id="datepicker" name="datepicker" />
        <button
          class="btn btn-primary btn-lg ml-5"
          @click="
            
            $route.query.datepicker.length == 10
              ? getAPI($route.params.id, $route.query.datepicker)
              : logToConsole(
                  `This is coming from datepicker error or maybe params id error ...`
                )
          "
        >Check</button>
      </div>
    </form>-->

    <div class="container">
      <div class="jumbotron">
        <div>{{getAPI($route.params.id) ? null: null}}</div>
        <div v-for="result in results" :key="result.time">
          <h3>{{ result.time }} | Total Tests = {{ result.tests.total>0? result.tests.total : "NA"}}</h3>
          <ul>
            Cases:
            <li>active: {{ result.cases.active }}</li>
            <li>critical: {{ result.cases.critical }}</li>
            <li>new: {{ result.cases.new }}</li>
            <li>recovered: {{ result.cases.recovered }}</li>
            <li>total: {{ result.cases.total }}</li>
          </ul>
        </div>
      </div>
    </div>
    <!-- {{ this.$route.query.datepicker.length == 10 }} -->
  </div>
</template>

<script>
export default {
  name: "S-country",
  data() {
    return {
      results: []
    };
  },
  methods: {
    updateResult: function(val) {
      this.results = val;
    },
    logToConsole: function(log) {
      console.log(log);
    },
    getAPI: async function(country) {
      const res = await fetch(
        `https://covid-193.p.rapidapi.com/statistics?country=${country}`,
        {
          method: "GET",
          headers: {
            "x-rapidapi-host": "covid-193.p.rapidapi.com",
            "x-rapidapi-key":
              "a7614406e2msh59ee1345507dfb4p186de0jsn2bf93e973b3a"
          }
        }
      );
      const res_json = await res.json();
      //   await console.log(res_json)

      const response = await res_json.response;
      this.updateResult(response);
      await console.log(response);
    }
  }
};
</script>

<style lang="scss">
#datepicker {
  padding: 10px;
  font-size: 1.5em;
}
</style>
