<template>
  <div class="home">
    <div class="main-menu">
      <button class=" btn btn-menu" @click="mainMenuIsOpen = !mainMenuIsOpen">Меню</button>
      <ul class="window" :class="mainMenuIsOpen?'open':''">
        <li @click="Logout">Выход</li>
      </ul>
    </div>
    <div class="console">
      <ul>
        <li v-for="item in consoleList">{{ item }}</li>
      </ul>
      <input @keydown.enter="ConsoleSend" v-model="consoleInput" type="text" class="console-input">
    </div>
    <canvas id="game"></canvas>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'Home',
  data () {
    return {
      token: '',
      mainMenuIsOpen: false,
      consoleInput: '',
      consoleList: [],
      sprites: null,
      canvas: document.getElementById('game')
    }
  },
  sockets: {
    join: function (data) {
      if (!data.status) {
        localStorage.removeItem('token')
        this.$router.push({name: 'Auth'})
      }
    },
    console: function (data) {
      this.consoleList.push(data)
    }
  },
  methods: {
    Logout: function () {
      console.log(this.token)
      axios.post('//localhost:3000/logout',
        {
          token: this.token
        }).then(response => {
        if (response.data.status) {
          localStorage.removeItem('token')
          this.$router.push({name: 'Auth'})
        }
      })
    },
    Join: function () {
      this.$socket.emit('join', {token: this.token})
      let image = new Image()
      image.src = './static/sprites.png'
      image.onload = () => {
        this.sprites = image
        this.StartRender()
      }
    },
    StartRender: function () {
      let canvas = document.getElementById('game')
      let ctx = canvas.getContext('2d')
      ctx.canvas.width = window.innerWidth
      ctx.canvas.height = window.innerHeight
      ctx.fillStyle = '#333'
      ctx.font = '20px Georgia'
      ctx.fillRect(0, 0, canvas.width, canvas.height)
      ctx.drawImage(this.sprites, 24, 0, 24, 24, 100, 100, 24, 24)
    },
    ConsoleSend: function () {
      if (this.consoleInput.length > 0) {
        this.$socket.emit('console', this.consoleInput)
        this.consoleInput = ''
      }
    }
  },
  mounted: function () {
    if (localStorage.getItem('token')) {
      this.token = localStorage.getItem('token')
      this.Join()
    } else {
      this.$router.push({name: 'Auth'})
    }
  }
}
</script>

<style scoped>
  #game{
    position: absolute;
    width: 100%;
    height: 100%;
  }
  .home {
    position: absolute;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
  }

  .main-menu {
    position: absolute;
    z-index: 10;
    left: 15px;
    top: 15px;
  }

  .window {
    display: none;
    opacity: 0;
    transition: .1s ease-out;
  }

  .window.open {
    display: block;
    opacity: 1;
  }

  .main-menu ul {
    width: 200px;
    margin-top: 5px;
    -webkit-border-radius: 4px;
    -moz-border-radius: 4px;
    border-radius: 4px;
    overflow: hidden;
    background: #444764;
    color: #eeeeee;
    box-shadow: 0 2px 2px 0 rgba(0, 0, 0, 0.14), 0 1px 5px 0 rgba(0, 0, 0, 0.12), 0 3px 1px -2px rgba(0, 0, 0, 0.2);
  }

  .main-menu li {
    padding: 5px 5px 5px 20px;
    font-size: 14px;
    border-top: 1px solid #575c81;
    border-bottom: 1px solid #575c81;
    transition: .1s ease-out;
    cursor: pointer;
  }

  .main-menu li:hover {
    background: #575c81;
  }

  .btn {
    margin: 0;
    cursor: pointer;
    padding: 7px 20px;
    -webkit-border-radius: 40px;
    -moz-border-radius: 40px;
    border-radius: 40px;
    border: none;
    color: #ffffff;
    background: #4e5273;
    transition: .1s ease-out;
    box-shadow: 0 2px 2px 0 rgba(0, 0, 0, 0.14), 0 1px 5px 0 rgba(0, 0, 0, 0.12), 0 3px 1px -2px rgba(0, 0, 0, 0.2);
  }

  .btn:active {
    box-shadow: none;
  }

  .btn:hover {
    background: #575c81;
  }

  .console {
    position: absolute;
    z-index: 10;
    bottom: 15px;
    left: 15px;
    width: 400px;
    background: rgba(255, 255, 255, 0.62);
    box-shadow: 0 2px 2px 0 rgba(0, 0, 0, 0.14), 0 1px 5px 0 rgba(0, 0, 0, 0.12), 0 3px 1px -2px rgba(0, 0, 0, 0.2);
    -webkit-border-radius: 4px;
    -moz-border-radius: 4px;
    border-radius: 4px;
    overflow: hidden;
  }

  .console ul {
    max-height: 400px;
  }

  .console li {
    padding: 5px 10px;
    color: #333;
  }

  .console-input {
    width: 100%;
    box-sizing: border-box;
    background: rgba(51, 51, 51, 0.29);
    padding: 5px;
    border: none;
    font-weight: bold;
    color: #444444;
  }
</style>
