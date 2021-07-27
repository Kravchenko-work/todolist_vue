<template>
  <form action="" class="todo_list__form" @submit.prevent="addItem">
    <input type="text" placeholder="task" class="todo_list__input" @input="(e) => this.textInput=e.target.value" :value="this.textInput">
    <button class="todo_list__button">ADD</button>
  </form>
</template>

<script>

export default {
  name: 'Form',
  props: ['data'],
  data: () => ({
    url: process.env.VUE_APP_URL,
    textInput: ''
  }),
  methods: {
    addItem (event) {
      const axios = require('axios')
      axios
        .post(this.url, {
          flag: false,
          text: this.textInput
        })
        .then(response => {
          this.textInput = ''
          console.log('Успешно данные добавлены')
          console.log(response.data)
          this.$emit('updateList', response.data)
          // this.data.push(response.data)
        })
        .catch(responseError => {
          console.log('Данные НЕ были добавлены')
        })
    }
  }
}

</script>

<style>
.todo_list__form{
  display: flex;
  margin-bottom: 15px;
}
.todo_list__input {
  flex: 1 1 400px;
  margin-right: 15px;
  border-radius: 5px;
  border: 1px solid #c8c8c8;
  color: #c8c8c8;
  padding: 10px 15px;
  transition: box-shadow .2s, border .2s;
}
.todo_list__input:hover {
  box-shadow: 0 0 10px rgba(210, 210, 210, 0.9);
  cursor: pointer;
}
.todo_list__input:focus {
  border-radius: 5px;
  border: 1px solid #46a14a;
  color: #000;
}
.todo_list__button{
  flex: 0 0 130px;
  border-radius: 5px;
  color: #46a14a;
  border: 1px solid #46a14a;
  background-color: #fff;
}
.todo_list__button:hover{
  box-shadow: 0 0 15px rgba(210, 210, 210, 0.9);
  cursor: pointer;
  background-color: #d9fada;
}
.todo_list__button:active{
  background-color: #81f686;
}
@media(max-width: 440px){
  .todo_list__form{
    flex-direction: column;
    height: 100px;
  }
  .todo_list__input{
    margin: 0 0 10px 0;
    flex: 1 0 45%
  }
  .todo_list__button{
    flex: 1 0 45%;
    width: 50%;
    margin: 0 auto;
  }
}
</style>
