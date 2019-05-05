<template>
  <div class="hello">
    <div class="container">
      <div class="tabs">
        <span @click="isReg=false" :class="isReg?'':'active'">Вход</span>
        <span @click="isReg=true" :class="isReg?'active':''">Регистрация</span>
      </div>
      <div class="slider" :class="isReg?'active':''"></div>
      <div class="forms-container" :class="isReg?'active':''">
        <form action="">
          <input type="text" v-model="authLogin" placeholder="Логин">
          <input type="text" v-model="authPass" placeholder="Пароль">
          <input @click.prevent='Auth' type="submit" value="Авторизация">
        </form>
        <form action="">
          <input type="text" v-model="regLogin" placeholder="Логин">
          <input type="text" v-model="regPass" placeholder="Пароль">
          <input @click.prevent='Reg' type="submit" value="Регистрация">
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'Auth',
  data () {
    return {
      isReg: false,
      authLogin: '',
      authPass: '',
      regLogin: '',
      regPass: '',
      token: ''
    }
  },
  methods: {
    Reg: function () {
      axios.post('//localhost:3000/reg',
        {
          login: this.regLogin,
          pass: this.regPass
        }).then(response => {
        if (response.data.token) {
          localStorage.setItem('token', response.data.token)
          this.$router.push({name: 'Home'})
        } else {
          alert(response.data.error)
        }
      })
    },
    Auth: function () {
      axios.post('//localhost:3000/auth',
        {
          login: this.authLogin,
          pass: this.authPass
        }).then(response => {
        if (response.data.token) {
          localStorage.setItem('token', response.data.token)
          this.$router.push({name: 'Home'})
        } else {
          alert(response.data.error)
        }
      })
    }
  },
  beforeCreate: function () {
    if (localStorage.getItem('token')) {
      this.$router.push({name: 'Home'})
    }
  }
}
</script>

<style scoped>
  .container{
    background: #444764;
    color: #efefef;
    width: 300px;
    overflow: hidden;
    margin: 40px auto 0 auto;
    border-radius: 2px;
    box-shadow: 0 4px 5px 0 rgba(0, 0, 0, 0.14), 0 1px 10px 0 rgba(0, 0, 0, 0.12), 0 2px 4px -1px rgba(0, 0, 0, 0.4);
  }
  .tabs{
    display: flex;
    align-items: center;
  }
  .tabs>span{
    font-size: 18px;
    width: 50%;
    text-align: center;
    padding: 10px;
    cursor: pointer;
    transition: .2s ease-out;
  }
  .tabs>span.active{
    background: #4e5273;
  }
  form{
    width: 50%;
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .forms-container{
    display: flex;
    flex-direction: row;
    width: 200%;
    position: relative;
    left: 0;
    transition: .2s ease-out;
  }
  .forms-container.active{
    left: -100%;
  }
  input[type='text'],
  input[type='password']{
    padding: 5px;
    border: 1px solid #eeeeee;
    margin-top: 20px;
    -webkit-border-radius: 20px;
    -moz-border-radius: 20px;
    border-radius: 20px;
    color: #444;
  }
  input[type='submit']{
    cursor: pointer;
    margin-top: 20px;
    margin-bottom: 20px;
    padding: 7px 20px;
    -webkit-border-radius: 40px;
    -moz-border-radius: 40px;
    border-radius: 40px;
    border:none;
    color: #ffffff;
    background: #4e5273;
    transition: .1s ease-out;
    box-shadow:  0 2px 2px 0 rgba(0, 0, 0, 0.14), 0 1px 5px 0 rgba(0, 0, 0, 0.12), 0 3px 1px -2px rgba(0, 0, 0, 0.2);
  }
  input[type='submit']:active{
    box-shadow:  none;
  }
  input[type='submit']:hover{
    background: #575c81;
  }
  .slider{
    background: #575c81;
    height: 4px;
    width: 50%;
    position: relative;
    transition: .2s ease-out;
    left: 0;
  }
  .slider.active{
    left: 50%;
  }
</style>
