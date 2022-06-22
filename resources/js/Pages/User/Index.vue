<template>
    <AppLayout title="Dashboard">
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                USER
            </h2>
        </template>

       

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="bg-white overflow-hidden shadow-xl sm:rounded-lg">
                    <div class="p-6 sm:px-20 bg-white border-b border-gray-200">
                    
                    <inertia-link :href="route('user.create')" class="btn btn-success inline-block">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 float-left" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                            <path stroke-linecap="round" stroke-linejoin="round" d="M12 9v3m0 0v3m0-3h3m-3 0H9m12 0a9 9 0 11-18 0 9 9 0 0118 0z" />
                        </svg>
                        Create user
                    </inertia-link>

                    <form method="GET" @submit.prevent="submit" class="flex mt-2">
                        
                        <inertia-link href="#" class="btn btn-primary mr-2" @click="form.name=''">
                            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                                <path stroke-linecap="round" stroke-linejoin="round" d="M3 4a1 1 0 011-1h16a1 1 0 011 1v2.586a1 1 0 01-.293.707l-6.414 6.414a1 1 0 00-.293.707V17l-4 4v-6.586a1 1 0 00-.293-.707L3.293 7.293A1 1 0 013 6.586V4z" />
                            </svg>
                        </inertia-link>

                        <jet-input v-model="form.name" class="w-full" placeholder="Find by name or email" type="text" />
                        <button type="submit" class="btn btn-primary ml-2">Search</button>
                    </form>

                    <table class="table-auto w-full">
                        <thead>
                            <tr>
                                <th class="p-3">ID</th>
                                <th class="p-3">Nombre</th>
                                <th class="p-3">Email</th>
                                <th class="p-3">Actions</th>
                                
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="u in users.data" :key="u.id">
                                <td class="p-3 border">{{ u.id }}</td>
                                <td class="p-3 border">{{ u.name }}</td>
                                <td class="p-3 border">{{ u.email }}</td>
                                <td class="p-3 border">
                                    <!-- PASSING USER SELECTED TO SHOW -->
                                    <inertia-link :href="route('user.show', {customer: u})" class="btn btn-primary mr-2 inline-block">
                                        <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                                            <path stroke-linecap="round" stroke-linejoin="round" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z" />
                                            <path stroke-linecap="round" stroke-linejoin="round" d="M2.458 12C3.732 7.943 7.523 5 12 5c4.478 0 8.268 2.943 9.542 7-1.274 4.057-5.064 7-9.542 7-4.477 0-8.268-2.943-9.542-7z" />
                                        </svg>
                                    </inertia-link>
                                    <inertia-link :href="route('user.edit', {customer: u})" class="btn btn-primary mr-2 inline-block">
                                        <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                                            <path stroke-linecap="round" stroke-linejoin="round" d="M15.232 5.232l3.536 3.536m-2.036-5.036a2.5 2.5 0 113.536 3.536L6.5 21.036H3v-3.572L16.732 3.732z" />
                                        </svg>
                                    </inertia-link>
                                    <!-- WHEN DELETING WE HAVE TO INDICATE THE METHOD USED BECAUSE THE ROUTE IS THE SAME AS SHOW -->
                                    <!-- <inertia-link method="DELETE" :href="route('user.destroy', {customer: u})" as="button">
                                        Delete
                                    </inertia-link> -->

                                    <button @click="modalOpen=true;selectedUser=u" class="btn btn-danger align-top">
                                        <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                                            <path stroke-linecap="round" stroke-linejoin="round" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
                                        </svg>
                                    </button>
                                </td>
                            </tr>
                        </tbody>
                    </table>

                    <paginator class="mt-3" :paginator="users" />

                </div>
                </div>
            </div>
        </div>

        <jet-dialog-modal :show="modalOpen">
            <template v-slot:title>
                <h1>DELETE</h1>
            </template>
            <template v-slot:content>
                <p v-if="selectedUser">Are you sure you want to delete <strong>{{ selectedUser.name }}</strong> ?</p>
            </template>
            <template v-slot:footer>
                <jet-button @click="deleteUser()" class="bg-red-500 hover:bg-red-800">
                    Delete
                </jet-button>
                <jet-button @click="modalOpen=false">
                    Close
                </jet-button>
            </template>
        </jet-dialog-modal>

    </AppLayout>
</template>

<script>
import AppLayout from '@/Layouts/AppLayout'
import {Inertia} from "@inertiajs/inertia"

import JetDialogModal from "@/Jetstream/DialogModal"
import JetButton from "@/Jetstream/Button"
import JetInput from '@/Jetstream/Input'

import Paginator from "@/Components/Paginator"

export default {

    data(){
        return{
            modalOpen: false,
            selectedUser:Object,
            form: {
                name: this.name,
            }
        }
    },

    components: {
        AppLayout,
        JetDialogModal,
        JetButton,
        Paginator,
        JetInput,
    },

    props: ['users','name'],

    methods: {
        deleteUser: function(){
            // if(confirm("Are you sure you want to delete " + data.name +"?")){
            //     Inertia.delete(route('user.destroy', {customer:data}));
            // } else {
            //     return
            // }

            Inertia.delete(route('user.destroy', {customer:this.selectedUser}))
            this.modalOpen=false
        },

        submit: function(){
            Inertia.get(route("user.index", this.form))
        },
    },

}
</script>

<style>

</style>