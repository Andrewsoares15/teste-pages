<template>
  <v-container>
    <div class="header">
      <h1 class="text-left">Challenge</h1>
      <v-card
    
      flat
    >
      <v-toolbar
        flat
        height="72"
        class="text-right"
      >
        <v-switch
          v-model="$vuetify.theme.dark"
          inset
          persistent-hint
        ></v-switch>
      </v-toolbar>

    </v-card>
  </div>
  <v-form
    ref="form"
    v-model="valid"
    lazy-validation
  >
    <v-text-field
      v-model="nam"
      :counter="30"
      :rules="nameRules"
      label="Name"
      required
    ></v-text-field>
    <v-btn
      :disabled="!valid"
      color="success"
      class="mr-4"
      @click="validate"
      
    >
      Validate
    </v-btn>
  </v-form>
   
    <div v-if="dados">
      <div class="header-el">
        <v-avatar size="156" class="mt-4">
      <img
        :src="contas.avatar_url"
        :alt="contas.login"
      >
    </v-avatar>
    <ul class="mt-2">
      <li>@{{contas.login}}</li>
      <li>{{contas.bio}}</li>
    </ul>
    
      </div>
     <v-simple-table class="tabela mt-4">
    <template >
      <thead>
        <tr>
          <th class="text-left">
            Repositorios
          </th>
          <th class="text-left">
            Seguidores
          </th>
          <th class="text-left">
            Seguindo
          </th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>{{ contas.public_repos }}</td>
          <td>{{ contas.followers }}</td>
          <td>{{ contas.following }}</td>
        </tr>
      </tbody>
    </template>
  </v-simple-table>
  <ul class="mt-4 info-user">
    <li>
      <v-avatar size="20"> 
      <img src="../assets/local2.png" alt="">
      </v-avatar><span class="ml-2">{{contas.location}}</span>
      </li>
      <li>
      <v-avatar size="20"> 
      <img src="../assets/link.png" alt="">
      </v-avatar><span class="ml-2">{{contas.html_url}}</span>
      </li>
      <li>
      <v-avatar size="20"> 
      <img src="../assets/origem.png" alt="">
      </v-avatar><span class="ml-2">{{contas.created_at}}</span>
      </li>
      <li>
      <v-avatar size="20"> 
      <img src="../assets/twitter.png" alt="">
      </v-avatar><span class="ml-2">{{contas.twitter_username}}</span>
      </li>
  </ul>
    
    </div>
  </v-container>
</template>

<script>
  export default {
    name: 'HelloWorld',

    data: () => ({
       valid: true,
       dados: false,
       nam: '',
       nameRules: [
        v => !!v || 'Name is required',
        v => (v && v.length <= 30) || 'Name must be less than 30 characters',
      ],
      contas: [],
      
    }),
    props: {
      attrs: {
        type: Object,
        default: () => ({}),
      },
    },
      methods: {
      validate () {
        this.$refs.form.validate()
        console.log(this.nam)
        console.log(this.dados)
        this.chamarApi()
      }, 
      chamarApi(){
         fetch('https://api.github.com/users/' + this.nam)
        .then(resultado => resultado.json())
        .then(json => {
          this.contas = json
          console.log(this.contas)
          this.mostraDados()
        }) 
      },
      mostraDados(){
        this.dados = true;
      }
    },
  }
  
</script>
<style scoped>
div.v-toobar_content{
  background: red;
}
.header{
  display: flex;
  justify-content: space-between;
}
.header-el{
  display: flex;
  align-items: center;
  justify-content: space-between;
}
ul{
  list-style: none;
}
.infos{
  display: flex;
  justify-content: space-between;
  font-size: 20px;
  margin: 10px 0 4px 0;
  }
.tabela{
  background-color: red;
}
.info-user{
  padding: 5px;
}
</style>