<template>
  <v-layout column justify-center align-center>
    <v-flex xs12 sm8 md6>
      <v-card>

        <v-text-field type="text" v-model="name" placeholder="NAME"></v-text-field>
        <v-text-field type="text" v-model="tel" placeholder="TEL"></v-text-field>
        <v-btn @click="insertToContact(tel, name)">Add</v-btn>

        <ul :key="key" v-for="(contact, key) in contacts">
          <li>
            {{contact.name}} : {{contact.tel}} {{key}}
            <v-btn @click="deleteContact(key)">Delete</v-btn>
          </li>
        </ul>
        <br />
        <br />
        <br />
        <ul :key="key" v-for="(contact, key) in contacts">
          <li v-if="updateKey === key">
            <v-text-field type="text" v-model="updateName" placeholder="NAME"></v-text-field>
            <v-text-field type="text" v-model="updateTel" placeholder="TEL"></v-text-field>
            <v-btn @click="updateContact(updateTel, updateName)">Save</v-btn>
          </li>
          <li v-else>
            {{contact.name}} : {{contact.tel}}
            <v-btn @click="setUpdateContact(key, contact)">Update</v-btn>
            <v-btn @click="deleteContact(key)">Delete</v-btn>
          </li>
        </ul>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
  import Logo from '@/components/Logo.vue'
  import VuetifyLogo from '~/components/VuetifyLogo.vue'
  import Swal from 'sweetalert2'
  // var database = this.$fire.database()
  // var contactRef = this.$fire.ref('/contacts')


  export default {
    components: {
      Logo,
      VuetifyLogo
    },
    data() {
      return {
        contacts: {},
        tel: '',
        name: '',
        contactRef: this.$fire.database.ref('/contacts'),
        updateTel: '',
        updateName: '',
        updateKey: ''
        // database : this.$fire.database(),


      }
    },
    mounted() {

      this.contactRef.on('value', (snapshot) => {
        this.contacts = snapshot.val()
      })
    },
    methods: {

      async test() {
        try {
          await this.$fire.auth.createUserWithEmailAndPassword('foo@foo.foo', 'test1235')
        } catch (e) {
          Swal.fire({
            icon: 'error',
            title: 'Oops...',
            text: e.message,
            // footer: '<a href>Why do I have this issue?</a>'
          })
          console.log(e);
        }
      },
      insertToContact(tel, name) {
        // const contactRef = this.$fire.database.ref('/contacts');
        let data = {
          tel: tel,
          name: name
        }
        this.contactRef.push(data)
        this.tel = ''
        this.name = ''
      },
      deleteContact(key) {
        // const this.contactRef = this.$fire.database.ref('/contacts');
        this.contactRef.child().remove()
      },
      setUpdateContact(key, contact) {
        this.updateKey = key
        this.updateTel = contact.tel
        this.updateName = contact.name
      },
      updateContact(tel, name) {
        this.contactRef.child(this.updateKey).update({
          tel: tel,
          name: name
        })
        this.updateKey = ''
        this.updateTel = ''
        this.updateName = ''
      },
    },

  }
</script>