<template>
  <div>
    <div class="container">
      <b-table class="mt-5" striped hover :items="items" :fields="fields">
        <template #cell(#)="data">
          <button class="btn btn-success" @click="onEditForm(data.item)">Edit</button>
          <button class="btn btn-danger" @click="onDelete(data.item)">Delete</button>
        </template>
      </b-table>
    </div>
    <div class="container">
      <b-form @submit="onSubmit" @reset="onReset">
        <b-form-group id="input-group-2" label="Your Nama:" label-for="input-2">
          <b-form-input id="input-2" v-model="form.nama" placeholder="Enter nama" required></b-form-input>
        </b-form-group>

        <b-form-group id="input-group-2" label="Your umur:" label-for="input-2">
          <b-form-input id="input-2" v-model="form.umur" placeholder="Enter umur" required></b-form-input>
        </b-form-group>

        <div class="mt-2">
          <b-button type="submit" variant="primary">Submit</b-button>
          <b-button type="reset" variant="danger">Reset</b-button>
        </div>
      </b-form>
      <div v-if="onEdit" class="mt-5">
        <b-form @submit="onSubmitEdit">
        <b-form-group id="input-group-2" label="Your nama:" label-for="input-2">
          <b-form-input id="input-group-2" v-model="form_edit.nama" placeholder="Enter nama" required></b-form-input>
        </b-form-group>
        <b-form-group id="input-group-2" label="Your umur:" label-for="input-2">
          <b-form-input id="input-group-2" v-model="form_edit.umur" placeholder="Enter umur" required></b-form-input>
        </b-form-group>

        <div class="mt-2">
          <b-button type="submit" variant="primary">Submit</b-button>
        </div>
      </b-form>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
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
      };
    },
    methods: {
      async onDelete(data) {
        await this.$axios.$delete(
          "http://localhost:35000/users/employee/" + data.id
        );
        this.getapi
      },
      async onEditForm(data) {
        this.onEdit = true;
        console.log(data);
        this.form_edit.nama = data.nama;
        this.form_edit.umur = data.umur;
        this.form_edit.id = data.id;
      },
      async getapi() {
        const data = await this.$axios.$get(
          "http://localhost:35000/users/employee",
        );
        this.items = data.data;
        for (let i = 0; i < this.items.length; i++) {
          const element = this.items[i];
          element.id = i + 1;
        }
      },
      async onSubmitEdit(event) {
        event.preventDefault();
        try {
          await this.$axios.$put(
          "http://localhost:35000/users/employee/" + this.form_edit.id,
          this.form_edit
        );
        this.getapi
        } catch(error) {
          console.log(error)
        }
      },
      async onSubmit(event) {
        event.preventDefault();
        await this.$axios.$post(
          "http://localhost:35000/users/employee",
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
    },
  }
</script>
