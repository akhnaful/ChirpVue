<script setup>
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue';
import Chirp from '@/Components/Chirp.vue';
import InputError from '@/Components/InputError.vue';
import PrimaryButton from '@/Components/PrimaryButton.vue';
import { useForm, Head } from '@inertiajs/vue3';
import Editor from 'primevue/editor';

defineProps(['chirps']);

const form = useForm({
    message: '',
    avatar: null,
});

import { ref } from "vue";

const value = ref('');
</script>

<template>
    <Head title="Chirps" />

    <AuthenticatedLayout>
        <div class="max-w-2xl mx-auto p-4 sm:p-6 lg:p-8">
            <!-- Form untuk mengirim Chirp -->
            <form @submit.prevent="form.post(route('chirps.store'), { onSuccess: () => form.reset() })">
                <!-- Editor -->
                <div>
                    <Editor 
                        name="content" 
                        v-model="form.message" 
                        placeholder="What's on your mind?"
                        class="block bg-white w-full border-gray-300 focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50 rounded-md shadow-sm"
                    />
                </div>
                
                <!-- Input untuk file avatar -->
                <div class="mt-2 block">
                    <input 
                        type="file" 
                        @change="form.avatar = $event.target.files[0]"
                    />
                </div>
                
                <!-- Progress bar upload -->
                <progress v-if="form.progress" :value="form.progress.percentage" max="100">
                    {{ form.progress.percentage }}%
                </progress>

                <!-- Pesan kesalahan -->
                <InputError :message="form.errors.message" class="mt-2" />
                
                <!-- Tombol Kirim -->
                <PrimaryButton class="mt-2">Chirp</PrimaryButton>
            </form>

            <!-- Daftar Chirps -->
            <div class="mt-6 bg-white shadow-sm rounded-lg divide-y">
                <Chirp
                    v-for="chirp in chirps"
                    :key="chirp.id"
                    :chirp="chirp"
                />
            </div>
        </div>
    </AuthenticatedLayout>
</template>
