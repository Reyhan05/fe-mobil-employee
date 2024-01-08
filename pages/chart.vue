<template>
   <div class="container">
    <div class="row">
      <div class="col-md-6 d-flex align-items-center">
         <simple-chart v-if="!loading" :data="items"></simple-chart>
       </div>
  
       <div class="col-md-6 myChart">
         <!-- <donut-chart v-if="!loading" :data="items"></donut-chart> -->
         <dougnat-chart v-if="!loading" :data="items"></dougnat-chart>
       </div>
    </div>

   <div class="container border mt-5">
     <b-table striped hover :items="items" :fields="fields">
       <template #cell(#)="data">
           <button class="btn btn-success" @click="onEditForm(data.item)">
           Edit
         </button>
         <button class="btn btn-danger" @click="onDelete(data.item)">
           Delete
         </button>
       </template>
     </b-table>
   </div>
   <div class="container">
     <b-form @submit="onSubmit" @reset="onReset">
       <b-form-group id="input-group-1" label="Nama" label-for="input-1">
         <b-form-input id="input-1" v-model="form.nama" placeholder="Masukan Nama" required>
         </b-form-input>
       </b-form-group>
       <b-form-group id="input-group-3" label="Umur" label-for="input-3">
         <b-form-input id="input-3" v-model="form.umur" placeholder="Masukan Umur" required>
         </b-form-input>
       </b-form-group>
       <b-button type="submit" variant="primary">Submit</b-button>
       <b-button type="reset" variant="danger">Reset</b-button>
     </b-form>
    
     <!-- Form edit --> 
     <div v-if="onEdit" class="mt-5">
       <b-form @submit="onSubmitEdit">
         <b-form-group id="input-group-1" label="Nama:" label-for="input-1">
           <b-form-input v-model="form_edit.nama" value="form.nama" placeholder="Masukan Nama" required>
           </b-form-input>
         </b-form-group>
         <b-form-group id="input-group-2" label="Umur:" label-for="input-2">
           <b-form-input v-model="form_edit.umur" value="form.umur" placeholder="Masukan umur" required>
           </b-form-input>
         </b-form-group>
           <b-button type="submit" variant="primary">Submit</b-button>
       </b-form>
     </div>
     <!-- end form edit -->
 
   </div>
 </div>
 </template>
 
 <script>
import DougnatChart from '~/components/DougnatChart.vue';
   export default {
  components: { DougnatChart },
   data() {
     return {
       form: {
         nama: "",
         umur: "",
       },
       form_edit: {
         nama: "",
         umur: "",
       },
       show: true,
       onEdit: false,
       fields: [
         { key: "id", sortable: "true" },
         { key: "nama", sortable: "true" },
         { key: "umur", sortable: "true" },
         { key: "#", sortable: "false" },
       ],
       items: [],
       show: true,
       loading: false
     };
   },
   methods: {
       async onDelete(data) {
           await this.$axios.$delete(
               "http://localhost:3333/users/employee/" + data.id
           );
           this.getapi();
       },
       async onEditForm(data) {
           this.onEdit = true;
           this.form_edit.nama = data.nama;
           this.form_edit.umur = data.umur;
           this.form_edit.id = data.id;
       },
       async getapi() {
           this.loading = true
           const data = await this.$axios.$get( "http://localhost:3333/users/employee");
           this.items = data.data;
           for (let i = 0; i < this.items.length; i++) {
               const element = this.items[i]
               element.id = i + 1
           }
           console.log(data.data);
           this.loading = false
       },
       async onSubmitEdit(event) {
           event.preventDefault();
           try {
               await this.$axios.$put(
                   "http://localhost:3333/users/employee/" + this.form_edit.id,
                   this.form_edit
               );
               this.getapi();
           }
           catch (error) {
               console.log(error);
           }
       },
       async onSubmit(event) {
        event.preventDefault();
        await this.$axios.$post(
         "http://localhost:3333/users/employee",
         this.form
       );
       this.getapi();
     },
       onReset(event) {
           event.preventDefault();
           this.form.nama = "";
           this.form.umur = "";
           this.show = false;
           this.$nextTick(() => {
               this.show = true;
           });
       },
   },
   mounted() {
       this.getapi();
   }
 }
 </script>