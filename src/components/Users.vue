<template>
  <v-container>
    <v-card elevation="0">
      <v-form>
        <v-row>
          <v-col cols="3">
            <v-text-field
                v-model="name"
                label="Emri"
                outlined
                dense
                @input="showAlert=false"
            />
          </v-col>
          <v-col cols="2">
            <v-select
                v-model="userRole"
                :items="roles"
                outlined
                dense
            />
          </v-col>
          <v-col cols="3">
            <v-btn
                color="blue"
                @click="addUser"
            >
              + Shto
            </v-btn>
          </v-col>
        </v-row>
      </v-form>
      <v-divider/>
      <v-row class="mt-3">
        <v-col cols="3">
          <v-text-field
              outlined
              dense
              v-model="search"
              append-icon="mdi-magnify"
              label="Kërko"
          ></v-text-field>
        </v-col>

      </v-row>
    </v-card>
    <v-alert
        dense
        outlined
        type="error"
        v-if="showAlert"
    >
      Emri i përdoruesit duhet të ketë më shumë se dy shkronja.
    </v-alert>
    <v-list v-for="(user,index) in filteredList" :key="index">
      <v-card elevation="1">
        <v-list-item>
          <v-list-item-avatar>
            <v-img src="https://upload.wikimedia.org/wikipedia/commons/thumb/5/59/User-avatar.svg/2048px-User-avatar.svg.png"></v-img>
          </v-list-item-avatar>
          <v-list-item-content>
            <v-list-item-title>{{user.name}}</v-list-item-title>
            <v-list-item-subtitle><b>{{user.role}}</b></v-list-item-subtitle>
          </v-list-item-content>
          <v-spacer></v-spacer>
          <v-btn
              color="red darken-1"
              text
              @click="deleteUser(index)"
          >
              &#10060;
          </v-btn>
        </v-list-item>
      </v-card>
    </v-list>
    <v-row>
      <v-alert v-if="!filteredList.length" style="color: gray">0 rezultate nga kërkimi juaj.</v-alert>
    </v-row>
  </v-container>
</template>

<script>

  export default {
    name: 'Users',
    data() {
      return {
        name: '',
        roles: ['Mësues', 'Student'],
        userRole: 'Student',
        users: [],
        showAlert: false,
        search: '',
    }
    },
    mounted() {
      this.users = JSON.parse(localStorage.getItem('users'))
    },
    methods: {
      addUser () {
        if(this.name.length < 2) {
          this.showAlert = true;
        } else {
          this.users.push({
            name: this.name,
            role: this.userRole
          })
          localStorage.setItem('users', JSON.stringify(this.users))
          this.name = ''
          this.userRole = 'Student'
        }


      },

      deleteUser(id) {
        this.users.splice(id,1)
        localStorage.setItem('users', JSON.stringify(this.users))
      }
    },
    computed: {
      filteredList() {
        return this.users.filter(user => {
          return user.name.toLowerCase().includes(this.search.toLowerCase())
        })
      }
    }
  }
</script>
