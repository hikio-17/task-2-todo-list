<template>
  <form @submit.prevent="onSubmit">
    <h4 class="text-center">
      Add Your Task
    </h4>
    <div class="form-group">
      <label for="exampleFormControlInput1">Title</label>
      <input id="exampleFormControlInput1" v-model="title" type="text" class="form-control" placeholder="Add Your Title">
    </div>
    <div class="form-group">
      <label for="exampleFormControlTextarea1">Description</label>
      <textarea id="exampleFormControlTextarea1" v-model="description" class="form-control" rows="3" placeholder="Add Your Description" />
    </div>
    <div class="form-group">
      <label for="exampleFormControlSelect1">Select Category</label>
      <select id="exampleFormControlSelect1" v-model="category" class="form-control">
        <option v-for="(item, i) in categories" :key="i" :value="item">
          {{ item }}
        </option>
      </select>
    </div>
    <br>
    <button class="btn btn-primary" type="submit">
      Save
    </button>
    <button class="btn btn-danger" @click="onCancel">
      Cancel
    </button>
  </form>
</template>

<script>
export default {
  // eslint-disable-next-line vue/require-prop-types
  props: ['categories'],
  data () {
    return {
      title: '',
      description: '',
      category: ''
    }
  },
  methods: {
    onCancel () {
      this.$emit('on-cancel-form')
    },
    onSubmit () {
      if (this.title === '' || this.description === '' || this.category === '') {
        alert('Field inputan tidak boleh kosong!')
        return
      }
      this.$emit('on-submit-form', {
        title: this.title,
        description: this.description,
        category: this.category
      })
    }
  }
}
</script>

<style scoped>
   form {
      margin-top: 20px;
      border: 2px solid lightgray;
      border-radius: 20px;
      padding: 20px;
      width: 50%;
      margin: 30px auto;
   }

   .form-group {
      margin-top: 10px;
   }
</style>
