<template>
    <form @submit.prevent="storePost(post)">
        <!-- Title -->
        <div>
            <label for="post-title" class="block text-sm font-medium text-gray-700">
                Title
            </label>
            <input id="post-title" type="text" v-model="post.title"
                class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50" />
            <div class="text-red-600 mt-1">
                <div v-for="message in validationErrors?.title">
                    {{ message }}
                </div>
            </div>
        </div>
        <!-- Content -->
        <div class="mt-4">
            <label for="post-content" class="block text-sm font-medium text-gray-700">
                Content
            </label>
            <textarea id="post-content" v-model="post.content"
                class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50"></textarea>
            <div class="text-red-600 mt-1">
                <div v-for="message in validationErrors?.content">
                    {{ message }}
                </div>
            </div>
        </div>
        <!-- Category -->
        <div class="mt-4">
            <label for="post-category" class="block text-sm font-medium text-gray-700">
                Category
            </label>
            <select id="post-category" v-model="post.category_id"
                class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50">
                <option>-- Choose category --</option>
                <option v-for="category in categories" :value="category.id">{{ category.name }}</option>
            </select>
            <div class="text-red-600 mt-1">
                <div v-for="message in validationErrors?.category_id">
                    {{ message }}
                </div>
            </div>
        </div>
        <!-- Thumbnail -->
        <div class="mt-4">
            <label for="thumbnail" class="block font-medium text-sm text-gray-700">
                Thumbnail
            </label>
            <input @change="post.thumbnail = $event.target.files[0]" type="file" id="thumbnail" />
            <div class="text-red-600 mt-1">
                <div v-for="message in validationErrors?.thumbnail">
                    {{ message }}
                </div>
            </div>
        </div>

        <!-- Buttons -->
        <div class="mt-4">
            <button :disabled="isLoading"
                class="inline-flex items-center rounded-md bg-indigo-600 px-3 py-2 text-sm uppercase text-white">
                <span v-show="isLoading"
                    class="inline-block animate-spin w-4 h-4 mr-2 border-t-2 border-b-2 border-indigo-500"></span>
                <span v-if="isLoading">Processing...</span>
                <span v-else>Save</span>
            </button>
        </div>
    </form>
</template>

<script setup>
import { onMounted, reactive } from 'vue';
import useCategories from "@/composables/categories";
import usePosts from '@/composables/posts'

const post = reactive({
    title: '',
    content: '',
    category_id: '',
    thumbnail: '',
});

const { categories, getCategories } = useCategories();
const { storePost, validationErrors, isLoading } = usePosts();

onMounted(() => { getCategories() });
</script>
