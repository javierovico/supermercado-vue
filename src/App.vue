<template>
  <div id="principal">
    <header>
      <!-- Dropdown Structure -->
      <ul id="dropdown1" class="dropdown-content">
        <li><a v-on:click="cambiarSeccion('categorias')">Categorias</a></li>
        <li><a v-on:click="cambiarSeccion('productos')">Productos</a></li>
        <li class="divider"></li>
        <li v-if="auth.iniciado">
          <a v-on:click="cerrarSesion">Salir</a>
        </li>
        <li v-else>
          <a v-on:click="cambiarSeccion('iniciar')">Iniciar</a>
          <a v-on:click="cambiarSeccion('registro')">Registrarse</a>
        </li>
      </ul>
      <nav>
        <div class="nav-wrapper">
          <a href="#" v-on:click="cambiarSeccion('inicio')" data-target="mobile-demo" class="brand-logo">Logo</a>
          <a href="#" data-target="mobile-demo" class="sidenav-trigger"><i class="material-icons">menu</i></a>
          <ul class="right hide-on-med-and-down">
            <!-- Dropdown Trigger -->
            <li>
              <a class="dropdown-trigger" href="#!" data-target="dropdown1">
                {{auth.iniciado?auth.name:'Invitado'}}
                <i class="material-icons right">arrow_drop_down</i>
              </a>
            </li>
          </ul>
        </div>
      </nav>
      <ul class="sidenav" id="mobile-demo">
        <li><a v-on:click="cambiarSeccion('categorias')">Categorias</a></li>
        <li><a v-on:click="cambiarSeccion('productos')">Productos</a></li>
        <li class="divider"></li>
        <li v-if="auth.iniciado">
          <a v-on:click="cerrarSesion">Salir</a>
        </li>
        <li v-else>
          <a v-on:click="cambiarSeccion('iniciar')">Iniciar</a>
          <a v-on:click="cambiarSeccion('registro')">Registrarse</a>
        </li>
      </ul>
    </header>
    <main class="py-4">
      <div class="container">
        <lista-productos v-if="sel === 'productos'"></lista-productos>
        <seccion-categoria :_idPadre="null" v-if="sel === 'categorias'"></seccion-categoria>
        <iniciar-view @checkUser="checkUser();cambiarSeccion('inicio')" v-if="sel === 'iniciar'"></iniciar-view>
      </div>
    </main>
    <footer class="page-footer">
      <div class="container">
        <div class="row">
          <div class="col l6 s12">
            <h5 class="white-text">Footer Content</h5>
            <p class="grey-text text-lighten-4">You can use rows and columns here to organize your footer content.</p>
          </div>
          <div class="col l4 offset-l2 s12">
            <h5 class="white-text">Links</h5>
            <ul>
              <li><a class="grey-text text-lighten-3" href="#!">Link 1</a></li>
              <li><a class="grey-text text-lighten-3" href="#!">Link 2</a></li>
              <li><a class="grey-text text-lighten-3" href="#!">Link 3</a></li>
              <li><a class="grey-text text-lighten-3" href="#!">Link 4</a></li>
            </ul>
          </div>
        </div>
      </div>
      <div class="footer-copyright">
        <div class="container">
          © 2014 Copyright Text
          <a class="grey-text text-lighten-4 right" href="#!">More Links</a>
        </div>
      </div>
    </footer>
  </div>
</template>

<script>
const axios = require('axios').default;
const $ = require('jquery');
// import HelloWorld from './components/HelloWorld.vue'
import SeccionCategoria from './components/CategoriasView'
import ListaProductos from "@/components/ListaProductos";
import IniciarView from "@/components/IniciarView";

export default {
  name: 'App',
  data() {
    return {
      auth:{
        iniciado: false,
        name: ''
      },
      sel: 'categorias'
    }
  }, components: {
    SeccionCategoria,
    ListaProductos,
    IniciarView
  },
  mounted() {
    this.checkUser();
    $(document).ready(function(){
      $('.sidenav')
              .sidenav()
              .on('click tap', 'li a', () => {
                $('.sidenav').sidenav('close');
              });
    });
  },
  methods: {
    cambiarSeccion: function (secc) {
      this.sel = secc;
    },
    cerrarSesion: function () {
      axios.post('/logout').then((response) => {
        console.log(response);
        this.auth.iniciado = false;
        this.auth.name = '';
      }).catch((error)=>{
        console.error(error.response);
      });
    },
    checkUser: function () {
      axios.get('/checkUser').then((response) => {
        this.auth = response.data;
      }).catch((error)=>{
        console.error(error.response);
      });
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
