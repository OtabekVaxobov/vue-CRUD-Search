<template>
  <div id="emloyee-form">
      <form @submit.prevent="handleSubmit">
          <label>Employee name</label>
          <input ref="first"
    type="text"
    :class="{ 'has-error': submitting && invalidName }"
    v-model="employee.name"
    @focus="clearStatus"
    @keypress="clearStatus"
  />
           <label>Employee Email</label>
          <input
    type="text"
    :class="{ 'has-error': submitting && invalidEmail }"
    v-model="employee.email"
    @focus="clearStatus"
  />
           <label>Employee phone</label>
          <input
    type="number"
    :class="{ 'has-error': submitting && invalidNumber }"
    v-model="employee.phone"
    @focus="clearStatus"
  />
   <p v-if="error && submitting" class="error-message">
    ❗Please fill out all required fields
  </p>
  <p v-if="success" class="success-message">
    ✅ Employee successfully added
  </p>
          <button>Add Employee</button>
      </form>
  </div>
</template>

<script>
export default {
    name: 'employee-form',
    data() {
        return{
        submitting: false,
        error: false,
    success: false,
            employee:{
                name: '',
                email: '',
                phone: '',
            },
        }
    },
    methods: {
        handleSubmit() {
            this.submitting = true
    this.clearStatus()

    if (this.invalidName || this.invalidEmail || this.invalidPhone) {
      this.error = true
      return
    }

    this.$emit('add:employee', this.employee)
    this.$refs.first.focus()                 //focus to 'first'(name)
    this.employee = {
      name: '',
      email: '',
      phone: '',
    }
    this.error = false
    this.success = true
    this.submitting = false
  },

  clearStatus() {
    this.success = false
    this.error = false
  }
        },
    
    computed: {
  invalidName() {
    return this.employee.name === ''
  },

  invalidEmail() {
    return this.employee.email === ''
  },
   invalidNumber() {
    return this.employee.phone === ''
  },
},
}
</script>

<style scoped>
  form {
    margin-bottom: 2rem;
  }
   [class*='-message'] {
    font-weight: 500;
  }

  .error-message {
    color: #d33c40;
  }

  .success-message {
    color: #32a95d;
  }
</style>
