<template>
  <!-- Contenedor principal del chat -->
  <div class="superchat">
    <!-- User 01 - muestra imagen avatar y nombre del usuario 1-->
    <div class="panel_user" v-if="usuarios.length">
      <img :src="usuarios[0].picture.large" class="avatar" />
      <p class="nombre">
        {{ usuarios[0].name.first }} {{ usuarios[0].name.last }}
      </p>
      <!-- Permite al usuario 1 elegir un color para sus mensajes -->
      <input type="color" v-model="userColor1" />
      <textarea v-model="msgUser1" placeholder="Ingrese aquí su mensaje" />
      <button @click="enviarMensaje(1)">Enviar</button>
    </div>

    <!-- Chat -->
    <div class="Chats">
      <!-- Asigna una clase diferente al mensaje dependiendo del emisor (1 o 2) -->
      <div
        v-for="(mensaje, index) in mensajes"
        :key="index"
        :class="mensaje.emisor === 1 ? 'msg1' : 'msg2'"
        :style="{
          backgroundColor: mensaje.emisor === 1 ? userColor1 : userColor2,
        }"
        class="mensaje"
      >
        <!-- Muestra el nombre y mensaje del usuario -->
        <p>
          <strong>
            {{
              mensaje.emisor === 1
                ? usuarios[0].name.first + " " + usuarios[0].name.last
                : usuarios[1].name.first + " " + usuarios[1].name.last
            }}
            :
          </strong>
          {{ mensaje.texto }}
        </p>
      </div>
    </div>

    <!-- User 02 -->
    <div class="panel_user" v-if="usuarios.length">
      <img :src="usuarios[1].picture.large" class="avatar" />
      <p class="nombre">
        {{ usuarios[1].name.first }} {{ usuarios[1].name.last }}
      </p>
      <input type="color" v-model="userColor2" />
      <textarea v-model="msgUser2" placeholder="Escribe tu mensaje" />
      <button @click="enviarMensaje(2)">Enviar</button>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  // almacena info de usuarios, mensajes y colores para mensajes.
  data() {
    return {
      usuarios: [],
      mensajes: [],
      msgUser1: "",
      msgUser2: "",
      userColor1: "#F5F5F5",
      userColor2: "#E7FECC",
    };
  },
  // Método que corre cuando el componente se monta, Realiza un GET a la API y obtiene 2 users aleatoros
  async mounted() {
    const { data } = await axios.get("https://randomuser.me/api/?results=2");
    this.usuarios = data.results;
  },

  // Método para enviar un mensaje al chat
  methods: {
    enviarMensaje(emisor) {
      const texto = emisor === 1 ? this.msgUser1 : this.msgUser2;
      // Verifica que el mensaje no esté vacío
      if (texto.trim()) {
        // Agrega el mensaje al array 'mensajes' con la información del emisor y el texto, luego limpia el textarea
        this.mensajes.push({ emisor, texto });
        emisor === 1 ? (this.msgUser1 = "") : (this.msgUser2 = "");
      }
    },
  },
};
</script>
