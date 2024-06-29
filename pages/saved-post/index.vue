<template>
  <div class="ml-[400px] grid grid-cols-4 gap-4">
    <div class="col-span-2 w-full">
      <div class="border-2 p-4 rounded-lg h-[800px] w-[720px]">
        <div>
          <span class="text-2xl font-bold">Tin đã yêu thích</span>
        </div>
        <br />
        <div v-if="favoritePosts.length > 0">
          <PostCard v-for="post in favoritePosts" :key="post.id" :post="post" />
        </div>
        <div class="text-bold text-lg flex items-center justify-center" v-else>
          <span class="text-red-500"
            >Bạn chưa thêm tin nào vào danh sách yêu thích!</span
          >
          <i class="fa fa-heart text-red-500 text-3xl ml-2"></i>
        </div>
      </div>
    </div>
    <div class="col-span-1 h-1/2">
      <Categories title="Danh mục cho thuê" :list="mappedRoomTypes" />
      <br />
      <RelatedPost :posts="latestPosts" type="recent" />
    </div>
  </div>
</template>

<script>
import PostCard from "../../components/PostCard/index.vue";
import Categories from "../../components/Categories/index.vue";
import RelatedPost from "../../components/RelatedPost/index.vue";
import { mapActions, mapState } from "vuex";
import { jwtDecode } from "jwt-decode";

export default {
  components: {
    PostCard,
  },
  data() {
    return {
      userId: null,
      mappedRoomTypes: [],
    };
  },
  computed: {
    ...mapState({
      favoritePosts: (state) => state.modules["favorite"].favoritePosts,
      roomTypes: (state) => state.modules["post"].roomTypes,
      latestPosts: (state) => state.modules["post"].latestPosts,
    }),
  },
  async created() {
    await Promise.all([this.fetchRoomTypes(), this.fetchLatestPosts()]);
    this.mappedRoomTypes = this.roomTypes.map((roomType) => {
      return {
        ...roomType,
        name: roomType.room_type_name,
      };
    });
    if (process.browser) {
      this.accessToken = localStorage.getItem("accessToken");
      if (this.accessToken) {
        const decoded = jwtDecode(this.accessToken);
        this.userId = decoded.user.user_id;
        await this.fetchFavoriteByUser({ user_id: this.userId });
      }
    }
  },
  methods: {
    ...mapActions({
      fetchFavoriteByUser: "modules/favorite/fetchFavoriteByUser",
      fetchRoomTypes: "modules/post/fetchRoomTypes",
      fetchLatestPosts: "modules/post/fetchLatestPosts",
    }),
  },
};
</script>
