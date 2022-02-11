<template>
    <div class="container">
        <div class="row justify-content-center my-5">
            <div class="col-md-8">
                <div class="card">
                    <div class="card-header d-flex justify-content-between">
                        <h1 class="fw-bold">Contacts</h1>
                        <button class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#addContact" @click="contact={name:'',tel:''}">
                            Ajouter un Contact
                        </button>
                    </div>
                    <div class="search-box shadow">
                        <input type="text" class="form-control" v-model="contact_search" placeholder="Chercher ...">
                    </div>
                    <div class="card-body">
                        <table class="table">
                            <thead>
                                <tr>
                                <th scope="col">id</th>
                                <th scope="col">Nom</th>
                                <th scope="col">Telephone</th>
                                <th scope="col">Actions</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="contact in filtreContacts" :key="contact.id">
                                    <th scope="row">{{ contact.id }}</th>
                                    <td>{{ contact.name }}</td>
                                    <td>{{ contact.tel }}</td>
                                    <td>
                                        <button class="btn btn-primary" @click="getContact(contact.id)" data-bs-toggle="modal" data-bs-target="#editContact">
                                            <i class="fas fa-edit"></i>
                                        </button>

                                        <button class="btn btn-danger" data-bs-toggle="modal" @click="getContact(contact.id)" data-bs-target="#delete"><i class="fas fa-trash"></i></button>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
                <!-- Modal -->
                <!-- add modal  -->
                <div class="modal fade" id="addContact" data-bs-backdrop="static" data-bs-keyboard="false" tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
                    <div class="modal-dialog">
                        <div class="modal-content">
                        <div class="modal-header">
                            <h5 class="modal-title" id="staticBackdropLabel">Ajouter un nouveau contact</h5>
                            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                        </div>
                        <div class="modal-body">
                            <form>
                                <div class="mb-3">
                                    <label class="form-label">Nom</label>
                                    <input type="text" v-model="contact.name" class="form-control" id="exampleFormControlInput1" placeholder="Nom">
                                </div>
                                <div class="mb-3">
                                    <label class="form-label">Telephone</label>
                                    <input type="text" v-model="contact.tel" class="form-control" id="exampleFormControlInput1" placeholder="Telephone">
                                </div>
                            </form>
                        </div>
                        <div class="modal-footer">
                            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                            <button type="button" v-on:click="addContact()" data-bs-dismiss="modal" class="btn btn-primary">Confirme</button>
                        </div>
                        </div>
                    </div>
                </div>
                <!-- delete modal  -->
                <div class="modal fade" id="delete" data-bs-backdrop="static" data-bs-keyboard="false" tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
                    <div class="modal-dialog">
                        <div class="modal-content">
                        <div class="modal-header">
                            <h5 class="modal-title" id="staticBackdropLabel">Supprimer cette contact </h5>
                            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                        </div>

                        <div class="modal-footer">
                            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                            <button type="button" v-on:click="deleteContact(contact.id)" data-bs-dismiss="modal" class="btn btn-danger">Confirme</button>
                        </div>
                        </div>
                    </div>
                </div>
                     <!-- EDIT modal  -->
                    <div class="modal fade" id="editContact" data-bs-backdrop="static" data-bs-keyboard="false" tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
                    <div class="modal-dialog">
                        <div class="modal-content">
                        <div class="modal-header">
                            <h5 class="modal-title" id="staticBackdropLabel">Modifier</h5>
                            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                        </div>
                        <div class="modal-body">
                            <form>
                                <div class="mb-3">
                                    <label class="form-label">Nom</label>
                                    <input type="text"  v-model="contact.name" class="form-control" id="exampleFormControlInput1" placeholder="Nom">
                                </div>
                                <div class="mb-3">
                                    <label class="form-label">Telephone</label>
                                    <input type="text" v-model="contact.tel" class="form-control" id="exampleFormControlInput1" placeholder="Telephone">
                                </div>
                            </form>
                        </div>
                        <div class="modal-footer">
                            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                            <button type="button" v-on:click="updateContact(contact.id)" data-bs-dismiss="modal" class="btn btn-primary">Confirme</button>
                        </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data(){
            return{
                contacts : [],
                contact : {
                    name:'',
                    tel :'',
                },
                contact_search:''
            };
        },
        methods:{
            addContact(){
                axios.post('/api/contacts',this.contact)
                .then((res)=>{
                    this.getContacts();
                })
                .catch(err=>console.log(err));
                this.contact = {name:'',tel:''};
            },
            updateContact($id){
                axios.put('/api/contacts/'+$id,this.contact)
                .then((res)=>{
                    this.getContacts();
                })
                .catch(err=>console.log(err));
                this.contact = {name:'',tel:''};
            },
            getContact($id){
                axios.get('/api/contacts/'+$id)
                .then((res)=>{
                    this.contact=res.data;
                    console.log(this.contact);
                })
                .catch(err=>console.log(err));
            },
            deleteContact($id){
                axios.delete('/api/contacts/'+$id)
                .then((res)=>{
                    this.getContacts();
                })
                .catch(err=>console.log(err));
                this.contact = {name:'',tel:''};
            },
            getContacts:function(){
                axios.get('/api/contacts')
                .then((res) => {
                this.contacts=res.data;
                })
                .catch(err=>console.log(err));
            },

        },
        mounted() {
           this.getContacts()
        },
        computed:{
        filtreContacts(){
        return this.contacts.filter((contact)=>{
            return contact.name.match(this.contact_search);
            // if($data.length != this.contacts.length ){
            //     $data=contact.tel.match(this.contact_search);
            // }
            // return $data;
        });
    }
  }
    }
</script>

<style lang="scss">
    .search-box{
    margin: 10px;
    input{
      background: #fff;
      padding: 10px 20px;
      font-size: 1rem;
        font-weight: bold;
      &:focus{
        background: #dff9fb;
        box-shadow: none;
      }
      &::placeholder{
        font-size: 1rem;
        font-weight: bold;
      }
    }
  }
</style>
