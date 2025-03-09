<script setup lang="ts">
import EvaluationItem from './EvaluationItem.vue'
import DocumentationIcon from './icons/IconDocumentation.vue'
import NumOneIcon from './icons/IconOne.vue'
import NumTwoIcon from './icons/IconTwo.vue'
import axios from 'axios'
import { ref } from 'vue'
const userName = ref('')
let activeColor = ref('green')
let loading = ref('false');

// CSC : correctif de results[1] en results[0] , mauvais indice pour récupérer les infos du fichier distant JSON (API REST)
function getUser() {
  loading.value = true;
  axios.get('https://randomuser.me/api/')
  .then(function (response) {
    const name = response.data.results[0].name;
    userName.value = name.title + ' ' + 
                     name.first + ' ' + 
                     name.last;
  })
  .catch(function (error) {
    activeColor = ref('red')
    userName.value = error
  }).finally(() => {
          loading.value = false;
   });
}

const ageOfRegistered = '';

</script>
<script  lang="ts">
export default {
  data() {
    return {
      idVueJs: 'divVueJs',
      loading: false,
    };
  },
};
</script>

<template>
  <EvaluationItem>
    <template #icon>
      <DocumentationIcon />
    </template>
    <template #heading>Présentation</template>

    Vous trouverez ci-dessous une liste de tâches de difficultés croissantes, afin d'évaluer votre maitrise de : <u><span id="idJs" >Javascript</span>, <span style="color:chartreuse" 
                     onmouseover="this.style.color='yellow';this.style.fontWeight='bold'"  onmouseout="this.style.color='chartreuse';this.style.fontWeight='normal'" >HTML</span>, <span class="colorCSS">CSS</span> et <span v-bind:id="idVueJs">VueJS</span></u>.<br/>
    <br/>    

  </EvaluationItem>

  <EvaluationItem>
    <template #icon >
      <NumOneIcon class="icon"/>
    </template>
    <template #heading>Chargement dynamique</template>

    Votre première mission sera de modifier dynamiquement les 4 mots soulignés de la présentation ci-dessus, en utilisant le langage ou la technologie dont elle porte le nom.<br/>
    <i>Exemple: Le mot 'CSS' devra avoir un attribut, comme sa couleur, modifié en CSS. 'VueJS' sera chargé depuis une variable reactive. Etc.</i><br/>
    <b>Faites preuve d'imagination et montrez que vous maitrisez chaque sujet.</b>

  </EvaluationItem>

  <EvaluationItem>
    <template #icon>
      <NumTwoIcon class="icon"/>
    </template>
    <template #heading>Débogage Javascript</template>
    Ici, il y a un bogue qui empêche d'afficher le nom d'une personne. Vous devez comprendre le résultat de la requête et corriger ce bogue. <br/>
    En bonus, vous devrez afficher l'email et l'image au format vignette de la personne.<br/>
    <button variant="primary" v-on:click="getUser" v-bind:disabled="loading.value">Obtenir un nom</button>
    <div class="loader" v-if="loading.value">
      <img class="loaderImg" src="../assets/spinner2.gif">
    </div>

    <br/>
    <p :style="{color: activeColor}"> {{ userName }} </p>
  </EvaluationItem>

  <EvaluationItem>
    <template #icon>
      <NumTwoIcon class="icon"/>
    </template>
    <template #heading>Actualiser le résultat</template>
    D'abord, le numéro de cette étape devrait être un 3, donc à vous de corriger ça !<br/>
    Ensuite, vous devrez créer une propriété Computed, qui calcule et actualise l'age qu'avait la personne ci-dessus, lors de son inscription.<br/>
    <div v-if="ageOfRegistered != ''" class="age">
      Son age à l'inscription était {{ ageOfRegistered }} ans.
    </div>
    <br/>
  </EvaluationItem>

</template>

<style scoped>
.icon{
  width: 24px;
  height: 24px;
}
.age{
  color: blue;
  
}
.colorCSS{
  color: cornflowerblue;
}
.colorCSS:hover{
  color: cyan ;
  font-weight: bold;
}

#divVueJs{
  color:darkgreen;
}
#divVueJs:hover{
  color:darkseagreen;
  font-weight: bold;
}

.loader{  /* Loader Div Class */
    position: absolute;
    top:0px;
    right:0px;
    width: 100px;
    height:100px;
    align-content: center;
    background-color:#eceaea;
    background-image: url('../assets/ajax-loader.gif');
    background-size: 50px;
    background-repeat:no-repeat;
    background-position:center;
    z-index:10000000;
    opacity: 0.4;
    filter: alpha(opacity=40);
}

</style>