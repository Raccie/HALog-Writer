<template>
  <div v-if="renderComponent">
    <button v-on:click="items.push({id: items.length, name: ''})">Tätigkeit hinzufügen</button>
    <div v-for="item in items" :key="item.id">
      <span v-if="!item.removed">
        <input type="text" v-bind:id="items.indexOf(item)" v-model="item.name">
        <button v-on:click="remove(item)">Entfernen</button>
        <!--button v-on:click="items = items.filter((value)=>{return value !== item}">Entfernen</button-->
      </span>
    </div>
  </div>
</template>

<script>
export default {
name: "FormTextList",
  props: {
    items: Array,
    parentRerender: Function,
  },
  data() {
    return {
      renderComponent: true,
    }
  },
  methods: {
    remove(_value) {
      _value.removed = true;
      this.forceRerender();
    },
    forceRerender() {
      // Remove my-component from the DOM
      this.renderComponent = false;

      this.$nextTick(() => {
        // Add the component back in
        this.renderComponent = true;
      });

      this.parentRerender();
    }
  }
}
</script>

<style scoped>

</style>
