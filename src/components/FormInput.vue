<template>
  <div
    class="justify-content-center text-center"
    style="display:grid;grid-row-gap: 1em;"
  >
    <b-form>
      <div class="d-flex justify-content-between align-items-center text-center">
        <b-form-group
          id="input-group-1"
          label="Phone Number"
          label-for="phoneNo"
          :description="desc"
        >
          <b-form-input
            id="phoneNo"
            v-model="form.phoneNo"
            input-mode="decimal"
            placeholder="Enter Phone Number"
            required
          ></b-form-input>
        </b-form-group>

        <b-form-group id="input-group-4"
          class="mx-2"
        >
          <b-form-checkbox
            v-model="checked"
          >With Text</b-form-checkbox>
        </b-form-group>
      </div>
      <b-form-group
        v-if="checked"
        id="textarea-label"
        class="mt-2"
        label="Text WA"
        label-for="textarea"
        :description="desc"
      >
        <b-form-textarea
          id="textarea"
          v-model="textWa"
          placeholder="Enter something..."
          rows="3"
        ></b-form-textarea>
      </b-form-group>
    </b-form>
    <div
      class="justify-content-center"
      style="display:grid;grid-row-gap: 1em;"
    >
      <b-link
        v-if="validUrl"
        :href="urlApiWA"
        target="_blank"
      >API Whatapp</b-link>
      <b-link
        v-if="validUrl"
        :href="generalUrlWA"
        target="_blank"
      >General Whatapp</b-link>
      <b-link
        v-if="validUrl"
        :href="urlWebWA"
        target="_blank"
      >Web Whatapp</b-link>
    </div>
  </div>
</template>

<script>
import {
  BForm,
  BLink,
  BFormGroup,
  BFormCheckbox,
  BFormInput,
} from 'bootstrap-vue'

export default {
  name: '',
  data() {
    return {
      form: {
        phoneNo: '',
      },
      checked: null,
      validUrl: false,
      textWa: '',
      desc: 'Enter phone with 8xxxx'
    }
  },
  components: {
    BLink,
    BForm,
    BFormGroup,
    BFormCheckbox,
    BFormInput,
  },
  watch: {
    form: {
      deep: true,
      handler: function() {
        const ph = this.newDataNumber(this.form.phoneNo)
        this.desc = 'Enter phone with 8xxxx'
        if(ph && ph.length > 9){
          this.validUrl = true
          this.form.phoneNo = ph
        }else{
          this.validUrl = false
          this.form.phoneNo = ''
          this.desc = 'Error Phone Number INVALID'
        }
      },
    },
    checked: {
      handler(newVal) {
        if(!newVal){
          this.textWa = ''
        }
      }
    }
  },
  computed: {
    generalUrlWA: function () {
      return `https://wa.me/${this.form.phoneNo}`
    },
    urlWebWA: function () {
      const encodedText = encodeURIComponent(this.textWa).replace(/'/g,"%27").replace(/"/g,"%22");
      return `https://web.whatsapp.com/send/?phone=${this.form.phoneNo}&text=${encodedText}&app_absent=0`
    },
    urlApiWA: function () {
      const encodedText = encodeURIComponent(this.textWa).replace(/'/g,"%27").replace(/"/g,"%22");
      return `https://api.whatsapp.com/send/?phone=${this.form.phoneNo}&text=${encodedText}&app_absent=0`
    },
  },
  methods: {
    newDataNumber(val) {
      let newData = val.replace(/[^0-9]/gi, "").trimLeft().substring(0,17);
      const makeString = newData.toString()
      const dt = makeString.substring(0,1)
      if (dt.length > 0) {
        switch(dt){
          case '+':
            newData = makeString.substring(1,makeString.length)
            break
          case '0':
            newData = '62' + makeString.substring(1,makeString.length)
            break
          case '6':
            newData = makeString
            break  
          default:
            newData = '62' + makeString
            break
        }
        return newData
      }
    },
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
