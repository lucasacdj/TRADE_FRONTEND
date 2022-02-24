<template>
  <Transition name="modal">
    <div v-if="show" class="modal-mask">
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
                <tr
                  v-for="time in listaDeTimes"
                  :key="time.cd_time"
                >
                  <th scope="row">{{ time.cd_time }}</th>
                  <td>{{ time.ds_time }}</td>
                </tr>
              </tbody>
            </table>
          </div>

          <div class="modal-footer">
            <slot name="footer">
              default footer
              <button class="modal-default-button" @click="$emit('close')">
                OK
              </button>
            </slot>
          </div>
        </div>
      </div>
    </div>
  </Transition>
</template>

<script>
export default {
  name: "ModalVue",
  data: () => ({
    listaDeTimes: [
      {
        cd_time: 1,
        ds_time: "Time 1",
      },
      {
        cd_time: 2,
        ds_time: "Time 2",
      },
      {
        cd_time: 3,
        ds_time: "Time 3",
      },
      {
        cd_time: 4,
        ds_time: "Time 4",
      },
      {
        cd_time: 5,
        ds_time: "Time 5",
      },
      {
        cd_time: 6,
        ds_time: "Time 6",
      },
      {
        cd_time: 7,
        ds_time: "Time 7",
      },
      {
        cd_time: 8,
        ds_time: "Time 8",
      },
    ],
  }),
  props: {
    show: Boolean,
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
