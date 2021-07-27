<template>
  <div class="todo_list">
    <Form :data="data" @updateList="updateDataNew"/>
    <div v-if="data" class="todo_list__items">
      <Item @updateInput="updateDataNew" @deleteItemId="updateDataNew" @changeChecked="updateDataNew" v-for="(item, index) in data" :data="item" v-bind:key="index"/>
    </div>
    <div v-else class="todo_list__items">
      Loading ...
    </div>
  </div>
</template>

<script>
import Item from './Item'
import Form from './Form'

export default {
  name: 'TodoList',
  components: {
    Item,
    Form
  },
  data: () => ({
    data: null,
    url: process.env.VUE_APP_URL
  }),
  mounted () {
    const axios = require('axios')
    console.log(process.env.VUE_APP_URL)
    axios
      .get(this.url)
      .then(response => {
        this.data = response.data
        console.log(this.data)
      })
  },
  methods: {
    /* Функция срабатывает на событие 'deleteItemId', которое срабатывает
    * из-за this.$emit у дочернего компонента для обновления свойства data
    * текущего компонента */
    updateDataNew () {
      const axios = require('axios')
      axios
        .get(this.url)
        .then(response => {
          this.data = response.data
          console.log(this.data)
        })
    }
    // deleteItem (id) {
    //   for (let i = 0; i < this.data.length; i++) {
    //     if (this.data[i].id === id) {
    //       console.log('delete')
    //       this.data.splice(i, 1)
    //     }
    //   }
    // },
    // updateData (value) {
    //   this.data.push(value)
    //   console.log(this.data)
    // },
    // onChangeChecked (value) {
    //   console.log(value)
    //   for (let i = 0; i < this.data.length; i++) {
    //     if (this.data[i].id === value.id) {
    //       this.data[i] = value
    //       break
    //     }
    //   }
    // },
    // onUpdateInput (value) {
    //   for (let i = 0; i < this.data.length; i++) {
    //     if (this.data[i].id === value.id) {
    //       this.data[i] = value
    //       break
    //     }
    //   }
    // }
  }
}
</script>

<style scoped>
.todo_list {
  max-width: 600px;
  width:100%;
  border-radius:5px;
  box-shadow: 0 0 15px rgba(210, 210, 210, 0.9);

  padding: 5px;
  margin: 5px auto;

  background-color: #fff;
}
</style>
