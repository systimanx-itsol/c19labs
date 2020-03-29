<template>
  <b-loading :active.sync="isLoading">
    <b-icon pack="fas" icon="sync-alt" size="is-large" custom-class="fa-spin"></b-icon>
  </b-loading>
</template>

<script>
export default {
  props: {
    isLoading: {
      type: Boolean,
      required: false
    }
  }
};
</script>

<style>
</style>