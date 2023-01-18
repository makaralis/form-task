<template>
  <div>
    <form @submit.prevent="submitForm">
      <text-input
        label="First Name"
        v-model="firstName"
        @validate="validateFirstName"
        @input="onFirstNameInput($event)"
        :minlength="2"
        :maxlength="12"
        :pattern="'^[a-zA-Z]+$'"
        :required="true"
        :error="firstNameError"
      />

      <text-input
        label="Last Name"
        v-model="lastName"
        @validate="validateLastName"
        @input="onLastNameInput($event)"
        :minlength="2"
        :maxlength="15"
        :pattern="'^[a-zA-Z]+$'"
        :required="true"
        :error="lastNameError"
      />

      <email-input
        label="Email"
        v-model="email"
        @validate="validateEmail"
        @input="onEmailInput($event)"
        :required="true"
        :error="emailError"
      />

      <password-input
        label="Password"
        v-model="password"
        @validate="validatePassword"
        @input="onPasswordInput($event)"
        :required="true"
        :error="passwordError"
        type="password"
      />

      <span>Phone Number</span><br />
      <div class="phoneContainer">
        <div>
          <select v-model="phonePrefix" class="phonePrefixContainer">
            <option value="050">050</option>
            <option value="052">052</option>
            <option value="054">054</option>
          </select>
        </div>

        <div>
          <input v-model="phoneNumber" @input="validatePhoneNumber" type="text" maxlength="7" /><br />
          <span v-if="phoneError" class="error">{{ phoneError }}</span><br />
        </div>
      </div>

      <input class="submit" type="submit" value="Submit" />
      <span v-if="submitFormError" class="error">{{ submitFormError }}</span><br /> 
    </form>
  </div>
</template>

<script>
import textInput from "./TextInput.vue"
import emailInput from "./EmailInput.vue"
import passwordInput from "./PasswordInput.vue"

export default {
   // eslint-disable-next-line vue/multi-word-component-names
   name: 'Form',
   components: { textInput, emailInput, passwordInput },
   data() {
    return {
      firstName: '',
      firstNameError: '',
      lastName: '',
      lastNameError: '',
      email: '',
      emailError: '',
      phonePrefix: '',
      phoneNumber: '',
      phoneError: '',
      password: '',
      passwordError: '',
      submitFormError: ''
    }
  },
  methods: {
    onFirstNameInput(event) {
      this.firstName = event.target?.value
    },
    onLastNameInput(event) {
      this.lastName = event.target?.value
    },
    onEmailInput(event) {
      this.email = event.target?.value
    },
    onPasswordInput(event) {
      this.password = event.target?.value
    },
    validateFirstName(valid) {
      if(!valid) {
        this.firstNameError = "First name should not contain any charachters except letters.";
      } else {
        this.firstNameError = "";
      }
    },
    validateLastName(valid) {
      if(!valid) {
        this.lastNameError = "Last name should not contain any charachters except letters.";
      } else {
        this.lastNameError = "";
      }
    },
    validatePhoneNumber() {
        if(!/^\d{7}$/.test(this.phoneNumber)) {
            this.phoneError = "Phone number should contain exactly 7 digits.";
        } else {
            this.phoneError = "";
        }
    },
    validateEmail(valid) {
      if(!valid) {
        this.emailError = "Please enter a valid email.";
      } else {
        this.emailError = "";
      }
    },
    validatePassword(valid) {
        if(!valid) {
            this.passwordError = "Password should contain at least one lowercase letter, one uppercase letter, one number and one special character.";
        } else {
            this.passwordError = "";
        }
    },
    submitForm: function () {
      console.log({
        firstName: this.firstName,
        lastName: this.lastName,
        email: this.email,
        phone: this.phonePrefix + this.phoneNumber,
        password: this.password,
      });

      if ((this.phonePrefix + this.phoneNumber) === ''
        ) {
          this.submitFormError = "You need to fill all of the fields to submit the form";
        return;
      }

      if (this.phoneError.length > 0 ||
          this.firstNameError.length > 0 === '' ||
          this.lastNameError.length > 0 === '' ||
          this.passwordError.length > 0 === '' ||
          this.emailError.length > 0 === ''
        ) {
          this.submitFormError = "There are some errors in the form";
        return;
      }
    },
  },
};
</script>

<style>
form {
  display: flex;
  flex-direction: column;
  justify-content: center;
  max-width: 60vh;
  padding: 10px;
  border: 2px solid black;
  border-radius: 5px;
}

input {
  padding: 4px 8px;
  margin: 4px;
}

span {
  font-size: 18px;
  margin: 4px;
  font-weight: 500;
}

.submit {
  font-size: 15px;
  color: #fff;
  background: #222;
  padding: 6px 12px;
  border: none;
  margin-top: 8px;
  cursor: pointer;
  border-radius: 5px;
}

.phoneContainer {
  display: flex;
  flex-direction: row;
  align-items: baseline;
}

.phonePrefixContainer {
  min-height: 28px;
}


.error {
    color: red;
}


</style>
