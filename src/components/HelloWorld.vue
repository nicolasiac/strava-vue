<template>
  <div class="hello">
    <button v-on:click="stravaLogin()">Login to strava</button>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "HelloWorld",
  props: {
    msg: String
  },
  methods: {
    stravaLogin() {
      location.replace(
        "https://www.strava.com/oauth/authorize?client_id=33030" +
          "&redirect_uri=http://localhost:8080" +
          "&response_type=code"
      );
    }
  },
  mounted() {
    var url = new URL(window.location.href);
    var c = url.searchParams.get("code");
    console.log(c);

    if (c.length > 0) {
      var authurl =
        "https://www.strava.com/oauth/token?client_id=33030" +
        "&client_secret=6822dcd1afced7c8e147fc26ac87299e75962ca0" +
        "&code=" +
        c +
        "&grant_type=authorization_code";

      axios.post(authurl, {}).then(res => {
        var token = res.data.access_token;
        console.log(res);

        var getUrl =
          "https://www.strava.com/api/v3/athlete/activities?access_token=" +
          token;
        //       "https://www.strava.com/api/v3/athlete?access_token=" + token;

        axios.get(getUrl).then(res => {
          console.log(res);
        });
      });
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
