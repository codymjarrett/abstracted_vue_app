<template>
  <div class="snack-voting__wrapper">
    <div class="snack-voting" v-if="!error">
      <div class="snack-voting__heading">
        <h2 class="hdg_2 mix-hdg_dark">Snack Voting</h2>
        <p class="assideText">Vote on the snacks you want to see in this month's rotation</p>
        <p class="remaining-votes assideText" v-if="freshVotes">3 Remaining</p>
        <p class="remaining-votes assideText" v-else>{{ remainingVotes }} Remaining</p>
        <p
          class="remaining-votes no-votes-left assideText"
          v-if="hasNoMoreVotes"
        >You have exceeded your voting limit, please come back next month!</p>
      </div>
      <div class="snack-voting__section">
        <div class="voting-component">
          <div class="voting-component__title hdg_3">
            <h3>Available Items</h3>
          </div>
          <div class="voting-component__options">
            <ul class="voting-component__group mix-hdg_dark">
              <li class="voting-component__item" v-for="snack in newSnacks" :key="snack.id">
                <div class="voting-component__content">
                  <button v-on:click.prevent="handleVote(snack.id)">
                    <svg>
                      <use xlink:href="#Layer_1" />
                    </svg>
                  </button>
                  <div class="voting-component__item-title">
                    <span class="voting-product">{{ snack.product }}</span>
                    <span class="voting-count">{{ snack.votes }}</span>
                  </div>
                </div>
              </li>
            </ul>
          </div>
        </div>
        <app-selection
          :selectedSnack="selectedSnack"
          :hasNoMoreVotes="hasNoMoreVotes"
          :class="{hasNoMoreVotes: hasNoMoreVotes, }"
        ></app-selection>
      </div>
    </div>
    <app-error v-if="error"></app-error>
  </div>
</template>

<script>
import Error from "./Error";
import Selection from "./Selection";
import { parse } from 'path';

export default {
  name: "Vote",
  components: {
    "app-error": Error,
    "app-selection": Selection
  },
  props: {
    newSnacks: {
      type: Array,
      required: true
    },
    error: {
      type: Boolean
    }
  },
  data() {
    return {
      initialVotes: 3,
      remainingVotes: "",
      hasNoMoreVotes: false,
      freshVotes: false,
      selectedSnack: [],
      snacks: []
    };
  },
  methods: {
    setInitialVotes: function (){
      window.localStorage.setItem("votes", JSON.stringify(this.initialVotes));

    },
    handleVote: function(id) {
      if (!this.hasNoMoreVotes) {
        this.$http({
          method: "post",
          url: `http://localhost:3000/snacks/vote/${id}`,
          headers: {
            Authorization: "Bearer 33b55673-57c7-413f-83ed-5b4ae8d18827"
          }
        })
          .then(res => {
            this.$emit("changeVote", res);
            this.selectedSnack.push(res.data)
            if(this.snacks.length <= 3){
            this.snacks.push(res.data);  
            this.getSnacks();       
            }

          })
          .catch(err => console.log(err));
      }
      while(this.initialVotes > 0) {
        this.initialVotes--
        window.localStorage.setItem("votes", JSON.stringify(this.initialVotes))
        console.log(this.initialVotes)
      }
      this.freshVotes = false;
      
      // let voteCounter;

      // while (parseInt(votes) > 0){
      //   voteCounter = parseInt(window.localStorage.getItem("votes"));
      //   window.localStorage.setItem("votes", --voteCounter);
      // }
      // console.log(votes);
      

      // if (!votes) {
      //   window.localStorage.setItem("votes", "2");
      //   this.remainingVotes = window.localStorage.getItem("votes");
      // } else if (votes === "2") {
      //   window.localStorage.setItem("votes", "1");
      //   this.remainingVotes = window.localStorage.getItem("votes");
      // } else if (votes === "1") {
      //   window.localStorage.setItem("votes", "0");
      //   this.remainingVotes = window.localStorage.getItem("votes");
      //   this.hasNoMoreVotes = true;
      // }
    },
    getSnacks: function(){
      if(this.snacks.length <= 3){
        localStorage.setItem('snacks', JSON.stringify(this.snacks))
      }
    }
  },
  created() {
    const ls = window.localStorage.getItem("votes");
    if (ls === null) {
      this.setInitialVotes();
      this.freshVotes = true;
    }
    this.remainingVotes = ls;
    if (this.remainingVotes === "0" && this.selectedSnack.length === 0) {
      this.hasNoMoreVotes = true;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.snack-voting {
    margin: 2rem 0;
}
.snack-voting__section {
    display: flex;
    flex-direction: column;
}
.snack-voting__heading {
    text-align: center;
}

.snack-voting__heading p {
    padding-top: 1rem;
    font-size: 1.3rem;
}
.voting-component {
    margin: 0 1rem;
}
.voting-component, .selections {
    margin-top: 2rem;
    color: #919191;
}

.voting-component .voting-component__title {
    background-color: #3d8b9f;
    color: #fff;
    padding: 1rem;
}

.voting-component__content{
display: flex;
}

.voting-component__content button {
    flex: 5%;
    border: none;
    height: 50px;
    cursor: pointer;
}
.voting-component__item-title {
    flex: 90%;
    position: relative;
    display: flex;
    justify-content: space-between;
}

.voting-component__item-title .voting-product,
.voting-component__item-title .voting-count {
     align-self: center;
     padding: 0 1rem; 
}

.voting-component__item:nth-child(even) div button{
    background-color: rgb(207,207,207);
}

.voting-component__item:nth-child(odd) div button{
    background-color: rgb(197,197,197);
}

.voting-component__item:nth-child(odd) .voting-component__item-title  {
    background-color: rgb(214,214,214);
}

.voting-component__item:nth-child(even) .voting-component__item-title  {
    background-color: rgb(225,225,225);
}


.voting-component__content svg {
    width: 1rem;
    height: 1rem;
}

@media (min-width: 801px) {
    .snack-voting__wrapper {
        padding: 0 10rem;
    }
    .snack-voting__section {
        flex-direction: row;
        align-items: baseline;
    }
    .voting-component, .selections {
        flex: 1; 
       
    }
}
.remaining-votes.no-votes-left{
     color: red;
    font-weight: 600;
}


</style>
