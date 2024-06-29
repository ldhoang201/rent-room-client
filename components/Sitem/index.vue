<template>
  <div
    class="w-full flex items-center gap-2 py-2 border-b border-gray-300 cursor-pointer"
    @click="goToPostDetail"
  >
    <img
      :src="image"
      alt="anh"
      class="w-[65px] h-[65px] object-cover flex-none rounded-md"
    />
    <div class="w-full flex-auto flex flex-col justify-between gap-1">
      <h4 class="text-blue-600 text-[14px]">{{ truncatedTitle }}</h4>
      <div class="flex items-center justify-between w-full">
        <span class="text-sm font-medium text-green-500">{{
          formatCurrency(price)
        }}</span>
        <span class="text-sm text-gray-300">{{
          formattedTime(createdAt)
        }}</span>
      </div>
    </div>
  </div>
</template>

<script>
import moment from "moment";
import "moment/locale/vi";

export default {
  props: {
    postId: {
      required: true,
    },
    title: {
      required: true,
    },
    price: {
      required: true,
    },
    image: {
      required: true,
    },
    createdAt: {
      required: true,
    },
  },
  computed: {
    truncatedTitle() {
      return this.title.slice(0, 45) + "...";
    },
  },
  methods: {
    goToPostDetail() {
      this.$router.push(`/${this.postId}`);
    },
    formattedTime(createdAt) {
      return (
        moment(createdAt).fromNow().charAt(0).toUpperCase() +
        moment(createdAt).fromNow().slice(1)
      );
    },
    formatCurrency(amount) {
      const formatter = new Intl.NumberFormat("vi-VN", {
        style: "currency",
        currency: "VND",
      });
      return formatter.format(amount);
    },
  },
};
</script>

<style scoped></style>
