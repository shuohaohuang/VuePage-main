<!-- eslint-disable vue/valid-v-for -->
<template>
  <div class="menu">
    <h1>Mi pagina</h1>
    <article class="buttons">
      <navigation v-for="page in pageStatus" :status="page" @ChangeStatus="ChangeStatus"></navigation>
    </article>
  </div>

  <div v-if="page == 'Home'" class="ContentPanel">
    <viewer v-for="game in currentContent" class="gamesContent" :title="game.name" :description="game.description"
      :image="game.image">
    </viewer>
  </div>

  <div class="contentPanel2" v-if="page == 'CRUD'">
    <button v-if="IsFalse" @click="addNew = true" class="add">Nou</button>
    <cr v-if="!IsFalse" :array="[...Content]" @AppendArray="Apend" @ChangeAdd="ChangeButton"></cr>
    <ud :array="[...Content]" @NewContent="ActualizeContent"></ud>

  </div>



  <div v-if="page == 'API'">
    <Seeker @search="ConectaApi"></Seeker>
    <Show :pokemonToShow="pokemon"></Show>
  </div>
</template>

<script>
import Seeker from "./components/Seeker.vue";
import Show from "./components/Show.vue";
import viewer from '@/components/ShowGame.vue'
import navigation from '@/components/Status.vue'
import ud from '@/components/CrudComponents/Ud.vue'
import cr from "@/components/CrudComponents/CR.vue"
export default {
  components: {
    viewer,
    navigation,
    ud,
    Seeker,
    Show,
    cr
  },
  data() {
    return {
      Content: [
        {
          name: 'Dead Cells',
          description:
            'Dead Cellss es un videojuego híbrido entre el género de exploración de mazmorras o roguelite, y metroidvania, creando su propio género, “roguevania”, desarrollado por el estudio Motion Twin.1​ El juego fue lanzado para PC, Mac, Linux, Playstation 4, Nintendo Switch y Xbox One,1​ siendo lanzado en la plataforma de Steam el 6 de agosto de 2018, bajo la categoría de juego de acción e indie.',
          image: '/src/components/img/Dead Cells.jpg'
        },
        {
          name: 'Pokemon XY',
          description:
            'Pokémon X y Pokémon Y, conocidos en Japón como Pocket Monsters X&Y (ポケットモンスター X&Y Poketto Monsutā Ekkusu & Wai?), son dos videojuegos de RPG y aventura, desarrollados por Game Freak y distribuidos por Nintendo para la consola portátil Nintendo 3DS, que fueron lanzados el 12 de octubre de 2013 tanto en Japón como en América, Europa y Australia.2​ Pertenecen a la serie de videojuegos Pokémon, e inauguran la sexta generación de la misma. Fueron anunciados el 8 de enero de 2013 durante el Pokémon Direct por Satoru Iwata, presidente de Nintendo.3​',
          image: '/src/components/img/Pokemon.jpg'
        }
      ],
      page: 'Home',
      pageStatus: ['Home', 'CRUD', 'API'],
      addNew: false
    }
  },
  computed: {
    IsHome() {
      return this.CurrentStatus == 'Home'
    }, currentContent() {
      return this.Content;
    },

    IsFalse() {
      return this.addNew == false;
    }
  },
  methods: {
    ChangeStatus(status) {
      this.page = status
    },
    ConectaApi(pokemonName) {
      this.bValue = !this.bValue;

      fetch("https://pokeapi.co/api/v2/pokemon/" + pokemonName.toLowerCase())
        .then(res => {
          if (res.ok) {
            res.json().then((json) => {
              this.pokemon = json;
            })
          } else {
            throw new Error("")
          }
        })
        .catch((err => {
          console.log(err);
        }))
    },
    ActualizeContent(newContent) {
      this.Content = JSON.parse(JSON.stringify(newContent))
    },
    Apend(content) {
      console.log(content)
      this.Content.push(content)
    },
    ChangeButton(value) {
      console.log(value)
      this.addNew = value;
      console.log(this.addNew)
      console.log(this.IsFalse)
    }
  }
}
</script>

<style scoped>
.ContentPanel {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  justify-content: space-around;
}

.add {
  margin-left: 50%;
  width: 90px;
  height: 60px;
}

.ContentPanel2 {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.gamesContent {
  display: flex;
  flex-wrap: wrap;
}

.menu {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin-bottom: 10px;
  height: 130px;
  background-color: aliceblue;
}

.buttons {
  position: relative;
  right: 20px;
  margin-right: 5%;
  width: 100%;
  display: flex;
  justify-content: end;
  flex-direction: row;
}

h1 {
  position: relative;
  top: -15px;
  width: auto;
  text-align: center;
  border-bottom: 1px solid rgb(192, 192, 192);
  border-radius: 8%;
}
</style>
