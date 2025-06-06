<script setup>
import { computed, ref } from "vue";
import { Link, usePage, router } from "@inertiajs/vue3";
import { createToaster } from "@meforma/vue-toaster";
const toaster = createToaster({ });

const page = usePage();
const searchValue = ref();
const searchField = ref(["decription"]);
const headers = [
    { text: "No", value: "customId" },
    { text: "Description", value: "decription", sortable: true },
    { text: "Size", value: "size", sortable: true },
    { text: "Weight", value: "weight", sortable: true },
    { text: "Rate", value: "rate", sortable: true },
    { text: "Action", value: "action" },
];

const items = ref(page.props.products);

const itemsWithCustomId = computed(() => {
    return items.value.map((item, index) => ({
        ...item,
        customId: items.value.length - index,
    }))
})

const deleteProduct = (id) => {
    if (confirm("Are you sure you want to delete this product?")) {
        router.get(`/delete-product?id=${id}`);
    }
};

if (page.props.flash.status === true) {
  toaster.success(page.props.flash.message);
} else if (page.props.flash.status === false) {
  toaster.error(page.props.flash.message);
}
</script>

<template>
    <div class="p-4 bg-[#f8f8f8] min-h-screen">
        <h1 class="text-2xl font-bold mb-4">Product List</h1>

        <input
            v-model="searchValue"
            type="text"
            class="mb-2 px-3 py-2 w-72 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500 transition-all"
            placeholder="Search...."
        />

        <div class="mt-4 mb-4">
            <Link
                :href="`/product-save-page?id=${0}`"
                class="inline-block px-4 py-2 text-white bg-orange-600 rounded-lg hover:bg-orange-700 transition"
            >
                Add new Product
            </Link>
        </div>

        <EasyDataTable
            buttons-pagination
            alternating
            :headers="headers"
            :items="itemsWithCustomId"
            :search-value="searchValue"
            :search-field="searchField"
            :rows-per-page="20"
        >
            <template #item-decription="{ decription }">
                <div class="break-words">
                    <span v-if="decription.length > 80">{{ decription.substring(0, 80) }}...</span>
                    <span v-else>{{ decription }}</span>
                </div>
            </template>

            <template #item-action="{ id }">
                <Link
                    :href="`/product-save-page?id=${id}`"
                    class="inline-flex items-center bg-green-500 hover:bg-green-700 text-white py-1 px-2 rounded transition"
                    title="Edit Product"
                >
                    <span class="material-icons text-base">edit</span>
                </Link>

                <button
                    @click="deleteProduct(id)"
                    class="inline-flex items-center ml-1 bg-red-500 hover:bg-red-700 text-white py-1 px-2 rounded transition"
                    title="Delete Product"
                >
                    <span class="material-icons text-base">delete</span>
                </button>
            </template>
        </EasyDataTable>
    </div>
</template>

<style scoped>
.break-words {
    word-break: break-word;
    white-space: normal;
}
</style>
