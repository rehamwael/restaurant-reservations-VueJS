<template>
<div id="app">
    <Header />
    <Xrestaurant v-bind:tables="tables"/>
    <div class="new-party">
        <form @submit="newParty" >
            <h3>New Party</h3>
            <p>Fill The following form to allocate a table to a party that is waiting to be seated :</p>
            <br/>
            <p class="errors" v-if="errors.length">
              <ul>
                <li v-bind:key="error" v-for="error in errors">{{ error }}</li>
              </ul>
           </p>
          <label for="NumberOfPeople">Number of People</label>
          <input type="Number" min="1" v-model="NumberOfPeople" name="NumberOfPeople" placeholder="Number of People...">

          <label for="DurationOfStay">Duration of stay</label>
          <input type="Number" v-model="DurationOfStay" name="DurationOfStay" placeholder="Duration of Stay in seconds...">

          <label for="Message">Message</label>
          <textarea type="Text" v-model="Message" name="Message"></textarea>

          <input type="submit" value="Add" class="btn">
        </form>
    </div>
    
  </div>
</template>

<script>
import Xrestaurant from './components/Restaurant'
import Header from './layout/header'

export default {
  name: 'App',
  components: {
    Xrestaurant,
    Header
  },
  data() {
    return {
      tables: [
          {
              id : 0,
              capacity: 10,
              states: 'available',
              duration: '00:00',
              message: '',
              is_available: true,
              people: 0
          },
          {
              id : 1,
              capacity: 8,
              states: 'available',
              duration: '00:00',
              message: '',
              is_available: true,
              people: 0
          },
          {
              id : 2,
              capacity: 10,
              states: 'available',
              duration: '00:00',
              message: '',
              is_available: true,
              people: 0
          },
          {
              id : 3,
              capacity: 6,
              states: 'available',
              duration: '00:00',
              message: '',
              is_available: true,
              people: 0
          },
          {
              id : 4,
              capacity: 2,
              states: 'available',
              duration: '00:00',
              message: '',
              is_available: true,
              people: 0
          },
          {
              id : 5,
              capacity: 6,
              states: 'available',
              duration: '00:00',
              message: '',
              is_available: true,
              people: 0
          },
          {
              id : 6,
              capacity: 4,
              states: 'available',
              duration: '00:00',
              message: '',
              is_available: true,
              people: 0
          },
          {
              id : 7,
              capacity: 12,
              states: 'available',
              duration: '00:00',
              message: '',
              is_available: true,
              people: 0
          }
      ],
      errors: [],
      NumberOfPeople: "",
      DurationOfStay: "",
      Message: ""
    };
  },
  created:function() {
    if(localStorage.getItem("restaurantTables") !== null) {
        this.tables = JSON.parse(localStorage.getItem("restaurantTables"));
    }
    setInterval( () => {
        for(let i = 0; i < this.tables.length; i ++) {
            if(this.tables[i].is_available === false && this.tables[i].duration > 0) {
                this.tables[i].duration--;
            }
            if(this.tables[i].duration == 0){
                  this.tables[i].is_available = true;
            }
        }
    }, 1000);

},
  methods: {
    validateForm(){
      this.errors = [];
        if (this.NumberOfPeople === '' && this.DurationOfStay === '' && this.Message === '') {
          this.errors.push('all fields required');
        }
        if(this.NumberOfPeople > 12){
          this.errors.push('Number of people over restaurant capacity !');
        }
    },
    async newParty(e) {
      e.preventDefault();
      this.validateForm();
        if(!this.errors.length){
          let tableIndex = Xrestaurant.methods.tableOrder(this.NumberOfPeople);
          let table = this.$root.$children[0].$children[1].$children[tableIndex]; 
          let isAvailable = await table.untilAvailable();
            if(isAvailable){
                this.tables[tableIndex].people=this.NumberOfPeople;
                this.tables[tableIndex].duration=this.DurationOfStay;
                this.tables[tableIndex].message=this.Message;
                this.tables[tableIndex].is_available=false;
            }
            this.NumberOfPeople = "";
            this.DurationOfStay = "";
            this.Message = "";
          }
        },
        evictTable: function(tableIndex) {
            console.log(tableIndex);
            this.tables[tableIndex].people = 0;
            this.tables[tableIndex].duration = 0;
            this.tables[tableIndex].is_available = true;
            this.tables[tableIndex].message = '';
        }
  }
}
</script>

<style>
  body{
    margin: 0;
    padding: 0;
    font-family: sans-serif;
    color: #35495e;
  }
  .new-party{
    padding: 0 3em;
  }
  form label {
    display: block;
    margin-bottom: 7px;
    margin-top: 7px;
    font-size: 14px;
    font-weight: bold;
  }
  form input:focus , textarea:focus{
    border: 1px solid #35495e;
  }
  form input , textarea{
    padding: 8px 12px;
    width: 95%;
    border: 1px solid #eee;
    outline: 0;
    margin-bottom: 10px;
    transition: all 0.4s ease-in;
  }
  h3 {
    border-bottom: 1px solid #35495d;
    padding: 10px 0;
    margin: 5px;
  }
  p {
    font-size: 13px;
    margin: 5px 0;
  }
  .new-party {
    padding: 0 3em;
    margin: 0 auto;
    width: 50%;
  }
  form {
    margin-top: 1.5em;
    border: 1px solid #35495d;
    padding: 5px 36px;
    overflow: auto;
  }
  input[type="submit" i] {
      background: #35495d;
      color: #fff;
      text-transform: uppercase;
      margin: 10px 0;
      width: 200px;
      float: right;
      clear: both;
      padding: 10px 0;
      font-size: 13px;
      letter-spacing: 1px;
      cursor: pointer;
      transition: all 0.4s ease-in;
  }
  .errors{
    color: red;
    text-transform: uppercase;
    margin-top: -20px;
  }
  input[type="submit" i]:hover{
    background: #85ebcd;
    color: #35495d;
  }
  @media only screen and (max-width: 990px){
    .new-party {
      padding: 0;
      margin: 0 auto;
      width: 100%;
    } 
  }
</style>
