<template>
    <div>
      <label>{{ label }}</label>
      <input v-model="value" @input="$emit('input', value)" :minlength="minlength" :maxlength="maxlength" :pattern="pattern" :required="required"/>
      <p v-if="error" class="error">{{ error }}</p>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      label: String,
      minlength: Number,
      maxlength: Number,
      pattern: String,
      required: Boolean,
      error: String
    },
    data() {
      return {
        value: ''
      }
    },
    computed: {
      isValid() {
        if (this.required && !this.value) {
          return true
        }
        if (this.minlength && this.value.length < this.minlength) {
          return false
        }
        if (this.maxlength && this.value.length > this.maxlength) {
          return false
        }
        if (this.pattern && !this.value.match(this.pattern)) {
          return false
        }
        return true
      }
    },
    methods: {
      validate() {
        this.$emit('validate', this.isValid)
      }
    },
    mounted() {
      this.validate()
    },
    watch: {
      value() {
        this.validate()
      }
    }
  }
  </script>
  

<style>
.error {
    color: red;
}
</style>
  