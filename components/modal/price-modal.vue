<script setup>
const emit = defineEmits(["modalClose"]);
import Swal from "sweetalert2";
import axios from "axios";

const props = defineProps({
  modalOpen: Boolean,
  item: Object,
});

const form = ref({
  id: null,
  productId: null,
  price: 0,
  discountPrice: 0,
  discountRate: 0,
});

watch(
  () => props.item,
  async (newValue, oldValue) => {
    await axios
      .get(`http://localhost:3050/api/v1/price/${newValue.id}`)
      .then((res) => {
        console.log(res.data.list);
        form.value = res.data.list;
      })
      .catch((error) => {
        form.value = {
          id: null,
          productId: null,
          price: 0,
          discountPrice: 0,
          discountRate: 0,
        };
      });
  }
);

const handleModalClose = () => {
  emit("modalClose", false);
};

const submit = () => {
  form.value.productId = props.item.id;
  console.log(form.value);
  axios
    .post("http://localhost:3050/api/v1/price", form.value)
    .then((res) => {
      Swal.fire({
        title: "Good save!",
        text: "You clicked the button!",
        icon: "success",
      });
      emit("modalClose", false);
    })
    .catch((error) => {});
};

const handleDiscountPriceChanged = () => {
  const price = form.value.price;
  const discountPrice = form.value.discountPrice;
  form.value.discountRate = ((1 - discountPrice / price) * 100).toFixed(2);
};

const handleDiscountRateChanged = () => {
  const price = form.value.price;
  const discountRate = form.value.discountRate;
  form.value.discountPrice = (price - (price / 100) * discountRate).toFixed(2);
};
</script>
<template>
  <dialog
    id="my_modal_1"
    class="modal"
    :class="{ 'modal-open': props.modalOpen }"
  >
    <div class="modal-box">
      <h3 class="font-bold text-lg">{{ item?.title }}</h3>
      <p class="py-4">{{ item?.description }}</p>
      <form @submit.prevent="submit" class="space-y-8">
        <div
          class="mt-6 grid grid-cols-1 gap-x-6 gap-y-10 sm:grid-cols-3 lg:grid-cols-3 xl:gap-x-8"
        >
          <div>
            <label
              for="price"
              class="block mb-2 text-sm font-medium text-gray-900 dark:text-gray-300"
              >Price</label
            >
            <input
              type="number"
              id="price"
              class="shadow-sm bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-primary-500 focus:border-primary-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-primary-500 dark:focus:border-primary-500 dark:shadow-sm-light"
              placeholder="Stock Code"
              required
              v-model="form.price"
            />
          </div>
          <div>
            <label
              for="discountPrice"
              class="block mb-2 text-sm font-medium text-gray-900 dark:text-gray-300"
              >Discount Price</label
            >
            <input
              type="number"
              id="discountPrice"
              class="shadow-sm bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-primary-500 focus:border-primary-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-primary-500 dark:focus:border-primary-500 dark:shadow-sm-light"
              @change="handleDiscountPriceChanged"
              placeholder="Stock Code"
              required
              v-model="form.discountPrice"
            />
          </div>
          <div>
            <label
              for="discountRate"
              class="block mb-2 text-sm font-medium text-gray-900 dark:text-gray-300"
              >Discount Rate</label
            >
            <input
              type="number"
              id="discountRate"
              class="shadow-sm bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-primary-500 focus:border-primary-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-primary-500 dark:focus:border-primary-500 dark:shadow-sm-light"
              @change="handleDiscountRateChanged"
              placeholder="Stock Code"
              required
              v-model="form.discountRate"
            />
          </div>
        </div>
        <div class="modal-action">
          <button
            type="submit"
            class="py-3 px-5 mx-3 text-sm font-medium text-center text-white border border-gray-300 rounded-lg bg-primary-700 sm:w-fit hover:bg-primary-800 focus:ring-4 focus:outline-none focus:ring-primary-300 dark:bg-primary-600 dark:hover:bg-primary-700 dark:focus:ring-primary-800"
          >
            Save Price
          </button>
          <button type="button" class="btn" @click="handleModalClose()">
            Close
          </button>
        </div>
      </form>
    </div>
  </dialog>
</template>
