<template>
    <div class="overflow-hidden overflow-x-auto p-6 bg-white border-gray-200">
        <div class="min-w-full align-middle">
            <div class="mb-4">
                <select v-model="selectedCategory" class="block mt-1 w-full sm:w-1/4 rounded-md sh">
                    <option value="" selected>&#45;&#45; Filter by category &#45;&#45;</option>
                    <option v-for="category in categories" :value="category.id" :key="category.id">
                        {{ category.name }}
                    </option>
                </select>
                <!-- <input v-model="search_global" type="text" placeholder="Search..."
                    class="inline-block mt-1 w-full rounded-md shadow-sm border-gray-300 focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50"> -->
            </div>

            <table class="min-w-full divide-y divide-gray-200 border">
                <thead>
                    <tr>
                        <th class="px-6 py-3 bg-gray-50 text-left">
                            <input v-model="search_id" type="text"
                                class="inline-block w-full rounded-md shadow-sm border-gray-300 focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50"
                                placeholder="Filter by ID">
                        </th>
                        <th class="px-6 py-3 bg-gray-50 text-left">
                            <input v-model="search_title" type="text"
                                class="inline-block w-full rounded-md shadow-sm border-gray-300 focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50"
                                placeholder="Filter by Title">
                        </th>
                        <th class="px-6 py-3 bg-gray-50 text-left">
                            <select v-model="search_category"
                                class="inline-block w-full rounded-md shadow-sm border-gray-300 focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50">
                                <option value="" selected>-- all categories --</option>
                                <option v-for="category in categories" :value="category.id">
                                    {{ category.name }}
                                </option>
                            </select>
                        </th>
                        <th class="px-6 py-3 bg-gray-50 text-left">
                            <input v-model="search_content" type="text"
                                class="inline-block w-full rounded-md shadow-sm border-gray-300 focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50"
                                placeholder="Filter by Content">
                        </th>
                        <th class="px-6 py-3 bg-gray-50 text-left"></th>
                        <th class="px-6 py-3 bg-gray-50 text-left"></th>
                    </tr>
                    <tr>
                        <th class="px-6 py-3 bg-gray-50 text-left">
                            <div class="flex flex-row items-center justify-between cursor-pointer"
                                @click="updateOrdering('id')">
                                <div class="leading-4 font-medium text-gray-500 uppercase tracking-wider"
                                    :class="{ 'font-bold text-blue-600': orderColumn === 'id' }">
                                    ID
                                </div>
                                <div class="select-none">
                                    <span :class="{
                                        'text-blue-600': orderDirection === 'asc' && orderColumn === 'id',
                                        'hidden': orderDirection !== '' && orderDirection !== 'asc' && orderColumn === 'id',
                                    }">&uarr;</span>
                                    <span :class="{
                                        'text-blue-600': orderDirection === 'desc' && orderColumn === 'id',
                                        'hidden': orderDirection !== '' && orderDirection !== 'desc' && orderColumn === 'id',
                                    }">&darr;</span>
                                </div>
                            </div>
                        </th>
                        <th class="px-6 py-3 bg-gray-50 text-left">
                            <div class="flex flex-row items-center justify-between cursor-pointer"
                                @click="updateOrdering('title')">
                                <div class="leading-4 font-medium text-gray-500 uppercase tracking-wider"
                                    :class="{ 'font-bold text-blue-600': orderColumn === 'title' }">
                                    Title
                                </div>
                                <div class="select-none">
                                    <span :class="{
                                        'text-blue-600': orderDirection === 'asc' && orderColumn === 'title',
                                        'hidden': orderDirection !== '' && orderDirection !== 'asc' && orderColumn === 'title',
                                    }">&uarr;</span>
                                    <span :class="{
                                        'text-blue-600': orderDirection === 'desc' && orderColumn === 'title',
                                        'hidden': orderDirection !== '' && orderDirection !== 'desc' && orderColumn === 'title',
                                    }">&darr;</span>
                                </div>
                            </div>
                        </th>
                        <th class="px-6 py-3 bg-gray-50 text-left">
                            <span
                                class="text-xs leading-4 font-medium text-gray-500 uppercase tracking-wider">Category</span>
                        </th>
                        <th class="px-6 py-3 bg-gray-50 text-left">
                            <span
                                class="text-xs leading-4 font-medium text-gray-500 uppercase tracking-wider">Content</span>
                        </th>
                        <th class="px-6 py-3 bg-gray-50 text-left">
                            <div class="flex flex-row items-center justify-between cursor-pointer"
                                @click="updateOrdering('created_at')">
                                <div class="leading-4 font-medium text-gray-500 uppercase tracking-wider"
                                    :class="{ 'font-bold text-blue-600': orderColumn === 'created_at' }">
                                    Created at
                                </div>
                                <div class="select-none">
                                    <span :class="{
                                        'text-blue-600': orderDirection === 'asc' && orderColumn === 'created_at',
                                        'hidden': orderDirection !== '' && orderDirection !== 'asc' && orderColumn === 'created_at',
                                    }">&uarr;</span>
                                    <span :class="{
                                        'text-blue-600': orderDirection === 'desc' && orderColumn === 'created_at',
                                        'hidden': orderDirection !== '' && orderDirection !== 'desc' && orderColumn === 'created_at',
                                    }">&darr;</span>
                                </div>
                            </div>
                        </th>
                        <th class="px-6 py-3 bg-gray-50 text-left">
                            <span
                                class="text-xs leading-4 font-medium text-gray-500 uppercase tracking-wider">Actions</span>
                        </th>
                    </tr>
                </thead>
                <tbody class=" bg-white divide-y divide-gray-200">
                    <tr v-for="post in posts.data" :key="post.id">
                        <td class="px-6 py-4 whitespace-no-wrap text-sm leading-5 text-gray-900">
                            {{ post.id }}
                        </td>
                        <td class="px-6 py-4 whitespace-no-wrap text-sm leading-5 text-gray-900">
                            {{ post.category }}
                        </td>
                        <td class="px-6 py-4 whitespace-no-wrap text-sm leading-5 text-gray-900">
                            {{ post.title }}
                        </td>
                        <td class="px-6 py-4 whitespace-no-wrap text-sm leading-5 text-gray-900">
                            {{ post.content }}
                        </td>
                        <td class="px-6 py-4 whitespace-no-wrap text-sm leading-5 text-gray-900">
                            {{ post.created_at }}
                        </td>
                        <td class="px-6 py-4 whitespace-no-wrap text-sm leading-5 text-gray-900">
                            <router-link :to="{ name: 'posts.edit', params: { id: post.id } }">Edit </router-link>
                            <a href="#" @click.prevent="deletePost(post.id)" class="ml-2">Delete</a>
                        </td>
                    </tr>
                </tbody>
            </table>
            <TailwindPagination :data="posts"
                @pagination-change-page="page => getPosts(page, selectedCategory, orderColumn, orderDirection, search_category, search_title)"
                class="mt-4" />
        </div>
    </div>
</template>

<script setup>
import { onMounted, ref, watch } from "vue";
import { TailwindPagination } from "laravel-vue-pagination";
import usePosts from "@/composables/posts";
import useCategories from "@/composables/categories";

const { categories, getCategories } = useCategories();
const { posts, getPosts, swal, deletePost } = usePosts();
const selectedCategory = ref('');
const orderColumn = ref('created_at');
const orderDirection = ref('desc');
const search_category = ref('')
const search_id = ref('')
const search_title = ref('')
const search_content = ref('')
const search_global = ref('')

onMounted(() => {
    getPosts();
    getCategories();
});

watch(search_category, (current, previous) => {
    getPosts(
        1,
        current,
        search_id.value,
        search_title.value,
        search_content.value
    )
})

const updateOrdering = (column) => {
    orderColumn.value = column;
    orderDirection.value = (orderDirection.value === 'asc') ? 'desc' : 'asc';
    getPosts(
        search_category.value,
        search_id.value,
        search_title.value,
        search_content.value,
        orderColumn.value,
        orderDirection.value,
    );
}
watch(search_id, (current, previous) => {
    getPosts(
        1,
        search_category.value,
        current,
        search_title.value,
        search_id.value,
        search_content.value
    )
})
watch(search_title, (current, previous) => {
    getPosts(
        1,
        search_category.value,
        search_id.value,
        current,
        search_content.value
    )
})

watch(search_content, (current, previous) => {
    getPosts(
        1,
        search_category.value,
        search_id.value,
        search_title.value,
        current
    )
})

watch(search_content, (current, previous) => {
    getPosts(
        1,
        search_category.value,
        search_id.value,
        search_title.value,
        current
    )
})
watch(search_category, (current, previous) => {
    getPosts(
        1,
        current,
        search_id.value,
        search_title.value,
        search_content.value
    )
})

</script>
