<template>
  <div class="container">
    <div class="mb-3">
      <label for="campeonato" class="form-label">Informe um campeonato .</label>
      <input
        class="form-control"
        v-model="campeonato.nomeCampeonato"
        id="campeonato"
        placeholder="Libertadores "
      />
      <button
        @click="salvarCampeonato()"
        type="button"
        class="btn btn-primary mt-3"
      >
        Salvar
      </button>
    </div>
    <table class="table">
      <thead>
        <tr>
          <th scope="col">#</th>
          <th scope="col">Campeonato</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="campeonato in listaDeCampeonatos" :key="campeonato.id">
          <th scope="row">{{ campeonato.id }}</th>
          <td>{{ campeonato.ds_campeonato }}</td>
          <td>
            <button
              @click="
                showModal = true;
                adicionarTimes(campeonato.id);
              "
              type="button"
              class="btn btn-primary mt-3"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                width="16"
                height="16"
                fill="currentColor"
                class="bi bi-plus-circle-fill"
                viewBox="0 0 16 16"
              >
                <path
                  d="M16 8A8 8 0 1 1 0 8a8 8 0 0 1 16 0zM8.5 4.5a.5.5 0 0 0-1 0v3h-3a.5.5 0 0 0 0 1h3v3a.5.5 0 0 0 1 0v-3h3a.5.5 0 0 0 0-1h-3v-3z"
                />
              </svg>
            </button>
          </td>
        </tr>
      </tbody>
    </table>
    <div>
      <!-- use the modal component, pass in the prop -->
      <modal :show="showModal" @close="showModal = false">
        <template #header>
          <h3>Times</h3>
        </template>
      </modal>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Modal from "./Modal.vue";

export default {
  name: "FormCampeonato",
  components: {
    Modal,
  },
  data: () => ({
    campeonato: {
      nomeCampeonato: "",
    },
    listaDeCampeonatos: [],
    showModal: false,
  }),
  props: {
    msg: String,
  },
  methods: {
    async getCampeonatos() {
      const { data } = await axios.get("http://127.0.0.1:8000/api/campeonato");
      this.listaDeCampeonatos = data;
    },
    adicionarTimes(idCampeonato) {
      console.log(idCampeonato);
    },
    async salvarCampeonato() {
      const ds_campeonato = this.campeonato.nomeCampeonato;
      const dadosCampeonato = {
        ds_campeonato,
      };
      await axios.post("http://127.0.0.1:8000/api/campeonato", dadosCampeonato);
      this.getCampeonatos();
    },
  },
  mounted() {
    this.getCampeonatos();
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
