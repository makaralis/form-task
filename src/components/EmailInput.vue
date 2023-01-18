<template>
    <div>
      <label>{{ label }}</label>
      <input v-model="value" @input="$emit('input', value)" :required="required"/>
      <p v-if="error" class="error">{{ error }}</p>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      label: String,
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
        const emailRegex = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
        if (this.required && !this.value) {
          return true
        }
        if(!emailRegex.test(this.value)){
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