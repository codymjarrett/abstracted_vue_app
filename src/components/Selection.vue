<template>
  <div class="selections">
    <div class="selections__section">
      <div class="selections__heading hdg_3">
        <h3>Selection</h3>
      </div>
      <ul class="selections__group">
        <li class="selections__item" v-for="snack in selectedSnack" :key="snack.id">
          <div class="selection__title">{{ snack.product }}</div>
          <div class="selection__count">
            <span>{{ snack.votes }}</span>
          </div>
        </li>
      </ul>
      <ul class="selections__group" v-if="hasNoMoreVotes">
        <li class="selections__item" v-for="snack in theseSnacks" :key="snack.id">
          <div class="selection__title">{{ snack.product }}</div>
          <div class="selection__count">
            <span>{{ snack.votes }}</span>
          </div>
        </li>
      </ul>
      
    </div>
  </div>
</template>

<script>
export default {
  name: "Selection",
  props: ["selectedSnack", "hasNoMoreVotes"],
  data(){
    return {
     theseSnacks: []
    }
  },
  methods: {
    getSnacks: function() {
 this.theseSnacks = JSON.parse(localStorage.getItem('snacks'))

    }
    
  },
  created(){
this.getSnacks()    
  }
 
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.selections {
    padding: 0 1rem;
}
.selections__section .selections__heading {
    padding:  0 0 1rem 1rem;
    border-bottom: 1px solid black; 
}

.selections__group {
    margin: 1rem 0 0;
}

.selections__item {
    display: flex;
    justify-content: space-between;
}

.selections__group .selections__item{
    padding: 1rem 0;
}

.hasNoMoreVotes {
  max-width: 30rem;
  min-width: 21rem;
  margin: auto;
  margin-top: 4rem;
}

.displayNone {
  display: none;
}

@media (min-width: 801px) {

}
</style>
