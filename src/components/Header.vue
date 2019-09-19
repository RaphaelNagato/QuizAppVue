<template>
  <div>
    <b-nav tabs>
      <b-nav-item disabled>
        <b>Quiz App</b>
      </b-nav-item>
      <b-nav-item disabled>Count: {{numCorrect}}/{{numTotal}}</b-nav-item>
      <b-nav-item>
        <b-button variant="outline-primary" @click="refresh" size="sm">Refresh</b-button>
      </b-nav-item>
    </b-nav>
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
  }
};
</script>