<template>
    <div class="site-snacks__wrapper">
        <div class="isLoading" v-if="isLoading">Loading...</div>
         <div class="site-snacks__section" v-if="!error">
          <div class="site-snacks__heading" v-if="!isLoading">
              <h2 class="hdg_2">Currently In Stock</h2>
              <p class="copy">Here are the snacks that are available in the Nerdery kitchen now.</p>
          </div>
          
          <div class="site-snacks__group" >
              <ul class="site-snacks__list">
                  <li class="site-snacks__item" v-for="snack in snacks" :key="snack.id">
                      <div class="snack">
                          <div class="snack__number">
                              {{ snack.number }}
                          </div>
                          <div class="site-snack__content">
                              <img :src="snack.image" :alt="snack.brand">
                          </div>
                      </div>
                      <div class="site-snack__details">
                          <h3>{{snack.product}}</h3>
                          <span>{{snack.brand}}</span>
                      </div>
                  </li>
              </ul>
          </div>
      </div>
    </div>
     
    
  
</template>

<script>
export default {
  props: {
      snacks: {
          type: Array,
          required: true
      },
      error: {
          type: Boolean,
      },
       isLoading: {
           type: Boolean
       }
  },
  methods: {
      
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>


.site-snacks__section {
    background-image: url(../../public/media/images/geo.jpg);

}

.site-snacks__heading {
    padding: 2rem 1rem;
    text-align: center;
}

.site-snacks__heading h2 {
    padding: 0 0 1rem; 
}

.snack {
    height: 10rem;
    width: 11.25rem;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: white;
    position: relative;
    box-shadow: 19px 14px 14px -14px rgba(0,0,0,0.54);


}

.snack:after{
    content: '';
    position: absolute;
    top: 3px;
    right: -3px;
    border-bottom: 1.25rem solid rgb(13,195,221);
    border-right: 1.25rem solid transparent;
    border-left: 1.25rem solid transparent;
    transform: rotate(45deg);
}

.snack__number{
    position: absolute;
    z-index: 10;
    color: white;
    top: .5rem;
    right: .875rem;
    font-size: .8rem;
    font-weight: 900;
}

.site-snacks__list {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
}

.site-snacks__item {
    margin-bottom: 2rem;
}


.site-snack__content img {
    width: 8.125rem
}

.site-snack__details {
    margin-top: 1rem;
}

.site-snack__details h3{
    font-size: .9rem;
}

.site-snack__details span{
    font-size: .875rem;
}

@media (min-width: 801px) {
    .site-snacks__list {
    display: grid;
    grid-template-columns: 15rem 15rem 15rem;
    grid-template-rows: 1fr 1fr;
    justify-content: center;
    grid-column-gap: 40px;
    grid-row-gap: 5px;

    }
    .site-snacks__item {
        min-width: 13.0625rem;
    }
    .snack {
        width: 100%;
    }
    .site-snack__details h3{
    font-size: 1rem;
}
}

.isLoading {
    font-size: 2rem;
}
</style>
