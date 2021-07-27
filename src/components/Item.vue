<template>
  <div class="item" v-if="data">
    <div class="item__left">
      <input @click="clickCheckbox" type="checkbox" class="item__checkbox" :id="`item__checkbox${data.id}`" :checked="data.flag">
      <label :for="`item__checkbox${data.id}`" class="item__custom-checkbox"></label>
    </div>
    <div class="item__center">
      <input @input="changeInput" type="text" class="item__input" :value="data.text">
    </div>
    <div class="item__right">
      <input @click="clickDelete" type="button" class="item__delete">
    </div>
  </div>
</template>

<script>
export default {
  name: 'Item',
  data: () => ({
    url: process.env.VUE_APP_URL
  }),
  props: ['data'],
  methods: {
    clickCheckbox () {
      console.log(this.data.flag)
      const axios = require('axios')
      axios
        .patch(this.url + '/' + this.data.id, {
          flag: !this.data.flag,
          id: this.data.id,
          text: this.data.text
        })
        .then(response => {
          console.log(response.data)
          console.log('Успешно')
          this.$emit('changeChecked', response.data)
        })
        .catch(responseError => {
          console.log('Не выполнилось')
        })
    },
    clickDelete () {
      const axios = require('axios')
      axios
        .delete(this.url + '/' + this.data.id)
        .then(response => {
          console.log('Успешно')
          this.$emit('deleteItemId', this.data.id)
        })
        .catch(responseError => {
          console.log('Не выполнилось')
        })
    },
    changeInput (event) {
      const axios = require('axios')
      axios
        .patch(this.url + '/' + this.data.id, {
          flag: this.data.flag,
          text: event.target.value,
          id: this.data.id
        })
        .then(response => {
          console.log('Успешно данные добавлены')
          console.log(response.data)
          this.$emit('updateInput', response.data)
          // this.data.push(response.data)
        })
        .catch(responseError => {
          console.log('Данные НЕ были добавлены')
        })
    }
  }
}
</script>

<style scoped>
.item{
  display: flex;
  padding: 5px 0;
  margin-bottom: 10px;
}
.item__left{
  flex: 0 0 50px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-right: 10px;
}
.item__checkbox{
  position: absolute;
  z-index: -1;
  opacity: 0;
}
.item__custom-checkbox {
  width: 18px;
  height: 18px;
  background: #fff;

  border: 1px solid #7b7b7b;
  border-radius: 2px;
}
.item__custom-checkbox:hover{
  cursor: pointer;
}
.item__checkbox:checked+.item__custom-checkbox{
  background: #fff url("./../assets/checkbox.png") center / cover no-repeat scroll;
  border: none;
}

.item__center{
  flex: 1 1 0;
}
.item__input {
  width: 100%;
  padding:  5px;
  margin-right: 10px;
  border-bottom: 1px solid #c8c8c8;
  transition: border .2s;
}
.item__input:hover {
  border-bottom: 1px solid #000;
  cursor: pointer;
}
.item__input:focus{
  border-bottom: 1px solid #46a14a
}
.item__right{
  flex: 0 0 50px;
  display: flex;
  align-items: center;
  justify-content: center;
}
.item__delete{
  width: 13px;
  height: 13px;
  background: #fff url("./../assets/delete.png") center / cover no-repeat scroll;

  transition: transform .2s;
}
.item__delete:hover{
  transform: scale(1.5);
  cursor: pointer;
}
</style>
