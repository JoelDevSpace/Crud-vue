<script setup lang="ts">
import InputError from '@/components/InputError.vue';
import { Button } from '@/components/ui/button';
import { Input } from '@/components/ui/input';
import { Label } from '@/components/ui/label';
import AppLayout from '@/layouts/AppLayout.vue';
import { type BreadcrumbItem } from '@/types';
import { Head, Link, useForm } from '@inertiajs/vue3';

const breadcrumbs: BreadcrumbItem[] = [
    {
        title: 'Post create',
        href: '/posts/create',
    },
];

const form = useForm({
    title: '',
    body: '',
});

function submit() {
    form.post(route('posts.store'), {
        preserveScroll: true,
        onSuccess: () => {
            form.reset();
        },
    });
}
</script>

<template>
    <Head title="Post create" />

    <AppLayout :breadcrumbs="breadcrumbs">
        <div class="flex h-full flex-1 flex-col gap-4 rounded-xl p-4">
            <div>
                <Link
                    href="/posts"
                    class="rounded-lg bg-blue-700 px-3 py-2 text-xs font-medium text-white hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 focus:outline-none dark:bg-blue-600"
                >
                    back
                </Link>
            </div>
            <form @submit.prevent="submit">
                <div class="grid gap-2">
                    <Label for="title">Title</Label>
                    <Input id="title" type="text" class="mt-1 block w-full" v-model="form.title" placeholder="Title" />
                    <InputError class="mt-2" :message="form.errors.title" />
                </div>
                <div class="mt-3 grid gap-2">
                    <Label for="body">Body</Label>
                    <textarea
                        id="body"
                        type="text"
                        class="mt-1 block h-15 w-full min-w-0 rounded-md border border-input bg-transparent px-3 py-1 text-base shadow-xs transition-[color,box-shadow] outline-none selection:bg-primary selection:text-primary-foreground file:inline-flex file:h-7 file:border-0 file:bg-transparent file:text-sm file:font-medium placeholder:text-muted-foreground disabled:pointer-events-none disabled:cursor-not-allowed disabled:opacity-50 md:text-sm dark:bg-input/30"
                        v-model="form.body"
                        placeholder="body"
                    ></textarea>
                    <InputError class="mt-2" :message="form.errors.body" />
                </div>

                <div class="mt-3 flex items-center gap-4">
                    <Button>Submit</Button>
                </div>
            </form>
        </div>
    </AppLayout>
</template>
