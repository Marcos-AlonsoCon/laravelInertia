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
                    

                        <form @submit.prevent="submit">
                            <div class="mt-4">
                                <jet-label value="Name:"/>
                                <jet-input class="mt-1 block w-full" type="text" v-model="form.name"/>
                                    <jet-input-error :message="errors.name"></jet-input-error>
                            </div>
                            <div class="mt-4">
                                <jet-label value="Email:"/>
                                <jet-input class="mt-1 block w-full" type="text" v-model="form.email"/>
                                    <jet-input-error :message="errors.email"></jet-input-error>
                            </div>
                            <div class="mt-4">
                                <jet-label value="Password:"/>
                                <jet-input class="mt-1 block w-full" type="password" v-model="form.password"/>
                                    <jet-input-error :message="errors.password"></jet-input-error>
                            </div>
                            <div v-if="!customer.profile_photo_path"  class="mt-4">
                                <jet-label value="Avatar:"/>
                                <jet-input class="mt-1 block w-full" type="file" @input="form.avatar = $event.target.files[0]"/>
                                    <jet-input-error :message="errors.avatar"></jet-input-error>
                            </div>
                            <jet-button class="mt-4" type="submit">Send</jet-button>

                             <hr class="my-4">

                            <div v-if="customer.profile_photo_url">
                                <img :src="customer.profile_photo_url" :alt="customer.name" class="h-20 w-20 border object-cover m-auto block rounded-full">
                                <inertia-link v-if="customer.profile_photo_path" class="block text-center text-red-600 text-xs font-bold m-0"  @click="deleteAvatar">Delete</inertia-link>
                            </div>

                        </form>

                    </div>
                </div>
            </div>
        </div>
    </AppLayout>
</template>

<script>

// WHEN USING FILES FROM resources/js MUST USE @ TO INDICATE THAT PATH
import AppLayout from '@/Layouts/AppLayout'
import {Inertia} from '@inertiajs/inertia'

// JET COMPONENTS
import JetInputError from '@/Jetstream/InputError'
import JetInput from '@/Jetstream/Input'
import JetLabel from '@/Jetstream/Label'
import JetButton from '@/Jetstream/Button'

export default {

    data(){
        return {
            form: {
                name: this.customer.name,
                email: this.customer.email,
                password: "",
                avatar: "",
            }
        }
    },

    components: {
        AppLayout,
        JetInputError,
        JetInput,
        JetLabel,
        JetButton
    },

    // ERRORS COME FROM PROPS WHEN AN ERROR OCCURS
    props: ["customer", "errors"],

    methods: {
        submit(){
            // Inertia.put(route('user.update', {'customer': this.customer}), this.form);
            // MUST USE post METHOD WHEN UPLOADING FILES, EVEN HERE WE ARE UPDATING
            Inertia.post(route('user.update', {'customer': this.customer}),
            {
                _method: 'put',
                name: this.form.name,
                email: this.form.email,
                password: this.form.password,
                avatar: this.form.avatar,
            });
        },

        deleteAvatar: function(){
            Inertia.delete(route('user.deleteAvatar',{customer: this.customer}))
        },
    },

}
</script>

<style>

</style>