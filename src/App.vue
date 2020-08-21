<template>
  <div id="app" class="container-fluid">
    <div class="row align-items-start row-cols-lg-2 row-cols-md-2 row-cols-sm-1">
      <div class="col col-lg-6">
        <label for="date">Datum:</label><br>
        <input v-model="date" id="date" type="date"><br><br>
        <label for="taetigkeiten">Tätigkeiten:</label>
        <FormTextList id="taetigkeiten" v-bind:parent-rerender="forceRerender" v-bind:items="items"></FormTextList>
        <label for="zeitaufw">Zeitaufwand</label><br>
        <input id="zeitaufw" type="text" v-model="zeitaufwand">
        <br>
        <label for="gelernt">Gelerntes</label><br>
        <textarea class="form-control" id="gelernt" v-model="gelernt"></textarea><br>
        <span>
          <input class="form-check-input" v-model="drawOpenQuestions" type="checkbox">
          <label>Offene Fragen:</label>
        </span>
        <textarea class="form-control" id="fragen" v-model="questions" v-bind:hidden="!drawOpenQuestions"></textarea><br>
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
      </div>
      <div class="col col-lg-6">
      <div v-if="renderComponent" id="output">
        <div id="output-formatted">
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
          <span><b>Zeitaufwand: </b>{{zeitaufwand}}</span><br>
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
    </div>
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
      zeitaufwand: '',
      drawOpenQuestions: false,
      dividerPosition: 'unten',
      dividerString: '#',
      dividerwidth: 30,
      htmlRaw: '',
    }
  },
  methods: {
    forceRerender() {
      // Remove my-component from the DOM
      this.renderComponent = false;

      this.$nextTick(() => {
        // Add the component back in
        this.renderComponent = true;
        this.setOuptutHTML();
      });
    },
    divider(divider, width) {
      if (divider === '') return '';
      console.log(this.dividerPosition);
      let out = '';
      let actLength = width >= 300 ? 300 : width
      while (out.length < actLength) {
        out += divider;
      }
      return out;
    },
    setOuptutHTML() {
      let a = document.getElementById('output-formatted');
      if(a !== null) {
        this.htmlRaw = a.innerHTML;
      }
      this.htmlRaw = '';
    }
  }
}

</script>

<style>
@import "~bootstrap/dist/css/bootstrap.min.css";

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;

  margin-left: 10px;
  margin-top: 20px;
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

pre {
  overflow-x: auto;
  white-space: pre-wrap;
  white-space: -moz-pre-wrap;
  white-space: pre-wrap;
  white-space: -o-pre-wrap;
  word-wrap: break-word;
}


</style>
