<template>
  <div class="my-2 mx-4">
    <b-row>
      <b-col cols="2">
        <b-form @submit="sacuvajOsobu" id="forma">
          <b-form-group id="ime" label="Ime:" label-for="ime" class="my-1">
            <b-form-input id="ime" v-model="form.ime" required placeholder="Unesite ime"></b-form-input>
          </b-form-group>

          <b-form-group id="prezime" label="Prezime:" label-for="prezime" class="my-1">
            <b-form-input id="prezime" v-model="form.prezime" required placeholder="Unesite prezime"></b-form-input>
          </b-form-group>

          <b-form-group id="maticniBroj" label="Maticni broj:" label-for="maticniBroj" class="my-1">
            <b-form-input id="maticniBroj" v-model="form.maticniBroj" required placeholder="Unesite maticni broj"></b-form-input>
          </b-form-group>
          
          <b-form-group id="visina" label="Visina:" label-for="visina" class="my-1">
            <b-form-input id="visina" type="number" v-model="form.visina" placeholder="Unesite visinu"></b-form-input>
          </b-form-group>

          <b-form-group id="tezina" label="Tezina:" label-for="tezina" class="my-1">
            <b-form-input id="tezina" type="number" v-model="form.tezina" placeholder="Unesite tezinu"></b-form-input>
          </b-form-group>

          <b-form-group id="bojaOciju" label="Boja ociju:" label-for="bojaOciju" class="my-1">
            <b-form-select id="bojaOciju" v-model="form.bojaOciju" :options="bojeOciju"></b-form-select>
          </b-form-group>

          <b-form-group id="telefon" label="Telefon:" label-for="telefon" class="my-1">
            <b-form-input id="telefon" v-model="form.telefon" required placeholder="Unesite broj telefona"></b-form-input>
          </b-form-group>

          <b-form-group id="email" label="Email adresa:" label-for="email" class="my-1">
            <b-form-input id="email" v-model="form.email" type="email" required placeholder="Unesite email adresu"></b-form-input>
          </b-form-group>

          <b-form-group id="rodjendan" label="Datum rodjenja:" label-for="rodjendan" class="my-1">
            <b-form-input id="myDate" type="date" v-model="form.rodjendan" required placeholder="Unesite datum rodjenja"></b-form-input>
          </b-form-group>

          <b-form-group id="prebivaliste" label="Prebivaliste:" label-for="prebivaliste" class="my-1">
            <b-form-select id="prebivaliste" v-model="form.prebivaliste" :options="prebivalista" required></b-form-select>
          </b-form-group>

          <b-form-group id="adresa" label="Adresa:" label-for="adresa" class="my-1">
            <b-form-input id="adresa" v-model="form.adresa" placeholder="Unesite adresu"></b-form-input>
          </b-form-group>

          <b-button type="submit" variant="primary" class="m-2">Sacuvaj</b-button>
          <b-button variant="outline-primary" class="m-2" @click="reset">Reset</b-button>
        </b-form>
      </b-col>
      <b-col cols="">
        <b-table id="my-table" class="mt-4" striped hover :items="items" :fields="columns" :per-page="perPage" :current-page="currentPage">
          <template v-slot:cell(akcija)="data">
            <b-button @click="izaberi(data.item.id)" variant="primary">Izaberi</b-button>
            <b-button @click="obrisiOsobu(data.item.id)" variant="danger" class="ml-2">Obrisi</b-button>
          </template>
        </b-table>
        <b-pagination  v-model="currentPage" :total-rows="rows" :per-page="perPage" aria-controls="my-table"></b-pagination>
      </b-col>
    </b-row>
    <b-modal id="modal" title="Obavestenje" ok-only="">
      <p class="my-4">Uspesno ste sacuvali osobu!</p>
    </b-modal>
    <b-modal id="modal-greska" title="Obavestenje" ok-only="">
      <p class="my-4">Format unesenih vrednosti nije ispravan!</p>
    </b-modal>
    <b-modal id="model-delete" title="Obavestenje" ok-only="">
      <p class="my-4">Uspesno ste obrisali osobu!</p>
    </b-modal>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: "Home",
  components: {},
  data() {
    return {
      form: {
        id: null,
        ime: "",
        prezime: "",
        maticniBroj: "",
        visina: "",
        tezina: "",
        bojaOciju: null,
        telefon: "",
        email: "",
        rodjendan: "",
        prebivaliste: null,
        adresa: ""
      },
      bojeOciju: [
        { text: "Izaberite jedno", value: null },
        "plave",
        "zelene",
        "kestenjaste"
      ],
      prebivalista: [
        { text: "Izaberite jedno", value: null },
        "Beograd",
        "Bor",
        "Prijepolje",
        "Pirot",
        "Sombor",
        "Smederevo"
      ],
      items: [],
      columns: [
        { key: "id", label: "Id" },
        { key: "ime", label: "Ime" },
        { key: "prezime", label: "Prezime" },
        { key: "maticniBroj", label: "JMBG" },
        { key: "visina", label: "Visina" },
        { key: "tezina", label: "Tezina" },
        { key: "bojaOciju", label: "Boja ociju" },
        { key: "telefon", label: "Telefon" },
        { key: "email", label: "Email" },
        { key: "rodjendan", label: "Datum rodjenja" },
        { key: "prebivaliste", label: "Prebivaliste" },
        { key: "adresa", label: "Adresa" },
        { key: "akcija", label: "Akcija" }
      ],
      currentPage: 1,
      perPage: 10
    };
  },
  computed: {
    rows() {
      return this.items.length;
    }
  },
  created() {
    this.vratiOsobe();
  },
  methods: {
    sacuvajOsobu() {
      console.log(this.form);
      var osoba = {
        Id: this.form.id,
        Ime: this.form.ime,
        Prezime: this.form.prezime,
        MaticniBroj: this.form.maticniBroj,
        Visina: parseInt(this.form.visina),
        Tezina: parseInt(this.form.tezina),
        BojaOciju: this.form.bojaOciju,
        Telefon: this.form.telefon,
        Email: this.form.email,
        Rodjendan: this.form.rodjendan,
        Prebivaliste: this.form.prebivaliste,
        Adresa: this.form.adresa
      }
      const headers = {
        'Content-Type': 'application/json'
      }
      if(this.form.id == null) {
        axios.post("http://localhost:5000/osoba/dodajOsobu", osoba, { headers : headers})
        .then(() => {
          this.$bvModal.show("modal");
          this.reset();
          this.vratiOsobe();
        })
        .catch(err => {
          this.$bvModal.show("modal-greska");
          console.log(err);
        });
      } else {
        axios.post("http://localhost:5000/osoba/izmeniOsobu", osoba, { headers : headers})
        .then(() => {
          this.$bvModal.show("modal");
          this.reset();
          this.vratiOsobe();
        })
        .catch(err => {
          this.$bvModal.show("modal-greska");
          console.log(err);
        });
      }
      
    },
    vratiOsobe() {
      axios
        .get("http://localhost:5000/osoba/vratiOsobe")
        .then(res => {
          res.data.forEach(this.formatDate);
          this.items = res.data;
        })
        .catch(() => {
        });
    },
    obrisiOsobu(id) {
      axios
        .delete("http://localhost:5000/osoba/obrisiOsobu/" + id)
        .then(() => {
          this.$bvModal.show("modal-delete"); 
          this.vratiOsobe();
        })
        .catch(error => {
          console.log(error);
        });
    },
    reset() {
      document.getElementById("forma").reset();
      this.form = {
        id: null,
        ime: "",
        prezime: "",
        maticniBroj: "",
        visina: "",
        tezina: "",
        bojaOciju: null,
        telefon: "",
        email: "",
        rodjendan: "",
        prebivaliste: null,
        adresa: ""
      }
    },
    izaberi(id) {
      axios
        .get("http://localhost:5000/osoba/vratiOsobu/" + id)
        .then(res => {
          res.data.rodjendan = res.data.rodjendan.substring(0,10);
          this.form = res.data;
        })
        .catch(error => {
          console.log(error);
        });
    },
    formatDate(item) {
      item.rodjendan = item.rodjendan.substring(0,10);
    }
  }
};
</script>
