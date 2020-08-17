<template>
  <div id="app">
    <label for="date">Datum:</label><br>
    <input v-model="date" id="date" type="date"><br><br>
    <label for="taetigkeiten">Tätigkeiten:</label>
    <FormTextList id="taetigkeiten" v-bind:parent-rerender="forceRerender" v-bind:items="items"></FormTextList>
    <br>
    <label for="gelernt">Gelerntes</label><br>
    <textarea id="gelernt" v-model="gelernt"></textarea><br><br>
    <label>Offene Fragen:</label>
    <input v-model="drawOpenQuestions" type="checkbox"><br>
    <textarea id="fragen" v-model="questions" v-bind:hidden="!drawOpenQuestions"></textarea><br><br>
    <label>Trennlinie:</label>
    <div>
      <input type="radio" id="over" name="divider" v-model="dividerPosition" value="oben">
      <label for="over">Oben</label>
    </div>
    <div>
      <input type="radio" id="under" name="divider" v-model="dividerPosition" value="unten">
      <label for="under">Unten</label>
    </div>
    <div>
      <input type="radio" id="none" name="divider" v-model="dividerPosition" value="kein">
      <label for="none">Kein</label>
    </div>
    <input type="text" id="dividersymbol" v-model="dividerString">
    <input type="number" id="dividerwidth" v-model="dividerwidth">


    <div v-if="renderComponent" id="output">
        <span v-if="dividerPosition==='oben'">
            {{ divider(dividerString, dividerwidth) }}
        </span><br>
      <b>Datum: {{ new Date(date).toLocaleDateString() }}</b><br>
      <b>Tätigkeiten:</b>
      <ul v-for="item of items" :key="item.id">
        <span v-if="!item.removed">
          <li>{{ item.name }}</li>
        </span>
      </ul>
      <b>Was habe ich gelernt?</b><br>
      <span>{{ gelernt }}</span><br><br>
      <div v-bind:hidden="!drawOpenQuestions">
        <span><b>Offene Fragen: </b></span><br>
        <span>{{ questions }}</span><br><br>
      </div>
      <span v-if="dividerPosition==='unten'">
        {{ divider(dividerString, dividerwidth) }}
      </span>
    </div>
  </div>
</template>

<script>
//import HelloWorld from './components/HelloWorld.vue'

import FormTextList from "@/components/FormTextList";

export default {
  name: 'App',
  components: {
    FormTextList
    //HelloWorld
  },
  data: function () {
    return {
      renderComponent: true,
      date: `${new Date().toLocaleDateString()}`,
      items: [
        {id: 0, name: "", removed: false}
      ],
      gelernt: '',
      questions: '',
      drawOpenQuestions: false,
      dividerPosition: 'unten',
      dividerString: '#',
      dividerwidth: 30,
    }
  },
  methods: {
    forceRerender() {
      // Remove my-component from the DOM
      this.renderComponent = false;

      this.$nextTick(() => {
        // Add the component back in
        this.renderComponent = true;
      });
    },
    divider(divider, width) {
      if (divider === '') return '';
      console.log(this.dividerPosition);
      let out = '';
      while (out.length < width) {
        out += divider;
      }
      return out;
    }
  }
}

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

#app > label {
  margin-right: 20px;
  font-weight: bold;
}


#output {
  margin-top: 100px;
}

#dividersymbol {
  width: 30px;
}

#dividerwidth {
  margin-left: 10px;
  width: 40px;
}
</style>
