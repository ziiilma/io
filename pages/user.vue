<template lang="pug">
  #user
    v-container.pb-0
      v-layout(row wrap mt-5 pt-3)
        v-flex(shrink class="mr-4 mb-4")
          v-card(max-width="250")
            v-img(min-width="250" src="https://dummyimage.com/400x400/f1f1f1/f1f1f1" v-if="!user.avatar")
            v-img(min-width="250" :src="user.avatar" v-if="user.avatar")
              //- TODO: need lang data in players Index
              //- v-avatar(size="40" tile)
              //-   v-img(:src="us.langUrl('php')" class="white elevation-2")
            v-card-title()
              h3(class="headline") {{ user.name }}
            v-card-text(class="user__card-text pt-0" v-if="user.company")
              v-icon(size="20" class="mr-2") people
              span {{ user.company }}
            v-card-text(class="user__card-text pt-0" v-if="user.location")
              v-icon(size="20" class="mr-2") place
              span {{ user.location }}
            v-card-text(class="user__card-text pt-0" v-if="user.blog")
              v-icon(size="20" class="mr-2") public
              a(:href="user.blog" target="_blank") {{ user.blog }}
        v-flex.mb-4.pa-0(grow)
          h3(class="headline mt-1 mb-3") Bot performance in the last 10 days
          Chart.elevation-1(:values="userChartData" username="kezlya")
      v-flex(grow)
        h3(class="headline mt-1 mb-3") User games:
          //TODO: use v-on span(class="grey--text") ({{ totalGames }})
        gamesTable(:Filters="filters" :PageSize=17)
</template>

<script>
import Chart from '@/components/chart';
import gamesTable from "@/components/gamesTable";
import userService from "@/services/User";
import axios from "axios";

export default {
  name: 'user',
  data: () => ({
    status: null,
    user: {},
    us: userService,
    filters: [],
    userChartData: [
      ['Jan 1',        938,         522],
      ['Jan 2',       1120,        599],
      ['Jan 3',       1167,        587],
      ['Jan 4',      1110,        615],
      ['Jan 5',       691,         629],
      ['Jan 6',       712,         810],
      ['Jan 7',       810,         950],
      ['Jan 8',       950,         1100],
      ['Jan 9',       1100,         1350],
      ['Jan 10',       1200,         1150]
    ],
  }),
  created() {
    var username = this.$route.query.username;

    if(username == '') {
      this.$router.push('/leaderboard')
    }

    this.filters = [{ "term":  { "Players.Username.keyword": username}}];
    this.us.getUserdata(username).then((result) => {
      this.user = result
    })
  },
  components: {
    gamesTable,
    Chart
  }
}
</script>
<style>
.user__card-text {
  width: 100%;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
</style>
