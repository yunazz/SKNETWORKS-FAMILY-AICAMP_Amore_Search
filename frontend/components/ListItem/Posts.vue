<script setup>
const props = defineProps({
  item: { required: true },
  favorite_type: { type: String },
  is_favorite: {
    type: Boolean,
    required: false,
  },
});
const emit = defineEmits(["showDetail", "notify", "success"]);

function openLink(url) {
  if (isEmpty(url)) return;
  window.open(url);
}

const copyToClipboard = async (text) => {
  try {
    await navigator.clipboard.writeText(text);
    emit("notify", "링크가 복사되었습니다.");

    setTimeout(() => {
      copied.value = false;
    }, 2000);
  } catch (err) {
    console.log(err);
  }
};
</script>
<template>
  <div class="search_card">
    <v-card class="mx-auto" min-height="120">
      <v-card-text class="pb-1">
        <div class="flex align-end justify-between">
          <div class="mb-2">
            <span class="text-primary mr-1 fw-600">
              [{{ enums.post_type[item?.post_type] }}]
            </span>
          </div>
          <div class="mb-3">
            <!-- <v-icon
              icon="mdi-text-box-search-outline"
              variant="text"
              color="primary"
              @click="reveal = true"
            /> -->
            <v-icon
              v-if="
                !isEmpty(item.original_file_url) || !isEmpty(item.source_url)
              "
              class="ml-4"
              icon="mdi-share-variant-outline"
              variant="text"
              color="primary"
              @click="
                copyToClipboard(item.original_file_url || item.source_url)
              "
            />
          </div>
        </div>

        <p class="search_card_title text-clamp-2">{{ item?.title }}</p>

        <div class="text-medium-emphasis text-clamp-4">{{ item?.content }}</div>
      </v-card-text>

      <v-card-actions>
        <div class="flex align-center justify-between w-full ml-2">
          <div>
            <!-- <v-btn
              icon="mdi-text-box-search-outline"
              variant="text"
              color="primary"
              @click="reveal = true"
            />
            <v-btn
              icon="mdi-share-variant-outline"
              variant="text"
              color="primary"
              @click="reveal = true"
            />
            <v-btn
              class="icon--toggle"
              :class="{ active: is_favorite }"
              icon="mdi-star"
              variant="text"
              @click="toggleFavorites(item)"
            /> -->
            <v-chip color="primary" size="small" class="pr-3">
              <div
                v-if="item?.original_file_url"
                class="cur-d"
                :class="{ 'cur-p': !isEmpty(item.original_file_url) }"
                @click="openLink()"
              >
                <v-icon class="mr-1" color="sub" icon="mdi-folder" />
                {{ item.post_ctgry }}
              </div>
              <div
                v-else
                class="cur-d"
                :class="{ 'cur-p': !isEmpty(item.source_url) }"
                @click="openLink(item.source_url)"
              >
                <v-icon class="mr-1" color="sub" icon="mdi-link-variant" />
                {{ item.source_name }}
              </div>
            </v-chip>
          </div>
          <span class="text-gray-03 mr-3 body--s">
            {{ formatDate(item.created_at) }}
          </span>
        </div>
      </v-card-actions>

      <!-- <v-expand-transition>
        <v-card
          v-if="reveal"
          class="position-absolute w-100"
          height="100%"
          style="bottom: 0"
        >
          <v-card-text class="pb-0">
            <p class="text-h4">제목</p>

            <p class="text-medium-emphasis"></p>
          </v-card-text>

          <v-card-actions class="pt-0">
            <v-btn
              color="primary"
              text="닫기"
              variant="text"
              @click="reveal = false"
            ></v-btn>
          </v-card-actions>
        </v-card>
      </v-expand-transition> -->
    </v-card>
  </div>
</template>
<style scoped>
.search_card {
  position: relative;
  background: #ddd;
  margin-bottom: 14px;
}
.search_card_title {
  font-size: 14px;
  font-weight: 500;
  line-height: 1.2;
  margin-bottom: 10px;
}
.text-medium-emphasis {
  line-height: 1.3;
  font-size: 12px;
}
</style>
