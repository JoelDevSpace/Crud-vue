<script setup lang="ts">
import AppLayout from '@/layouts/AppLayout.vue';
import { type BreadcrumbItem } from '@/types';
import { Head, Link, useForm } from '@inertiajs/vue3';
import { Pencil, Plus, Trash2 } from 'lucide-vue-next';

const breadcrumbs: BreadcrumbItem[] = [
    {
        title: 'Posts',
        href: '/posts',
    },
];

defineProps<{
    posts: Array<{
        id: number;
        title: string;
        body: string;
    }>;
}>();

const form = useForm({
    id: null,
});

function deletePost(id: number) {
    if (confirm('Are you sure you want to delete this post?')) {
        form.delete(route('posts.destroy', id), {
            preserveScroll: true,
            onSuccess: () => {
                // Optionally, you can show a success message or perform other actions
            },
        });
    }
}
</script>

<template>
    <Head title="Posts" />

    <AppLayout :breadcrumbs="breadcrumbs">
        <div class="flex h-full flex-1 flex-col gap-4 rounded-xl p-4">
            <div>
                <Link
                    :href="route('posts.create')"
                    class="inline-flex rounded-lg bg-green-700 px-3 py-2 text-xs font-medium text-white hover:bg-green-800 focus:ring-4 focus:ring-green-300 focus:outline-none dark:bg-green-600"
                >
                    <Plus class="mr-2 h-4 w-4" />Create Post
                </Link>
            </div>

            <div class="overflow-x-auto">
                <table class="w-full text-left text-sm text-gray-500 dark:text-gray-400">
                    <thead class="bg-gray-50 text-xs text-gray-700 uppercase dark:bg-gray-700 dark:text-gray-400">
                        <tr>
                            <th scope="col" class="px-6 py-3">ID</th>
                            <th scope="col" class="px-6 py-3">Title</th>
                            <th scope="col" class="px-6 py-3">Body</th>
                            <th scope="col" class="px-6 py-3">Actions</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="post in posts" :key="post.id" class="odd:bp-white add:dark:bg-gray-900 even:bg-gray-50 even:dark:bg-gray-900">
                            <td class="px-6 py-2 font-medium text-gray-900 dark:text-white">{{ post.id }}</td>
                            <td class="px-6 py-2 text-gray-600 dark:text-gray-300">{{ post.title }}</td>
                            <td class="px-6 py-2 text-gray-600 dark:text-gray-300">{{ post.body }}</td>
                            <td class="px-6 py-2">
                                <Link
                                    :href="route('posts.edit', post.id)"
                                    class="inline-flex rounded-lg bg-blue-700 px-3 py-2 text-xs font-medium text-white hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 focus:outline-none dark:bg-blue-600"
                                >
                                    <Pencil class="mr-2 h-3 w-3" />Edit
                                </Link>
                                <button
                                    @click="deletePost(post.id)"
                                    class="ml-2 inline-flex rounded-lg bg-red-700 px-3 py-2 text-xs font-medium text-white hover:bg-red-800 focus:ring-4 focus:ring-red-300 focus:outline-none dark:bg-red-600"
                                >
                                    <Trash2 class="mr-2 h-3 w-3" />Delete
                                </button>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </AppLayout>
</template>
