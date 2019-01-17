<template>
  <b-container>
    <b-jumbotron header="Bootstrap Vue" lead="VueJs-BootStrap Boiler Plate">
      <p>{{msg}}</p>
      <b-btn variant="primary" href="#">More Info</b-btn>
    </b-jumbotron>

    <template>
      <b-table
        ref="table1"
        striped
        hover
        :bordered="true"
        :items="items"
        :fields="fields"
        :outlined="true"
      >
        <template slot="age" slot-scope="data">{{randomAge()}}</template>
        <template slot="sex" slot-scope="data">{{randomGender()}}</template>
        <template slot="address" slot-scope="row">
          <div>
            <b-link
              v-b-modal="'myModal'"
              @click.stop="info(row.item, row.index, $event.target)"
            >View Address</b-link>
          </div>
        </template>
      </b-table>
    </template>

    <div class="flex">
      <!-- the modal -->
      <b-modal id="myModal" title="Address">
        <!-- <pre>{{ myModal.content }}</pre> -->
 
          <b-card :title=myModal.title>
              
            <p>City: {{address.city}}</p>
            <p>Street: {{address.street}}</p>
            <p>Apt: {{address.suite}}</p>
            <p>Zip: {{address.zipcode}}</p>
   

            <p class="card-text">
              {{address.geo}}

    </p> 
    
    </b-card>
          <b-card title="Company Information">
              <p><b>Name</b>: {{company.name}}</p>
              <p><b>Core Compentency</b>: {{company.bs}}</p>
              <p><b>Catch Phrase</b>: {{company.catchPhrase}}</p>

          </b-card>
          
          

        <div>
  
</div>

      </b-modal>
    </div>
  </b-container>
</template>



<script>
export default {
  name: "MainPage",
  props: {
    msg: String
  },
  data() {
    return {
      fields: [
        {
          key: "id",
          label: "#",
          sortable: false
        },
        {
          key: "name",
          label: "Full Name",
          sortable: true
        },
        {
          key: "username",
          label: "Usernme",
          sortable: true
        },
        {
          key: "email",
          label: "E-Mail",
          sortable: true
        },
        {
          key: "phone",
          label: "Phone",
          sortable: true
        },
        "age",
        "sex",
        "address"
      ],
      items: this.items,
      address: {},
      company: {},
      myModal: { title: "", content: "" }
    };
  },
  mounted() {
    {
      // GET /someUrl
      this.$http.get("https://jsonplaceholder.typicode.com/users").then(
        response => {
          // get body data
          this.items = response.body;
          console.log(this.items);
        },
        response => {
          // error callback
          //console.log(console.error);
        }
      );
    }
  },
  methods: {
    info(item, index, button) {
      //this.myModal.title = `Row index: ${index}`;
      this.myModal.title = item.name;
      this.myModal.content = JSON.stringify(item, null, 2);
      this.address = item.address;
      this.company = item.company;
      console.log("Street: " + this.address.city);
      this.$root.$emit("bv::show::modal", "myModal", button);
    },
    randomAge(){
       console.log('randomAge logged')
      return parseInt(Math.random() * (100 - 18) + 18)
    },
   randomGender(){
     var genderArray = [
       'Male',
       'Female'
     ];
     var randomNum = Math.floor(Math.random()*genderArray.length);
     console.log(genderArray[randomNum]);
     return genderArray[randomNum];

   }
  },

  computed: {
    getAge() {
      console.log('getAge logged')
      return parseInt(Math.random() * (100 - 18) + 18)
    }
  }
};
</script>

