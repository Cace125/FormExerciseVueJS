<script setup>
  import { ref } from "vue";

  const listOfPeople = ref([]);

  const register = (firstName, lastName, weight, dateOfBirth) => {

    if (firstName === undefined || lastName === undefined || weight === undefined || dateOfBirth === undefined) {
        alert("Por favor llenar todos los campos");
    } else {
    
      if (JSON.parse(localStorage.getItem("TableList") === null)) {
          listOfPeople.value = [];

          listOfPeople.value.push(
            {first_name:firstName,last_name:lastName,weight:weight,date_of_birth:dateOfBirth}
          );

          localStorage.setItem("TableList",JSON.stringify(listOfPeople.value));

      } else {
          const bringData = JSON.parse(localStorage.getItem("TableList"));
          listOfPeople.value = bringData;
          let validator = 0;

          const results = listOfPeople.value.filter(element => {
            if(element.first_name === firstName && element.last_name === lastName){
              validator = 1;
            } 
          });

          if (validator === 1) {
              alert("Persona ya existe");
              
          } else {
             listOfPeople.value.push(
               {first_name:firstName,last_name:lastName,weight:weight,date_of_birth:dateOfBirth}
             );


             localStorage.setItem("TableList",JSON.stringify(listOfPeople.value));
          }
      }
    }
  }

const clearList = () => {
  window.localStorage.clear();
  window.location.reload();  
}

const refresh = () => {
  const bringData = JSON.parse(localStorage.getItem("TableList"));
  listOfPeople.value = bringData;
}

refresh();
</script>

<template>
  <div class="mother">
    <div class="formdiv">
      <div class="form">
        <section>
          <h3>First Name</h3>
          <input type="text" v-model="firstName" class="fields">
        </section>
        <section>
          <h3>Last Name</h3>
          <input type="text" v-model="lastName" class="fields">
        </section>
        <section>
          <h3>Weight</h3>
          <input type="number" v-model="weight" class="fields">
        </section>
        <section>
          <h3>Date of Birth</h3>
          <input type="date" v-model="dateOfBirth" class="fields">
        </section>
        <section class="buttonSection">
          <button @click="register(firstName,lastName,weight,dateOfBirth)" class="button">Registrar</button>
          <button @click="clearList()" class="button">Limpiar</button>
        </section>
      </div>
    </div>
    <div class="tablediv">
      <div class="table">
        <table>
          <tr class="tableHeaders">
            <th>First Name</th>
            <th>Last Name</th>
            <th>Weight</th>
            <th>Date of Birth</th>
          </tr>
          <tr v-for="(name, index) in listOfPeople" :key="index">
            <td>{{ name.first_name }}</td>
            <td>{{ name.last_name }}</td>
            <td>{{ name.weight }}</td>
            <td>{{ name.date_of_birth }}</td>
          </tr>
        </table>
      </div>
    </div>
  </div>
</template>