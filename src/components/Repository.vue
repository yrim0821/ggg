<template>
  <div class="py-3">
    <v-layout>
      <v-flex xs8>
        <a v-bind:href="repos.http_url_to_repo">
          <h2 v-line-clamp="1" class="font-weight-regular">{{repos.path_with_namespace}}</h2>
        </a>

        <!-- dialog 테스트 --> 
           <v-dialog v-model="dialog" width="500">
            <template v-slot:activator="{ on }">
                <v-btn color="green" flat v-on="on" v-on:click="test(repos.owner.username)" >{{repos.owner.name}}</v-btn>
             </template>

      <v-card>
        <v-card-title  class="headline grey lighten-2" primary-title>
          {{repos.owner.name}}님의 연간 활동
        </v-card-title>

        <v-card-text>
           <!-- 미니그래프 테스트 -->
                <v-container fluid>
                  <v-btn >보기</v-btn>
    <v-sparkline
      :value="value"
      :gradient="['#f72047', '#ffd200', '#1feaea']"
      :smooth="radius || false"
      :padding="padding"
      :line-width="width"
      :stroke-linecap="lineCap"
      :gradient-direction="gradientDirection"
      auto-draw
    ></v-sparkline>

  </v-container>

           <!-- 미니그래프 테스트 끝 -->
        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="primary"
            flat
            @click="dialog = false"
          >
            닫기
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

        <!-- dialog 테스트 끝 -->
      </v-flex>
    </v-layout>
  </div>
</template>

<script>
import GitlabService from "@/services/GitlabService";
const BASE_URL = "https://lab.ssafy.com/api/v4";
var tokens = {'myccpb08':'5yRamVkqs4Z4bq-G1roY', 'Kim_yh':'N9RKhWdxvbGzn3oYEwVe',
                      'JIGyeongmin':'yYcb5LEDsxxbN1PPxKEj', 'LeeSuKyeong':'dCp7MpuwFQNzYrLBZix5', 'seok':'xTftb51x12NTwFbxxAC5'}


function calendar(datas){
  var graph = {'1':0,'2':0,'3':0,'4':0,'5':0,'6':0,'7':0,'8':0,'9':0,'10':0,'11':0,'12':0}
  var act = []
  if (datas != undefined) {
    // datas 로 반복문 돌림
    for (let index = 0; datas[index] != null; index++) {
      var data = datas[index].created_at;
      if (data == null) continue;
      var month = new Date(data).getMonth() + 1;
        graph[month]++
}
  for(var key in graph){
    act.push(graph[key])
  }
   
}
}


export default {
  name: "Repository",
  props: {
    repos: { type: null }
  },
  data() {
    return {
      dialog : false,
      stats: {},
      value: [1,1,3,1,],
      width: 3.5,
      radius: 5,
      padding: 4,
      lineCap: 'round',
      gradient: ['#f72047', '#ffd200', '#1feaea'],
      gradientDirection: 'top',
    };
  },
  mounted() {

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
          var datass = [];

          // total_try 만큼 요청보내기
          for (var trying = 0; trying < total_try; trying++) {
            
            fetch(
              `${BASE_URL}/users/${id}/events?namespaces&per_page=100&page=${trying +
                1}&private_token=${token}`
            )
              .then(res => {
                return res.json();
              })
              .then(data => {
                data.forEach(function(value) {
                  datass.push(value);
                });
                calendar(datass)
          })
          
          } // for문 끝 요청부분 완료

      }} // callback 함수 완료
      request(options, callback);
    }, // getRepose

    test(username) {
      this.getRepos(username, tokens[username]);
    },


  calendar(datas){
  var graph = {'1':0,'2':0,'3':0,'4':0,'5':0,'6':0,'7':0,'8':0,'9':0,'10':0,'11':0,'12':0}
  var act = []
  if (datas != undefined) {
    // datas 로 반복문 돌림
    for (let index = 0; datas[index] != null; index++) {
      var data = datas[index].created_at;
      if (data == null) continue;
      var month = new Date(data).getMonth() + 1;
        graph[month]++
}
  for(var key in graph){
    act.push(graph[key])
  }
  this.value = act
   
}
}




    } //method
}; // default
</script>