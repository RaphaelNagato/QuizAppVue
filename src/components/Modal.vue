<template>
  <div>
    <b-modal id="modal-1" title="Quiz App" ok-only @ok="refresh" ok-title="Refresh">
      <p class="my-4">Count: {{numCorrect}}/{{numTotal}}</p>
    </b-modal>
  </div>
</template>

<script>
export default {
  props: {
    numCorrect: Number,
    numTotal: Number
  },
  methods: {
    refresh() {
      this.$emit("reload");
    }
    /*showBox() {
     *this.$bvModal.msgBoxOk("Action completed");
     *}
     */
  },
  mounted() {
    this.$bvModal.show("modal-1");
  }
};
</script>