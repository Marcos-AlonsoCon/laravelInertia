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
                            <jet-button class="mt-4" type="submit">Send</jet-button>
                            <inertia-link class="mt-4 float-right cursor-pointer"  @click="cleanForm" preserve-scroll>Clean</inertia-link>
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
                name: "",
                email: "",
                password: "",
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
    props: ["errors",],

    methods: {
        submit(){
            Inertia.post(route('user.store'), this.form);
        },

        cleanForm: function(){
            this.form.name = "";
            this.form.email = "";
            this.form.password = "";
        }
    },

}
</script>

<style>

</style>