<template>
  <div class="home mx-5">
    <b-form @submit="onSubmit" @reset="onReset" v-if="show">
      <b-form-group id="ime" label="Ime:" label-for="ime">
        <b-form-input id="ime" v-model="form.ime" required placeholder="Unesite ime"></b-form-input>
      </b-form-group>

      <b-form-group id="prezime" label="Prezime:" label-for="prezime">
        <b-form-input id="prezime" v-model="form.prezime" required placeholder="Unesite prezime"></b-form-input>
      </b-form-group>

      <b-form-group id="maticniBroj" label="Maticni broj:" label-for="maticniBroj">
        <b-form-input id="maticniBroj" v-model="form.maticniBroj" required placeholder="Unesite maticni broj"></b-form-input>
      </b-form-group>
      
      <b-form-group id="visina" label="Visina:" label-for="visina">
        <b-form-input id="visina" type="number" v-model="form.visina" required placeholder="Unesite visinu"></b-form-input>
      </b-form-group>

      <b-form-group id="tezina" label="Tezina:" label-for="tezina">
        <b-form-input id="tezina" type="number" v-model="form.tezina" required placeholder="Unesite tezinu"></b-form-input>
      </b-form-group>

      <b-form-group id="bojaOciju" label="Boja ociju:" label-for="bojaOciju">
        <b-form-select id="bojaOciju" v-model="form.bojaOciju" :options="bojeOciju" required></b-form-select>
      </b-form-group>

      <b-form-group id="telefon" label="Telefon:" label-for="telefon">
        <b-form-input id="telefon" v-model="form.telefon" required placeholder="Unesite broj telefona"></b-form-input>
      </b-form-group>

      <b-form-group id="input-group-1" label="Email address:" label-for="input-1">
        <b-form-input
          id="input-1"
          v-model="form.email"
          type="email"
          required
          placeholder="Enter email"
        ></b-form-input>
      </b-form-group>

      <b-form-group id="rodjendan" label="Datum rodjenja:" label-for="rodjendan">
        <b-form-input id="rodjendan" type="date" v-model="form.rodjendan" required placeholder="Unesite datum rodjenja"></b-form-input>
      </b-form-group>

      <b-form-group id="prebivaliste" label="Prebivaliste:" label-for="prebivaliste">
        <b-form-select id="prebivaliste" v-model="form.prebivaliste" :options="prebivalista" required></b-form-select>
      </b-form-group>

      <b-form-group id="adresa" label="Adresa:" label-for="adresa">
        <b-form-input id="adresa" v-model="form.adresa" required placeholder="Unesite adresu"></b-form-input>
      </b-form-group>

      <b-button type="submit" variant="primary">Submit</b-button>
      <b-button type="reset" variant="danger">Reset</b-button>
    </b-form>
  </div>
</template>

<script>
export default {
  name: "Home",
  components: {},
  data() {
    return {
      form: {
        email: "",
        name: "",
        bojaOciju: null,
        prebivaliste: null,
        checked: []
      },
      bojeOciju: [
        { text: "Select One", value: null },
        "Plave",
        "Zelene",
        "Kestenjaste"
      ],
      prebivalista: [
        { text: "Select One", value: null },
        "Beograd",
        "Bor",
        "Prijepolje",
        "Pirot",
        "Sombor",
        "Smederevo"
      ],
      show: true
    };
  },
  methods: {
    onSubmit(evt) {
      evt.preventDefault();
      alert(JSON.stringify(this.form));
    },
    onReset(evt) {
      evt.preventDefault();
      // Reset our form values
      this.form.email = "";
      this.form.name = "";
      this.form.food = null;
      this.form.checked = [];
      // Trick to reset/clear native browser form validation state
      this.show = false;
      this.$nextTick(() => {
        this.show = true;
      });
    }
  }
};
</script>
