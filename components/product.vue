<script setup>
import { ref } from "vue";
import axios from "axios";
import Swal from "sweetalert2";
const route = useRoute();
const router = useRouter();

const form = ref({
  id: null,
  title: "",
  seo: "",
  description: "",
  stockCode: "",
  barcode: "",
  associative: "",
  tax: "",
});

onMounted(async () => {
  if (route.params.id) {
    await axios
      .get(`http://localhost:3050/api/v1/product/${route.params.id}?id=true`)
      .then((res) => {
        console.log(res.data.row);
        form.value = res.data.row;
      })
      .catch((error) => {});
  }
});

const submit = () => {
  if (form.value.id) {
    axios
      .put("http://localhost:3050/api/v1/product", form.value)
      .then((res) => {
        Swal.fire({
          title: "Good update!",
          text: "You clicked the button!",
          icon: "success",
        });
        form.value.id = res.data.data.id;
        router.replace(`/dashboard/product/${res.data.data.id}`);
      })
      .catch((error) => {});
  } else {
    axios
      .post("http://localhost:3050/api/v1/product", form.value)
      .then((res) => {
        Swal.fire({
          title: "Good insert!",
          text: "You clicked the button!",
          icon: "success",
        });
        form.value.id = res.data.data.id;
        router.replace(`/dashboard/product/${res.data.data.id}`);
      })
      .catch((error) => {});
  }
};
</script>
<template>
  <section class="bg-white dark:bg-gray-900">
    <div class="py-8 lg:py-16 px-4 mx-auto max-w-screen-md">
      <h2
        class="mb-4 text-4xl tracking-tight font-extrabold text-center text-gray-900 dark:text-white"
      >
        New Product
      </h2>
      <p
        class="mb-8 lg:mb-16 font-light text-center text-gray-500 dark:text-gray-400 sm:text-xl"
      >
        New Product
      </p>
      <form @submit.prevent="submit" class="space-y-8">
        <div>
          <label
            for="title"
            class="block mb-2 text-sm font-medium text-gray-900 dark:text-gray-300"
            >Product Title</label
          >
          <input
            type="text"
            id="title"
            class="shadow-sm bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-primary-500 focus:border-primary-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-primary-500 dark:focus:border-primary-500 dark:shadow-sm-light"
            placeholder="Product title"
            required
            v-model="form.title"
          />
        </div>
        <div>
          <label
            for="seo"
            class="block mb-2 text-sm font-medium text-gray-900 dark:text-gray-300"
            >Seo (Slug)</label
          >
          <input
            type="text"
            id="seo"
            class="block p-3 w-full text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 shadow-sm focus:ring-primary-500 focus:border-primary-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-primary-500 dark:focus:border-primary-500 dark:shadow-sm-light"
            placeholder="Product seo"
            required
            v-model="form.seo"
          />
        </div>
        <div class="sm:col-span-2">
          <label
            for="description"
            class="block mb-2 text-sm font-medium text-gray-900 dark:text-gray-400"
            >Description</label
          >
          <textarea
            id="description"
            rows="6"
            class="block p-2.5 w-full text-sm text-gray-900 bg-gray-50 rounded-lg shadow-sm border border-gray-300 focus:ring-primary-500 focus:border-primary-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-primary-500 dark:focus:border-primary-500"
            placeholder="Product description"
            v-model="form.description"
          ></textarea>
        </div>
        <div
          class="mt-6 grid grid-cols-1 gap-x-6 gap-y-10 sm:grid-cols-3 lg:grid-cols-4 xl:gap-x-8"
        >
          <div>
            <label
              for="stock-code"
              class="block mb-2 text-sm font-medium text-gray-900 dark:text-gray-300"
              >Stock Code</label
            >
            <input
              type="text"
              id="stock-code"
              class="shadow-sm bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-primary-500 focus:border-primary-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-primary-500 dark:focus:border-primary-500 dark:shadow-sm-light"
              placeholder="Stock Code"
              required
              v-model="form.stockCode"
            />
          </div>
          <div>
            <label
              for="barcode"
              class="block mb-2 text-sm font-medium text-gray-900 dark:text-gray-300"
              >Barcode</label
            >
            <input
              type="text"
              id="barcode"
              class="shadow-sm bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-primary-500 focus:border-primary-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-primary-500 dark:focus:border-primary-500 dark:shadow-sm-light"
              placeholder="Barcode"
              required
              v-model="form.barcode"
            />
          </div>
          <div>
            <label
              for="associative"
              class="block mb-2 text-sm font-medium text-gray-900 dark:text-gray-300"
              >Associative</label
            >
            <input
              type="text"
              id="associative"
              class="shadow-sm bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-primary-500 focus:border-primary-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-primary-500 dark:focus:border-primary-500 dark:shadow-sm-light"
              placeholder="Associative"
              required
              v-model="form.associative"
            />
          </div>
          <div>
            <label
              for="tax"
              class="block mb-2 text-sm font-medium text-gray-900 dark:text-gray-300"
              >Tax</label
            >
            <input
              type="number"
              id="tax"
              class="shadow-sm bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-primary-500 focus:border-primary-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-primary-500 dark:focus:border-primary-500 dark:shadow-sm-light"
              placeholder="Tax"
              required
              v-model="form.tax"
            />
          </div>
        </div>
        <button
          type="submit"
          class="py-3 px-5 text-sm font-medium text-center text-white border border-gray-300 rounded-lg bg-primary-700 sm:w-fit hover:bg-primary-800 focus:ring-4 focus:outline-none focus:ring-primary-300 dark:bg-primary-600 dark:hover:bg-primary-700 dark:focus:ring-primary-800"
        >
          Save Product
        </button>
      </form>
    </div>
  </section>
</template>
