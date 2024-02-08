<script setup>
import { ref, watch } from "vue";
import axios from "axios";
import { StreamBarcodeReader } from "vue-barcode-reader";
import Swal from "sweetalert2";
import PriceModal from "~/components/modal/price-modal.vue";

const router = useRouter();

const search = ref(null);
const products = ref([]);
const selectedProduct = ref({});
const modalOpen = ref(false);

watch(
  () => search.value,
  async (newValue, oldValue) => {
    await axios
      .get(`http://localhost:3050/api/v1/product/search/${newValue}`)
      .then((res) => {
        console.log(res.data);
        products.value = res.data.rows;
      })
      .catch((error) => {});
  }
);

onMounted(async () => {
  search.value = "";
});

const onDecode = (text) => {
  search.value = text;
};

const onLoaded = () => {
  console.log("loaded");
};

const customSwal = Swal.mixin({
  customClass: {
    confirmButton: "btn btn-accent mx-1",
    cancelButton: "btn btn-danger mx-1",
  },
  buttonsStyling: false,
});

const handleConfirmDisable = async (item) => {
  customSwal
    .fire({
      icon: "warning",
      title: "Is Disable?",
      showCancelButton: true,
      confirmButtonText: `<svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
            <path stroke-linecap="round" stroke-linejoin="round" d="M6.633 10.25c.806 0 1.533-.446 2.031-1.08a9.041 9.041 0 0 1 2.861-2.4c.723-.384 1.35-.956 1.653-1.715a4.498 4.498 0 0 0 .322-1.672V2.75a.75.75 0 0 1 .75-.75 2.25 2.25 0 0 1 2.25 2.25c0 1.152-.26 2.243-.723 3.218-.266.558.107 1.282.725 1.282m0 0h3.126c1.026 0 1.945.694 2.054 1.715.045.422.068.85.068 1.285a11.95 11.95 0 0 1-2.649 7.521c-.388.482-.987.729-1.605.729H13.48c-.483 0-.964-.078-1.423-.23l-3.114-1.04a4.501 4.501 0 0 0-1.423-.23H5.904m10.598-9.75H14.25M5.904 18.5c.083.205.173.405.27.602.197.4-.078.898-.523.898h-.908c-.889 0-1.713-.518-1.972-1.368a12 12 0 0 1-.521-3.507c0-1.553.295-3.036.831-4.398C3.387 9.953 4.167 9.5 5 9.5h1.053c.472 0 .745.556.5.96a8.958 8.958 0 0 0-1.302 4.665c0 1.194.232 2.333.654 3.375Z" />
            </svg>`,
    })
    .then(async (result) => {
      if (result.isConfirmed) {
        await axios
          .get(
            `http://localhost:3050/api/v1/product/confirm/disable/${item.id}`
          )
          .then((res) => {
            console.log(res.data.status);
            if (res.data.status) item.confirm = false;
          })
          .catch((error) => {});
      }
    });
};

const handleConfirmEnable = async (item) => {
  customSwal
    .fire({
      icon: "warning",
      title: "Is Enable?",
      showCancelButton: true,
      confirmButtonText: `<svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
            <path stroke-linecap="round" stroke-linejoin="round" d="M6.633 10.25c.806 0 1.533-.446 2.031-1.08a9.041 9.041 0 0 1 2.861-2.4c.723-.384 1.35-.956 1.653-1.715a4.498 4.498 0 0 0 .322-1.672V2.75a.75.75 0 0 1 .75-.75 2.25 2.25 0 0 1 2.25 2.25c0 1.152-.26 2.243-.723 3.218-.266.558.107 1.282.725 1.282m0 0h3.126c1.026 0 1.945.694 2.054 1.715.045.422.068.85.068 1.285a11.95 11.95 0 0 1-2.649 7.521c-.388.482-.987.729-1.605.729H13.48c-.483 0-.964-.078-1.423-.23l-3.114-1.04a4.501 4.501 0 0 0-1.423-.23H5.904m10.598-9.75H14.25M5.904 18.5c.083.205.173.405.27.602.197.4-.078.898-.523.898h-.908c-.889 0-1.713-.518-1.972-1.368a12 12 0 0 1-.521-3.507c0-1.553.295-3.036.831-4.398C3.387 9.953 4.167 9.5 5 9.5h1.053c.472 0 .745.556.5.96a8.958 8.958 0 0 0-1.302 4.665c0 1.194.232 2.333.654 3.375Z" />
            </svg>`,
    })
    .then(async (result) => {
      if (result.isConfirmed) {
        await axios
          .get(`http://localhost:3050/api/v1/product/confirm/enable/${item.id}`)
          .then((res) => {
            console.log(res.data.status);
            if (res.data.status) item.confirm = true;
          })
          .catch((error) => {});
      }
    });
};

const handleDelete = (item) => {
  customSwal
    .fire({
      icon: "warning",
      title: "Is Delete?",
      showCancelButton: true,
      confirmButtonText: `<svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
            <path stroke-linecap="round" stroke-linejoin="round" d="M6.633 10.25c.806 0 1.533-.446 2.031-1.08a9.041 9.041 0 0 1 2.861-2.4c.723-.384 1.35-.956 1.653-1.715a4.498 4.498 0 0 0 .322-1.672V2.75a.75.75 0 0 1 .75-.75 2.25 2.25 0 0 1 2.25 2.25c0 1.152-.26 2.243-.723 3.218-.266.558.107 1.282.725 1.282m0 0h3.126c1.026 0 1.945.694 2.054 1.715.045.422.068.85.068 1.285a11.95 11.95 0 0 1-2.649 7.521c-.388.482-.987.729-1.605.729H13.48c-.483 0-.964-.078-1.423-.23l-3.114-1.04a4.501 4.501 0 0 0-1.423-.23H5.904m10.598-9.75H14.25M5.904 18.5c.083.205.173.405.27.602.197.4-.078.898-.523.898h-.908c-.889 0-1.713-.518-1.972-1.368a12 12 0 0 1-.521-3.507c0-1.553.295-3.036.831-4.398C3.387 9.953 4.167 9.5 5 9.5h1.053c.472 0 .745.556.5.96a8.958 8.958 0 0 0-1.302 4.665c0 1.194.232 2.333.654 3.375Z" />
            </svg>`,
    })
    .then(async (result) => {
      if (result.isConfirmed) {
        await axios
          .get(`http://localhost:3050/api/v1/product/delete/${item.id}`)
          .then((res) => {
            console.log(res.data.status);
            item.deletedAt = res.data.now;
          })
          .catch((error) => {});
      }
    });
};

const handleEdit = (item) => {
  router.push(`/dashboard/product/${item.id}`);
};

const handlePriceModal = (item) => {
  modalOpen.value = true;
  selectedProduct.value = item;
};

const handleModalOpen = (item) => {
  modalOpen.value = item;
};
</script>
<template>
  <section class="bg-white dark:bg-gray-900">
    <div class="py-8 lg:py-16 px-4 mx-auto max-w-screen-md">
      <h2
        class="mb-4 text-4xl tracking-tight font-extrabold text-center text-gray-900 dark:text-white"
      >
        Products
      </h2>
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
                  />
                </svg>
              </span>
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
          <tr
            v-for="(item, index) in products.filter(
              (k) => k.deletedAt === null
            )"
          >
            <th>{{ item.id }}</th>
            <td>{{ item.title }}</td>
            <td>{{ item.seo }}</td>
            <td>{{ item.description }}</td>
            <td>{{ item.stockCode }}</td>
            <td>{{ item.barcode }}</td>
            <td>{{ item.associative }}</td>
            <td>{{ item.tax }}</td>
            <td class="inline-flex">
              <span class="cursor-pointer" @click="handlePriceModal(item)">
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
                    d="M2.25 18.75a60.07 60.07 0 0 1 15.797 2.101c.727.198 1.453-.342 1.453-1.096V18.75M3.75 4.5v.75A.75.75 0 0 1 3 6h-.75m0 0v-.375c0-.621.504-1.125 1.125-1.125H20.25M2.25 6v9m18-10.5v.75c0 .414.336.75.75.75h.75m-1.5-1.5h.375c.621 0 1.125.504 1.125 1.125v9.75c0 .621-.504 1.125-1.125 1.125h-.375m1.5-1.5H21a.75.75 0 0 0-.75.75v.75m0 0H3.75m0 0h-.375a1.125 1.125 0 0 1-1.125-1.125V15m1.5 1.5v-.75A.75.75 0 0 0 3 15h-.75M15 10.5a3 3 0 1 1-6 0 3 3 0 0 1 6 0Zm3 0h.008v.008H18V10.5Zm-12 0h.008v.008H6V10.5Z"
                  />
                </svg>
              </span>
              <span class="cursor-pointer" @click="handleEdit(item)">
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
                @click="handleConfirmDisable(item)"
              >
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
                    d="m4.5 12.75 6 6 9-13.5"
                  />
                </svg>
              </span>
              <span
                v-if="!item.confirm"
                class="cursor-pointer"
                @click="handleConfirmEnable(item)"
              >
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
                    d="M18.364 18.364A9 9 0 0 0 5.636 5.636m12.728 12.728A9 9 0 0 1 5.636 5.636m12.728 12.728L5.636 5.636"
                  />
                </svg>
              </span>
              <span class="cursor-pointer" @click="handleDelete(item)">
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
                    d="m14.74 9-.346 9m-4.788 0L9.26 9m9.968-3.21c.342.052.682.107 1.022.166m-1.022-.165L18.16 19.673a2.25 2.25 0 0 1-2.244 2.077H8.084a2.25 2.25 0 0 1-2.244-2.077L4.772 5.79m14.456 0a48.108 48.108 0 0 0-3.478-.397m-12 .562c.34-.059.68-.114 1.022-.165m0 0a48.11 48.11 0 0 1 3.478-.397m7.5 0v-.916c0-1.18-.91-2.164-2.09-2.201a51.964 51.964 0 0 0-3.32 0c-1.18.037-2.09 1.022-2.09 2.201v.916m7.5 0a48.667 48.667 0 0 0-7.5 0"
                  />
                </svg>
              </span>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </section>
  <PriceModal
    :modalOpen="modalOpen"
    :item="selectedProduct"
    @modalClose="handleModalOpen"
  />
</template>
