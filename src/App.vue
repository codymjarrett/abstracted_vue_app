<template >
  <div class="site-snacks">
    <app-stock :isLoading="isLoading" :error="error" :snacks="currentSnacks"></app-stock>
    <app-vote :error="error" :newSnacks="newSnacks" v-on:changeVote="updateVote($event)"></app-vote>
  </div>
</template>

<script>
import Stock from "./components/Stock";
import Vote from "./components/Vote";

export default {
  name: "app",
  components: {
    "app-stock": Stock,
    "app-vote": Vote
  },
  data() {
    return {
      currentSnacks: [],
      newSnacks: [],
      error: false,
      isLoading: false,
    };
  },
  methods: {
    updateVote: function(vote) {
      const idOfnewVote = vote.data.id;
      const newVote = this.newSnacks.filter(snack => snack.id === idOfnewVote);
      newVote[0].votes = vote.data.votes;
    },
  },
  created() {
    this.isLoading = true;
    this.$http
      .get("http://localhost:3000/snacks", {
        headers: {
          Authorization: "Bearer 33b55673-57c7-413f-83ed-5b4ae8d18827"
        }
      })
      .then(response => {
        this.currentSnacks = response.data.slice(0, 6);
        this.newSnacks = response.data.slice(7, 14).sort((a,b)=> b.votes - a.votes);
        this.isLoading = false;
      }).catch(err => {
        console.log(err)
        this.error = true
      })
  }
};
</script>

<style>
.site-snacks {
    color: #fff;
}

</style>
