<template>
  <div id="app" class="container-fluid">
    <div class="row row-cols-lg-2 row-cols-md-2 row-cols-sm-1">
      <div class="col col-lg-5">
        <label for="date">Datum:</label><br>
        <input v-model="date" id="date" class="form-control" type="date"><br><br>
        <label for="taetigkeiten">Tätigkeiten:</label>
        <FormTextList id="taetigkeiten" v-bind:parent-rerender="forceRerender" v-bind:items="items"></FormTextList>
        <label for="zeitaufw">Zeitaufwand</label><br>
        <input id="zeitaufw" class="form-control" type="text" v-model="zeitaufwand">
        <br>
        <label for="gelernt">Gelerntes</label><br>
        <textarea class="form-control" id="gelernt" v-model="gelernt"></textarea><br>
        <span style="margin-left: 20px">
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
        <label for="html-divider-chkbx">HTML-Style </label>
        <input style="margin-left: 10px" type="checkbox" id="html-divider-chkbx" v-model="dividerHtmlStyle"><br>
        <span style="margin-left: 20px">
          <input type="text" id="dividersymbol" v-model="dividerString" v-bind:disabled="dividerHtmlStyle">
          <input type="number" id="dividerwidth" v-model="dividerwidth" v-bind:disabled="dividerHtmlStyle">
        </span>
      </div>
      <div class="col col-lg-5">
        <div v-if="renderComponent" id="output">
          <!--div id="output-formatted">
            <span v-if="dividerPosition==='oben'">
                {{ divider(dividerString, dividerwidth) }}
            </span><br>
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
          </div-->
          <div id="output-formatted">
            <span v-if="dividerPosition==='oben'" v-html="divider(dividerString, dividerwidth)">
            </span>
            <b>Datum: {{ new Date(date).toLocaleDateString() }}</b><br>
            <span>
              <b>Tätigkeiten:</b>
            </span>
            <span>
              <ul id="taetlist" v-for="item of items" :key="item.id">
                <li v-if="!item.removed">{{ item.name }}</li>
              </ul>
            </span>
            <p>
              <b>Zeitaufwand: </b>{{ zeitaufwand }}<br>
              <span><b>Was habe ich gelernt?<br></b>{{ gelernt }}<br></span>

              <span v-if="drawOpenQuestions"><b>Offene Fragen<br></b>{{ questions }}</span>
            </p>
            <span v-if="dividerPosition==='unten'" v-html="divider(dividerString, dividerwidth)">
            </span>
          </div>
          <button class="btn btn-dark" v-on:click="copyHTML">Copy!</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
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
      dividerHtmlStyle: false,
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
      if(!this.dividerHtmlStyle) {
        if (divider === '') return '';
        let out = '<span>';
        let actLength = width >= 300 ? 300 : width
        while (out.length < actLength) {
          out += divider;
        }
        out += '</span><br>';
        return out;
      } else {
        return '<hr>'
      }
    },
    copyHTML() {
      let copyText = document.getElementById('output-formatted');
      navigator.clipboard.writeText(copyText.innerHTML);
      alert("Copied!");

    },
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

.form-control {
  max-width: 330px;
}


</style>
