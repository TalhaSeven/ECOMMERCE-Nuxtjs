<script setup>
import { ref, watch } from "vue";
import axios from "axios";
import { StreamBarcodeReader } from "vue-barcode-reader";

const search = ref("");
const products = ref([]);

watch(
  () => search.value,
  async (newValue, oldValue) => {
    console.log(`${newValue} ${oldValue}`);
    await axios
      .get(`http://localhost:3050/api/v1/product/search/${newValue}`)
      .then((res) => {
        console.log(res.data);
        products.value = res.data.rows;
      })
      .catch((error) => {});
  }
);

const onDecode = (text) => {
  search.value = text;
};

const onLoaded = () => {
  console.log("loaded");
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
      <div>
        <label
          for="title"
          class="block mb-2 text-sm font-medium text-gray-900 dark:text-gray-300"
          >Product Title</label
        >
        <div>
          <div class="relative">
            <input
              type="text"
              class="w-full pl-4 pr-4 py-2 border rounded-lg"
              placeholder="search"
              v-model="search"
            />
            <div
              class="absolute inset-y-0 right-5 pl-3 flex items-center pointer-events-none z-50"
            >
              <span role="button" class="cursor-pointer">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke-width="1.5"
                  stroke="currentColor"
                  class="w-6 h-6"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    d="M3.75 4.875c0-.621.504-1.125 1.125-1.125h4.5c.621 0 1.125.504 1.125 1.125v4.5c0 .621-.504 1.125-1.125 1.125h-4.5A1.125 1.125 0 0 1 3.75 9.375v-4.5ZM3.75 14.625c0-.621.504-1.125 1.125-1.125h4.5c.621 0 1.125.504 1.125 1.125v4.5c0 .621-.504 1.125-1.125 1.125h-4.5a1.125 1.125 0 0 1-1.125-1.125v-4.5ZM13.5 4.875c0-.621.504-1.125 1.125-1.125h4.5c.621 0 1.125.504 1.125 1.125v4.5c0 .621-.504 1.125-1.125 1.125h-4.5A1.125 1.125 0 0 1 13.5 9.375v-4.5Z"
                  />
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    d="M6.75 6.75h.75v.75h-.75v-.75ZM6.75 16.5h.75v.75h-.75v-.75ZM16.5 6.75h.75v.75h-.75v-.75ZM13.5 13.5h.75v.75h-.75v-.75ZM13.5 19.5h.75v.75h-.75v-.75ZM19.5 13.5h.75v.75h-.75v-.75ZM19.5 19.5h.75v.75h-.75v-.75ZM16.5 16.5h.75v.75h-.75v-.75Z"
                  /></svg
              ></span>
            </div>
          </div>
        </div>
      </div>
    </div>

    <StreamBarcodeReader
      @decode="onDecode"
      @loaded="onLoaded"
      class="mb-3"
      style="width: 400px"
      v-if="search === 'abc'"
    ></StreamBarcodeReader>
    <div class="overflow-x-auto">
      <table class="table table-xs">
        <thead>
          <tr>
            <th>Id</th>
            <th>Title</th>
            <th>Seo</th>
            <th>Description</th>
            <th>Stock Code</th>
            <th>Barcode</th>
            <th>Associative</th>
            <th>Tax</th>
            <th></th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(item, index) in products">
            <th>{{ item.id }}</th>
            <td>{{ item.title }}</td>
            <td>{{ item.seo }}</td>
            <td>{{ item.description }}</td>
            <td>{{ item.stockCode }}</td>
            <td>{{ item.barcode }}</td>
            <td>{{ item.associative }}</td>
            <td>{{ item.tax }}</td>
            <td class="inline-flex gap-1">
              <span>
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke-width="1.5"
                  stroke="currentColor"
                  class="w-5 h-5"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    d="m16.862 4.487 1.687-1.688a1.875 1.875 0 1 1 2.652 2.652L10.582 16.07a4.5 4.5 0 0 1-1.897 1.13L6 18l.8-2.685a4.5 4.5 0 0 1 1.13-1.897l8.932-8.931Zm0 0L19.5 7.125M18 14v4.75A2.25 2.25 0 0 1 15.75 21H5.25A2.25 2.25 0 0 1 3 18.75V8.25A2.25 2.25 0 0 1 5.25 6H10"
                  />
                </svg>
              </span>
              <span
                v-if="item.confirm"
                class="cursor-pointer"
                @click="handleConfirmHide(item)"
                ><svg
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke-width="1.5"
                  stroke="currentColor"
                  class="w-5 h-5"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    d="m4.5 12.75 6 6 9-13.5"
                  />
                </svg>
              </span>
              <span
                v-if="!item.confirm"
                class="cursor-pointer"
                @click="handleConfirmShow(item)"
                ><svg
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke-width="1.5"
                  stroke="currentColor"
                  class="w-5 h-5"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    d="M18.364 18.364A9 9 0 0 0 5.636 5.636m12.728 12.728A9 9 0 0 1 5.636 5.636m12.728 12.728L5.636 5.636"
                  />
                </svg>
              </span>
              <span>
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke-width="1.5"
                  stroke="currentColor"
                  class="w-5 h-5"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    d="m20.25 7.5-.625 10.632a2.25 2.25 0 0 1-2.247 2.118H6.622a2.25 2.25 0 0 1-2.247-2.118L3.75 7.5m6 4.125 2.25 2.25m0 0 2.25 2.25M12 13.875l2.25-2.25M12 13.875l-2.25 2.25M3.375 7.5h17.25c.621 0 1.125-.504 1.125-1.125v-1.5c0-.621-.504-1.125-1.125-1.125H3.375c-.621 0-1.125.504-1.125 1.125v1.5c0 .621.504 1.125 1.125 1.125Z"
                  />
                </svg>
              </span>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </section>
</template>
