<script setup>
import { computed, ref } from "vue";
import { Link,usePage,useForm, router } from "@inertiajs/vue3";
import { createToaster } from "@meforma/vue-toaster";
const toaster = createToaster({ });

const page = usePage();
const searchValue = ref();
const searchField = ref(["name", "price", "unit", "mobile"]);
const headers = [
    { text: "No", value: "customId" },
    { text: "Name", value: "name" },
    { text: "Unit", value: "unit" },
    { text: "Mobile", value: "mobile"},
    { text: "Action", value: "action" },
];

const itemsWithCustomId = computed(() => {
    return items.value.map((item, index) => ({
        ...item,
        customId: items.value.length - index,
    }))
})

const items = ref(page.props.customers);

const deleteCustomer = (id) => {
    if (confirm("Are you sure you want to delete this customer?")) {
        router.get(`/delete-customer?id=${id}`);
    }
};

if(page.props.flash.status===true){
    toaster.success(page.props.flash.message);

}else if(page.props.flash.status===false){
    toaster.error(page.props.flash.message);
}
</script>

<template>
    <div class="p-4 bg-[#f8f8f8]">
        <h1 class="text-2xl font-bold mb-4">Customer / Party List</h1>
        <input
            v-model="searchValue"
            type="text"
            class="mb-2 px-2 py-1 w-[300px] border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500 outline-none transition-all"
            placeholder="Search...."
        />
        <div class="mt-4 mb-4">
            <Link :href="`/customer-save-page?id=${0}`" class="bg-orange-600 hover:bg-orange-700 text-white py-2 px-4 rounded">Create new party</Link>
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
            <template #item-action="{ id }">
                <Link
                    :href="`/customer-save-page?id=${id}`"
                    class="bg-green-500 hover:bg-green-700 text-white py-2 px-4 rounded"
                >
                    Edit
                </Link>
                <button
                    @click="deleteCustomer(id)"
                    class="bg-red-500 ml-1 text-white py-2 px-4 rounded"
                >
                    Delete
                </button>
            </template>

        </EasyDataTable>

    </div>
</template>

<style scoped></style>
