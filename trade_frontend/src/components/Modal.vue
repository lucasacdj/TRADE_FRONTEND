<template>
  <Transition name="modal">
    <div v-if="showModal" class="modal-mask">
      <div class="modal-wrapper">
        <div class="modal-container">
          <div class="modal-header">
            <slot name="header">default header</slot>
          </div>

          <div class="modal-body">
            <table class="table">
              <thead>
                <tr>
                  <th scope="col">#</th>
                  <th scope="col">Campeonato</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="time in listaDeTimes" :key="time.id">
                  <th scope="row">{{ time.id }}</th>
                  <td>{{ time.ds_time }}</td>
                </tr>
              </tbody>
            </table>
            <table class="table" v-if="campeoes.length">
              <thead>
                <tr>
                  <th scope="col">Time</th>
                  <th scope="col">Colocação</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="classificacao in campeoes" :key="classificacao.id">
                  <th scope="row">{{ classificacao.id_time }}</th>
                  <th scope="row">{{ classificacao.colocacao }}</th>
                </tr>
              </tbody>
            </table>
          </div>

          <div class="modal-footer">
            <slot name="footer">
              <button
                class="modal-default-button btn btn-danger mt-3"
                @click="
                  $emit('close');
                  limparCampeoes();
                "
              >
                Fechar
              </button>
              <button
                class="modal-default-button btn btn-primary mt-3"
                @click="salvarChave()"
              >
                Salvar
              </button>
            </slot>
          </div>
        </div>
      </div>
    </div>
  </Transition>
</template>

<script>
import axios from "axios";

export default {
  name: "ModalVue",
  data: () => ({
    listaDeTimes: [],
    chave: [],
    campeoes: [],
  }),
  props: {
    showModal: Boolean,
    idCampeonatoSelect: {},
  },
  methods: {
    async verificarCampeoes() {
      const objChave = { id_campeonato: this.idCampeonatoSelect };

      const { data } = await axios.post(
        "http://127.0.0.1:8000/api/chave/list/listarCampeoes",
        objChave
      );

      data.forEach((item) => {
        if (item.campeao) {
          item.colocacao = "OURO";
        } else if (item.vice) {
          item.colocacao = "PRATA";
        } else {
          item.colocacao = "BRONZE";
        }
      });
      this.campeoes = data;
    },
    async getTimes() {
      this.campeoes = [];
      const { data } = await axios.get("http://127.0.0.1:8000/api/times");
      if (Array.isArray(data)) {
        this.listaDeTimes = data;
        this.verificarCampeoes();
      }
    },
    montarObjetoChave() {
      this.listaDeTimes.forEach((time) => {
        const objChave = {
          id_campeonato: this.idCampeonatoSelect,
          id_time: time.id,
        };
        this.chave.push(objChave);
      });
    },
    async salvarChave() {
      this.montarObjetoChave();
      const recebeChave = this.chave;
      const { data } = await axios.post(
        "http://127.0.0.1:8000/api/chave/salvar",
        recebeChave
      );

      data.forEach((item) => {
        if (item.campeao) {
          item.colocacao = "OURO";
        } else if (item.vice) {
          item.colocacao = "PRATA";
        } else {
          item.colocacao = "BRONZE";
        }
      });

      this.campeoes = data;
    },
    limparCampeoes() {
      this.campeoes = [];
    },
  },
  mounted() {
    this.getTimes();
  },
};
</script>

<style>
.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: table;
  transition: opacity 0.3s ease;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}

.modal-container {
  width: 300px;
  margin: 0px auto;
  padding: 20px 30px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
}

.modal-header h3 {
  margin-top: 0;
  color: #42b983;
}

.modal-body {
  margin: 20px 0;
}

.modal-default-button {
  float: right;
}

/*
 * The following styles are auto-applied to elements with
 * transition="modal" when their visibility is toggled
 * by Vue.js.
 *
 * You can easily play with the modal transition by editing
 * these styles.
 */

.modal-enter-from {
  opacity: 0;
}

.modal-leave-to {
  opacity: 0;
}

.modal-enter-from .modal-container,
.modal-leave-to .modal-container {
  -webkit-transform: scale(1.1);
  transform: scale(1.1);
}
</style>
