<script setup>
import { router, useForm, usePage, Link } from "@inertiajs/vue3";
import { reactive } from "vue";
import { createToaster } from "@meforma/vue-toaster";
const toaster = createToaster({ });

const page = usePage();
const status = reactive({
    title: "Create Product",
    buttonTitle: "Create Product",
});
let params = new URLSearchParams(window.location.search);
let id = params.get("id");

const form = useForm({
    description: "",
    size: "",
    rate: "",
    weight: "",
    id: id,
});

let URL = "/create-product";
if (id !== 0 && page.props.product !== null) {
    status.title = "Update Product";
    status.buttonTitle = "Update Product";
    URL = "/update-product";
    form.description = page.props.product.decription;
    form.rate = page.props.product.rate;
    form.size = page.props.product.size;
    form.weight = page.props.product.weight;
}

const submitForm = () => {
    if (form.description === '') {
        toaster.error('Name is required');
    } else {
        form.post(URL, {
            preserveScroll: true,
            onSuccess: () => {
                if (page.props.flash.status === true) {
                    toaster.success(page.props.flash.message);
                    setTimeout(() => {
                        router.get("/product-page");
                    }, 500);
                } else {
                    toaster.error(page.props.flash.message);
                }
            },
        });
    }
};
</script>

<template>
    <div class="container mx-auto py-8">
        <form
            @submit.prevent="submitForm"
            class="w-full max-w-sm mx-auto bg-white p-8 rounded-md shadow-md"
        >
            <div class="flex justify-end mb-4">
                <Link
                    href="/product-page"
                    class="px-4 py-2 bg-green-600 hover:bg-green-700 text-white rounded text-sm transition"
                >
                    Back
                </Link>
            </div>

            <h1 class="text-2xl font-bold mb-6">{{ status.title }}</h1>

            <div class="mb-4">
                <label
                    class="block text-gray-700 text-sm font-bold mb-2"
                    for="name"
                >
                    Description
                </label>
                <input
                    v-model="form.description"
                    class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:border-indigo-500"
                    type="text"
                    id="name"
                    name="name"
                />
                <input type="hidden" v-model="form.id" name="id" />
            </div>

            <div class="mb-4">
                <label
                    class="block text-gray-700 text-sm font-bold mb-2"
                    for="price"
                >
                    Rate
                </label>
                <input
                    v-model="form.rate"
                    class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:border-indigo-500"
                    type="text"
                    id="price"
                    name="price"
                />
            </div>

            <div class="mb-4">
                <label
                    class="block text-gray-700 text-sm font-bold mb-2"
                    for="size"
                >
                    Size
                </label>
                <input
                    v-model="form.size"
                    class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:border-indigo-500"
                    type="text"
                    id="size"
                    name="size"
                />
            </div>

            <div class="mb-4">
                <label
                    class="block text-gray-700 text-sm font-bold mb-2"
                    for="weight"
                >
                    Weight
                </label>
                <input
                    v-model="form.weight"
                    class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:border-indigo-500"
                    type="text"
                    id="weight"
                    name="weight"
                />
            </div>

            <button
                type="submit"
                class="w-full bg-orange-600 hover:bg-orange-700 text-white font-medium py-2.5 rounded-lg transition-colors"
            >
                {{ status.buttonTitle }}
            </button>
        </form>
    </div>
</template>

<style scoped></style>
