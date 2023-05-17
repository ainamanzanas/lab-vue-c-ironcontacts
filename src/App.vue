<template>
  <h1>Contacts</h1>
  
    <table>
      <tr>
        <th>IronContacts</th>
      </tr>
      <tr>
        <th>Picture</th>
        <th>Name</th>
        <th>Popularity</th>
        <th>Won Oscar</th>
        <th>Won Emmy</th>
      </tr>
      <tr v-for="contact in contacts"
        :key="contact.id">
        <td><img class="smallPicture" :src="contact.pictureUrl" /></td>
        <td>{{ contact.name }}</td>
        <td>{{ contact.popularity }}</td>
        <td v-if="contact.wonOscar">🏆</td>
        <td v-else>    </td>
        <td v-if="contact.wonEmmy">🏆</td>
        <td v-else>    </td>
        <td><button @click="_deleteContact(contact)">Delete</button> </td>
      </tr>
    </table>
    <button @click="_addRandomContact">Add Contact</button>
    <button @click="_sortName">Sort by Name</button>
    <button @click="_sortPopularity">Sort by Popularity</button>
    
  </template>
  
  <script>
  import { computed } from "@vue/reactivity";
  import contactsJson from "./contacts.json";
  
  export default {
    name: 'App',
    data() {
      return {
        dataContact: contactsJson,
        contacts: [],
        contactsNotShowed: [],
        numberOfContacts: 5,
      }
    },
    created() {
     this._firstFiveContact();
    },
    methods: {
       _firstFiveContact() {
        const sizeArr = this.dataContact.length;
        this.contacts = this.dataContact.slice(0,this.numberOfContacts)
        this.contactsNotShowed = this.dataContact.slice(this.numberOfContacts,sizeArr)
  
        console.log(sizeArr);
        console.log(this.contacts);
        console.log(this.contactsNotShowed)
      },
  
      _addRandomContact() {
       const randomContact = Math.floor(Math.random() * this.contactsNotShowed.length)
       this.contacts.push(this.contactsNotShowed[randomContact]);
       this.contactsNotShowed.splice(randomContact,1)
  
       console.log(randomContact)
       console.log(this.contacts);
       console.log(this.contactsNotShowed);
      },
      _sortName(){
        this.contacts.sort(function (a,b){
          if(a.name < b.name){
            return -1;
          }
          if (a.name > b.name) {
            return 1;
          }
          return 0
        })
      },
      _sortPopularity(){
        this.contacts.sort(function (a,b){
          if(a.popularity < b.popularity){
            return 1;
          }
          if (a.popularity > b.popularity) {
            return -1;
          }
          return 0
        })
      },
      _deleteContact(contactDelete) {
        const contactIdSelected = contactDelete.id
        const indexOfContact = this.contacts.indexOf(contactDelete)
        this.contacts.splice(indexOfContact,1)
        this.contactsNotShowed.push(contactDelete)
      }
    },
  }
  
  </script>
  
  <style scoped>
  .smallPicture{
    width: 40px;
  }
  
  </style>