<template>
    <div class="wrapper">
    <hr class="hr-modify">
      <h2>Profile Form</h2>
      <JsonForms
        v-model="formData"
        :schema="schema"
        :uischema="uischema"
        :renderers="renderers"
        @change="handleFormChange"
      ></JsonForms>
      <div>
        <button :disabled="!isAbove18" @click="onNext">Submit</button>
      </div>
      <div class="error-message" v-if="birthdateErrorMessage">{{ birthdateErrorMessage }}</div>

    </div>
  </template>
  
  <script>
  import { defineComponent } from 'vue';
  import { JsonForms } from '@jsonforms/vue';
  import { vanillaRenderers } from '@jsonforms/vue-vanilla';
  
  export default defineComponent({
    name: 'ProfileForm',
    components: {
      JsonForms,
    },
    data() {
      return {
        formData: {
          firstName: '',
          lastName: '',
          birthday: '',
          email: '',
        },
        isAbove18: false,
        birthdateErrorMessage: '',
      };
    },
    computed: {
      schema() {
        return {
          type: 'object',
          properties: {
            firstName: { type: 'string' },
            lastName: { type: 'string' },
            birthday: { type: 'string', format: 'date' },
            email: { type: 'string', format: 'email' },
          },
          required: ['firstName', 'lastName', 'birthday', 'email'],
        };
      },
      uischema() {
        return {
          type: 'VerticalLayout',
          elements: [
            { type: 'Control', scope: '#/properties/firstName' },
            { type: 'Control', scope: '#/properties/lastName' },
            { type: 'Control', scope: '#/properties/birthday' },
            { type: 'Control', scope: '#/properties/email' },
          ],
        };
      },
      renderers() {
        return vanillaRenderers;
      },
    },
    methods: {
        handleFormChange(updatedData) {
        this.formData = updatedData;
        this.checkAge();
    },
      checkAge() {
        const dataForm = this.formData;
        if (dataForm?.data?.birthday) {
          const currentDate = new Date();
          const birthDate = new Date(dataForm?.data?.birthday);
          const ageDifference = currentDate.getFullYear() - birthDate.getFullYear();
          this.isAbove18 = ageDifference >= 18;
          this.birthdateErrorMessage = !this.isAbove18 ? 'You must be 18 years or older' : '';
        } else {
          this.isAbove18 = false;
          this.birthdateErrorMessage = '';
        }
      },
      onNext() {
        if (this.formData.errors.length === 0 && this.isAbove18) {
            alert("Submit Successfully");
        }
      },
    },
    created() {
      this.checkAge();
    },
  });
  </script>
  
  <style scoped>

@import url('https://fonts.googleapis.com/css2?family=Oswald:wght@300;400;500;600;700&display=swap');

h2 {
    font-family: 'Oswald', sans-serif;
    margin-top: 0px;
}
*>>> label {
    font-family: 'Oswald', sans-serif;
}

*>>> .vertical-layout-item {
   margin-bottom: 10px;
}

 .jsonforms-control {
  margin-bottom: 10px;
}

.wrapper {
  max-width: 560px;
  margin: 120px auto;
}

button {
  padding: 10px 20px;
  background-color: #007bff;
  color: white;
  border: none;
  cursor: pointer;
  border-radius: 3px;
  font-family: 'Oswald', sans-serif;
  font-size: 14px;
    text-transform: uppercase;
}

button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

* >>> .input {
  width: 100%;
  padding: 10px 12px;
}

* >>> .error {
    color: red;
    font-family: 'Oswald', sans-serif;
    text-transform: lowercase;
    font-size: 13px;
}

.error-message {
    color: red;
  font-family: 'Oswald', sans-serif;
  margin-top: 10px;
}

.hr-modify {
    margin: 0px;
    width: 7%;
    border: 1px solid #007bff;
}
  </style>
  