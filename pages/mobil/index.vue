<template>
    <div>
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
        <b-form-group id="input-group-1" label="Merek Mobil" label-for="input-1">
          <b-form-input id="input-1" v-model="form.merek_mobil" placeholder="Masukan Merek mobil" required>
          </b-form-input>
        </b-form-group>
        <b-form-group id="input-group-2" label="Tipe mobil" label-for="input-2">
          <b-form-input id="input-2" v-model="form.tipe_mobil" placeholder="Masukan tipe mobil" required>
          </b-form-input>
        </b-form-group>
        <b-form-group id="input-group-3" label="Warna Mobil" label-for="input-3">
          <b-form-input id="input-3" v-model="form.warna" placeholder="Masukan warna mobil" required>
          </b-form-input>
        </b-form-group>
        <b-button type="submit" variant="primary">Submit</b-button>
        <b-button type="reset" variant="danger">Reset</b-button>
      </b-form>

      <!-- Form edit -->
      <div v-if="onEdit" class="mt-5">
        <b-form @submit="onSubmitEdit">
          <b-form-group id="input-group-1" label="Barang:" label-for="input-1">
            <b-form-input v-model="form_edit.merek_mobil" value="form.merek_mobil" placeholder="Masukan merek mobil" required>
            </b-form-input>
          </b-form-group>
          <b-form-group id="input-group-2" label="Harga:" label-for="input-2">
            <b-form-input v-model="form_edit.tipe_mobil" value="form.tipe_mobil" placeholder="Masukan tipe mobil" required>
            </b-form-input>
          </b-form-group>
          <b-form-group id="input-group-3" label="kategori:" label-for="input-3">
            <b-form-input v-model="form_edit.warna" value="form.warna" placeholder="Masukan warna mobil" required>
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
    export default {
    data() {
      return {
        form: {
          merek_mobil: "",
          tipe_mobil: "",
          warna: "",
        },
        form_edit: {
          merek_mobil: "",
          tipe_mobil: "",
          warna: "",
        },
        show: true,
        onEdit: false,
        fields: [
          { key: "id", sortable: "true" },
          { key: "merek_mobil", sortable: "true" },
          { key: "tipe_mobil", sortable: "true" },
          { key: "warna", sortable: "true" },
          { key: "#", sortable: "false" },
        ],
        items: [],
      };
    },
    methods: {
        async onDelete(data) {
            await this.$axios.$delete(
                "http://localhost:35000/users/car/" + data.id
            );
            this.getapi();
        },
        async onEditForm(data) {
            this.onEdit = true;
            this.form_edit.merek_mobil = data.merek_mobil;
            this.form_edit.tipe_mobil = data.tipe_mobil;
            this.form_edit.warna = data.warna;
            this.form_edit.id = data.id;
        },
        async getapi() {
            const data = await this.$axios.$get(
                "http://localhost:35000/users/car",
            );
            this.items = data.data;
            for (let i = 0; i < this.items.length; i++) {
                const element = this.items[i]
                element.id = i + 1
            }
            console.log(data.data);
        },
        async onSubmitEdit(event) {
            event.preventDefault();
            try {
                await this.$axios.$put(
                    "http://localhost:35000/users/car/" + this.form_edit.id,
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
          "http://localhost:35000/users/car",
          this.form
        );
        this.getapi();
      },
        onReset(event) {
            event.preventDefault();
         this.form.merek_mobil = "";
            this.form.tipe_mobil = "";
            this.form.warna = "";
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