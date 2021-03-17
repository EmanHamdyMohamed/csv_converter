<template>
  <div>
    <b-row class="form-group">
      <b-col>
          <b-form-file v-model="file" :state="Boolean(file)" accept=".csv" @change="loadTextFromFile"
          placeholder="Choose a file or drop it here..." drop-placeholder="Drop file here..."></b-form-file>
      </b-col>
    </b-row>
    <b-row style="text-align: left;margin-top: 10px;" class="form-group"
      v-if="keys && keys.length">
      <b-col>
          <p>Columns: </p>
          <b-form-checkbox-group
          v-model="selected"
          :options="keys"
          class="mb-3"
          stacked
          disabled-field="notEnabled"
        ></b-form-checkbox-group>
      </b-col>
    </b-row>
    <b-row class="form-group" v-if="selected && selected.length">
      <b-col>
          <b-button variant="success" @click="convert">Convert</b-button>
      </b-col>
    </b-row>
  </div>
</template>

<script>
import CSVToJSON from 'csvtojson';
import _ from 'lodash';

export default {
  name: 'converter',
  props: {
    msg: String
  },
  data() {
    return {
      file: null,
      text: '',
      keys: [],
      selected: [],
      csvRow: null,
    }
  },
  methods: {
    clear() {
      this.csvRow = null;
      this.selected = [];
      this.keys = [];
      this.text = '';
      this.file = null
    },
    loadTextFromFile(ev) {
      this.clear();
      const file = ev.target.files[0];
      console.log('selected a file');
      if(!file || file.type !== 'text/csv'){
        return;
      }
      let reader = new FileReader();
      reader.readAsText(file, "UTF-8");
      reader.onload =  evt => {
        this.text = evt.target.result;
        this.readCsvFile();
      }
      reader.onerror = evt => {
        console.error(evt);
      }
    },
    readCsvFile() {
      CSVToJSON()
      .fromString(this.text)
      .then((csvRow)=> { 
          this.csvRow = csvRow;
          _.each(csvRow, (row) => {
            this.keys.push(...Object.keys(row));
          });
          this.keys = _.uniq(_.compact(this.keys));
      });
    },
    convert() {
      const jsonData = _.map(this.csvRow, (row) => {
          const obj = {};
          _.each(this.selected, (item) => {
            obj[item] = row[item];
          })
          return obj;
      });
      const name = _.first(this.file.name.split('.'));
      const data = JSON.stringify(jsonData)
      const blob = new Blob([data], {type: 'text/json'})
      const e = document.createEvent('MouseEvents'),
      a = document.createElement('a');
      a.download = `${name}.json`;
      a.href = window.URL.createObjectURL(blob);
      a.dataset.downloadurl = ['text/json', a.download, a.href].join(':');
      e.initEvent('click', true, false, window, 0, 0, 0, 0, 0, false, false, false, false, 0, null);
      a.dispatchEvent(e);
    }
  }
}
</script>

<style scoped>

</style>
