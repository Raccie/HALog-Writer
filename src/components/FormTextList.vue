<template>
  <div v-if="renderComponent">
    <button class="btn btn-dark" v-on:click="items.push({id: items.length, name: ''})">Tätigkeit hinzufügen</button>
    <div v-for="item in items" :key="item.id">
      <span class="form-group form-inline" v-if="!item.removed">
        <input class="taet-text form-control " type="text" v-bind:id="items.indexOf(item)" v-model="item.name">
        <button class="btn btn-dark addbtn taet-btn form-control" v-on:click="remove(item)">Entfernen</button>
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
.addbtn {
  margin-left: 10px;
}
.taet-btn {
  max-width: 100px;
}
.taet-text {
  max-width: 300px;
}
.form-inline {
  margin-top: 10px;
}
</style>
