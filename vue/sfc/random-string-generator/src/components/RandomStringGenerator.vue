<template>
  <div>
    <div class="h5 my-4 text-center">Random String Generator</div>
    <form>
      <div class="form-group row">
        <label class="col-sm-2 col-form-label" for="stringLength">Length</label>
        <div class="col-sm-10">
          <input class="form-control" :class="{'is-invalid': stringLengthError}" id="stringLength" type="text" size="5" maxlength="5" v-model.trim="stringLength" @change="generate">
          <div class="invalid-feedback">{{stringLengthError}}</div>
        </div>
      </div>
      <fieldset class="form-group">
        <div class="row">
          <legend class="col-form-label col-sm-2">Use</legend>
          <div class="col-sm-10">
            <div class="form-check">
              <input class="form-check-input" :class="{'is-invalid': repertoireError}" id="useDigits" type="checkbox" v-model="useDigits">
              <label class="form-check-label" for="useDigits">Digits</label>
            </div>
            <div class="form-check">
              <input class="form-check-input" :class="{'is-invalid': repertoireError}" id="useCapital" type="checkbox" v-model="useCapital">
              <label class="form-check-label" for="useCapital">Capital Latin</label>
            </div>
            <div class="form-check">
              <input class="form-check-input" :class="{'is-invalid': repertoireError}" id="useSmall" type="checkbox" v-model="useSmall">
              <label class="form-check-label" for="useSmall">Small Latin</label>
            </div>
            <div class="form-check">
              <input class="form-check-input" :class="{'is-invalid': repertoireError}" id="usePunctuations" type="checkbox" v-model="usePunctuations">
              <label class="form-check-label" for="usePunctuations">Punctuations</label>
            </div>
            <div class="form-check">
              <input class="form-check-input" :class="{'is-invalid': repertoireError}" id="useCustom" type="checkbox" v-model="useCustom">
              <label class="form-check-label" for="useCustom">Custom</label>
              <input class="form-control" :class="{'is-invalid': repertoireError}" type="text" v-model="customChars">
              <div class="invalid-feedback">{{repertoireError}}</div>
            </div>
          </div>
        </div>
      </fieldset>
      <button class="btn btn-primary mb-3" type="button" @click="generate">Generate</button>
    </form>
    <div class="h1 text-monospace text-break">{{randomString}}</div>
  </div>
</template>

<script>
export default {
  name: 'RandomStringGenerator',
  data() {
    return {
      stringLength: 16,
      stringLengthError: null,
      useDigits: true,
      useCapital: true,
      useSmall: true,
      usePunctuations: false,
      useCustom: false,
      customChars: 'ABCDEF',
      repertoireError: null,
      randomString: null
    }
  },
  computed: {
    repertoire() {
      let result = ''
      if (this.useDigits) {
        result += '0123456789'
      }
      if (this.useCapital) {
        result += 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
      }
      if (this.useSmall) {
        result += 'abcdefghijklmnopqrstuvwxyz'
      }
      if (this.usePunctuations) {
        result += '!"#$%&\'()*+,-./:;<=>?@[\\]^_`{|}~'
      }
      if (this.useCustom) {
        result += this.customChars
      }
      return result
    }
  },
  created() {
    this.generate()
  },
  watch: {
    repertoire() {
      this.generate()
    }
  },
  methods: {
    validate() {
      this.stringLengthError = null
      let len = 0
      if (/^\d+$/.test(this.stringLength)) {
        len = Number(this.stringLength)
      }
      if (len <= 0) {
        this.stringLengthError = 'Length must be a positive integer.'
      }

      this.repertoireError = null
      if (this.repertoire.length === 0) {
        this.repertoireError = 'Select at least one character type.'
      }

      return !this.stringLengthError && !this.repertoireError
    },
    generate() {
      if (!this.validate()) {
        return
      }
      const buf = []
      const len = Number(this.stringLength)
      while (buf.length < len) {
        const random = Math.floor(Math.random() * this.repertoire.length)
        const pickedChar = this.repertoire[random]
        buf.push(pickedChar)
      }
      this.randomString = buf.join('')
    }
  }
}
</script>
