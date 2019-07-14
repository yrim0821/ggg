<template>
  <div class="py-3">
    <v-layout>

      <v-flex xs8>
        <a v-bind:href="repos.http_url_to_repo">
			<h2 v-line-clamp="1" class="font-weight-regular">{{repos.path_with_namespace}}</h2>
		</a>
        <p class="subheading mb-1 grey--text text--darken-1 font-weight-light">{{repos.owner.name}}</p>
		<v-btn flat outline v-on:click="test()">{{repos.owner.name}}</v-btn>

      </v-flex>

    </v-layout>
  </div>
</template>

<script>
import GitlabService from '@/services/GitlabService'

export default {
	name: 'Repository',
	props: {
		repos: {type: null}
	},
	data() {
		return {
			stats: {},
		}
	},
  mounted() {
		// this.test()
  },
	methods: {
	  getRepos(id, token) {
      var request = require("request");
      var headers = {
        "PRIVATE-TOKEN": token
      };
      var options = {
        url: "https://lab.ssafy.com/api/v4/users/" + id + "/events",
        headers: headers
      };
      
      function callback(error, response, body) {
        if (!error && response.statusCode == 200) {
          var howmany = response.headers["x-total"];
          var total_try = parseInt(howmany / 100) + 1;
          var datass = []

          // total_try 만큼 요청보내기
          for (var trying = 0; trying < total_try; trying++) {
            fetch(
              `${BASE_URL}/users/${id}/events?namespaces&per_page=100&page=${trying+1}&private_token=${token}`
            )
              .then(res => {
                return res.json();
              })

              .then(data => {
                data.forEach(function(value){
                  datass.push(value)
                })
                drawChart(datass)
              });
          } // for문 끝 요청부분 완료       
        }
      } // callback 함수 완료
      request(options, callback);
    },
			
		}
	}

</script>
