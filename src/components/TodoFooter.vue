<template>
  <div>
    <button @click="clearAll">모두지우기</button>
  </div>
</template>

<script>
export default {
  methods: {
    clearAll() {
      this.$emit('clearAll')
    },
  },
}
</script>

<style>

</style>