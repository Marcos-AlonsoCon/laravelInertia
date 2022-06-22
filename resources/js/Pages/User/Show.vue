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
                            <jet-label :value="'Name:' + customer.name"/>
                            <jet-label :value="'Email:' + customer.email"/>
                            
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
// JET COMPONENTS
import JetLabel from '@/Jetstream/Label'

export default {

    components: {
        AppLayout,
        JetLabel,
    },

    // ERRORS COME FROM PROPS WHEN AN ERROR OCCURS
    props: ["customer", ],

    

}
</script>

<style>

</style>