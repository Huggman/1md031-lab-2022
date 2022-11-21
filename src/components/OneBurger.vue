<template>
    <div id="hamburgarefont">
      <h3>{{burger.name}}, {{burger.pris}} kr</h3>
      <button v-on:click=increase type="button">
        +
      </button>
      <button v-on:click=decrease type="button">
         - 
      </button>
      <h5> Antalet beställda:  {{ amountOrdered }} </h5>

      <img id="burgerpic" v-bind:src="burger.pic"/>
      
      <li v-if="burger.name=='Glenns goa'"> Tryffelmajo, bacon & cheddarost</li>
      <li v-if="burger.name=='Glenns goa'"> En öl ingår</li>
      <li v-if="burger.name=='Änglarna'"> Känns som att du är i himlen när du äter den</li>
      <li v-if="burger.name=='Änglarna'"> 10 kr går till blåvitt</li>
      <li v-if="burger.name=='Frölunda'"> Bacon, BBQ-sås och fett</li>
      <li v-if="burger.name=='Frölunda'"> Bästsäljare</li>
      <li> {{burger.kCal}} kCal </li>
      <li v-if="burger.gluten"> Innehåller <span class="textcolor">gluten</span></li>
      <li v-if="burger.laktos"> Innehåller <span class="textcolor">laktos</span></li>

     
    </div>
  </template>
  
  <script>
  export default {
    name: 'OneBurger',
    props: {
      burger: Object
    },

    data: function () {
  return {
    amountOrdered: 0,
  }
},

    methods: {
      selectThisBurger: function() {
        this.$emit("selected", this.burger.name)
      },
    
    
      increase: function() {
        this.amountOrdered += 1
        this.$emit('orderedBurger', { name:   this.burger.name, 
                                amount: this.amountOrdered 
                              }
  );
      },
      decrease: function() {
        if(this.amountOrdered >= 1){
          this.amountOrdered -= 1
          this.$emit('orderedBurger', { name:   this.burger.name, 
                                amount: this.amountOrdered 
                              }
  );
        }
      }

    }
}
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>

.textcolor {
    color: yellow;
    display: inline-block;
    font-weight: bold;
 }


#headerimg {
   opacity: 0.5;
   height: 250px;
   width: 100%;
   overflow:hidden;
}

#intro {
  position:absolute; 
  padding: 10%; 
  margin-top: -20%; 
  margin-left: 25%;
}

h1 {
   position:absolute;
}



 #burgerpic {
  height: 300px;
  width: 300px;
 }



 #hamburgare {
    border: 5px dashed #fefefe;
    border-radius: 15px;
    background-color: rgb(19, 15, 226);
    color: antiquewhite;
 }



</style>
  