<template>
  <div class="py-3">
    <v-layout>
      <v-flex xs8>
        <a v-bind:href="repos.http_url_to_repo">
          <h4 v-line-clamp="1" class="font-weight-regular">{{repos.path_with_namespace}}</h4>
        </a>

        <!-- dialog 테스트 -->
           <v-dialog v-model="dialog" width="500">
            <template v-slot:activator="{ on }">
                <v-btn color="green" flat v-on="on" v-on:click="test(repos.owner.username)" >{{repos.owner.name}}님 자세히</v-btn>
             </template>

      <v-card>
        <v-card-title  class="headline grey lighten-2" primary-title>
          <a v-bind:href="'https://lab.ssafy.com/'+repos.owner.username">{{repos.owner.name}}</a>님의 연간 활동

        </v-card-title>

        <v-card-text>

           <!-- 미니그래프 테스트 -->
            <div :id="repos.owner.username" style="height: 250px; width: 90%;"></div>
           <!-- 미니그래프 테스트 끝 -->
           <v-divider></v-divider>
             <h4 class="mt-3">{{ repos.owner.name }}님 프로젝트 More <v-btn fab small v-on:click="test2(flags, repos.owner.username)"> <v-icon> arrow_downward</v-icon></v-btn> </h4>
              <div class="mt-0" v-if="flags">

                  <!-- 테스트 -->
                  <v-card class="mx-auto" max-width="1000">

                    <v-card dark flat>
                      <v-card-title class="pa-2 purple lighten-3">
                        <h3 class="title font-weight-light text-xs-left grow">전체 작업 프로젝트 : {{ totallength }} 개</h3>
                      </v-card-title>
                    </v-card>

                    <v-card-text class="py-0">
                      <v-timeline align-top dense>
                        <v-timeline-item color="pink"small>
                          <v-layout pt-3>
                            <v-flex xs3>
                              <strong>{{ when[0] }}</strong>
                            </v-flex>
                            <v-flex>
                              <strong>{{ three[0] }}</strong>
                            </v-flex>
                          </v-layout>
                        </v-timeline-item>

                        <v-timeline-item
                          color="teal lighten-3"
                          small
                        >
                          <v-layout wrap pt-3>
                            <v-flex xs3>
                              <strong>{{ when[0] }}</strong>
                            </v-flex>
                            <v-flex>
                              <strong>{{ three[1] }}</strong>

                            </v-flex>
                          </v-layout>
                        </v-timeline-item>

                        <v-timeline-item
                          color="pink"
                          small
                        >
                          <v-layout pt-3>
                            <v-flex xs3>
                              <strong>{{ when[0] }}</strong>
                            </v-flex>
                            <v-flex>
                              <strong>{{ three[2] }}</strong>
                            </v-flex>
                          </v-layout>
                        </v-timeline-item>

                        <v-timeline-item
                          color="teal lighten-3"
                          small
                        >
                          <v-layout pt-3>
                            <v-flex xs3>
                              <strong>{{ when[0] }}</strong>
                            </v-flex>
                            <v-flex>
                              <strong>{{ three[3] }}</strong>
                            </v-flex>
                          </v-layout>
                        </v-timeline-item>
                      </v-timeline>
                    </v-card-text>
                  </v-card>




                  <!-- 테스트 끝 -->







              </div>


        </v-card-text>



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

<script src="https://canvasjs.com/assets/script/canvasjs.min.js"></script>
<script>

import GitlabService from "@/services/GitlabService";
const BASE_URL = "https://lab.ssafy.com/api/v4";
var tokens = {'myccpb08':'5yRamVkqs4Z4bq-G1roY', 'Kim_yh':'N9RKhWdxvbGzn3oYEwVe',
                      'JIGyeongmin':'yYcb5LEDsxxbN1PPxKEj', 'LeeSuKyeong':'dCp7MpuwFQNzYrLBZix5', 'seok':'xTftb51x12NTwFbxxAC5'}


function calendar(datas,id){
  var graph = {'1':0,'2':0,'3':0,'4':0,'5':0,'6':0,'7':0,'8':0,'9':0,'10':0,'11':0,'12':0}
  var act = []
  var dataPoints = []
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
    dataPoints.push({x:parseInt(key), y:graph[key]})
  }
  mini(dataPoints,id)


}
}


function mini(data,id) {
var chart = new CanvasJS.Chart(id, {
  animationEnabled: true,
  axisY:{
  gridColor: "white"
},
  data: [{
    yValueFormatString: "#,###",
    xValueFormatString: "#,###",
    type: "spline",
    lineColor: "#6a60a9",
    dataPoints: data,
    lineThickness: 5,
     markerType:'circle',
     markerSize : 4,
     markerColor:'#dedcee'
  }]

});
chart.render();

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
      flags : false,
      three : [],
      link : [],
      when : [],
      totallength:0,
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
                calendar(datass,id)
          })

          } // for문 끝 요청부분 완료

      }} // callback 함수 완료
      request(options, callback);
    }, // getRepose

    test(username) {
      this.getRepos(username, tokens[username]);
    },

    test2(flags,username) {
      this.flags = !flags
      this.getGitlabRepos(username, tokens[username])},

    async getGitlabRepos(userName, token) {
      const response = await GitlabService.getRepos(userName, token)
      if(response.status !== 200) {
        return
      }
      this.totallength = response.data.length
      var ssample = [response.data[1].path_with_namespace,response.data[2].path_with_namespace,response.data[3].path_with_namespace,response.data[4].path_with_namespace ]
      var wwhen = [response.data[1].created_at.substring(2,10),response.data[2].created_at.substring(2,10),response.data[3].created_at.substring(2,10),response.data[4].created_at.substring(2,10) ]
      this.three = ssample
      this.when = wwhen

    },
    } //method
}; // default
</script>
