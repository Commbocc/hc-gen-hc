<template>
  <v-container
    id="genhc"
    fill-height
    :style="`--categories-length: ${categories.length}`"
  >
    <!--  -->
    <v-row id="gen-hc-categories" align="center" justify="center">
      <!--  -->
      <v-col cols="12" sm="6">
        <v-expansion-panels v-model="activeCategoryIndex" dark>
          <v-expansion-panel
            v-for="(category, index) in categories"
            :key="index"
            :style="`--categories-index: ${index}`"
          >
            <v-expansion-panel-header class="text-h6 py-5">
              {{ category.title }}
            </v-expansion-panel-header>

            <v-card
              v-if="$vuetify.breakpoint.xs && activeCategory === category"
              tile
              class="mb-5"
              color="rgba(0,0,0,20%)"
            >
              <v-img
                :src="activeCategory.image"
                :alt="activeCategory.title"
              ></v-img>
              <v-card-text class="text-body-1 white--text">
                <div v-text="activeCategory.body"></div>
              </v-card-text>
            </v-card>

            <v-expansion-panel-content>
              <!-- links -->

              <div>
                <v-btn
                  v-for="(link, i) in category.links"
                  :key="i"
                  large
                  block
                  outlined
                  rounded
                  color="white"
                  class="mb-3 text-truncate"
                  :target="link.target"
                  :href="link.url"
                  :title="link.title"
                >
                  {{ link.title }}
                </v-btn>

                <!--  -->
                <v-btn
                  :href="category.url"
                  target="_top"
                  large
                  block
                  outlined
                  rounded
                  color="white"
                  class="mb-3"
                  :title="`'${category.title}' Articles`"
                >
                  "{{ category.title }}" Articles
                </v-btn>
              </div>
            </v-expansion-panel-content>
          </v-expansion-panel>
        </v-expansion-panels>
      </v-col>

      <!--  -->
      <v-col v-if="!$vuetify.breakpoint.xs && activeCategory" cols="12" sm="6">
        <!-- -->
        <v-card v-if="activeCategory">
          <a :href="activeCategory.url" target="_top">
            <v-img :src="activeCategory.image" :alt="activeCategory.title">
            </v-img>
          </a>
          <v-card-title>{{ activeCategory.title }}</v-card-title>
          <v-card-text class="text-body-1">
            <div v-text="activeCategory.body"></div>
          </v-card-text>
          <v-card-actions>
            <v-spacer />
            <v-btn
              :href="activeCategory.url"
              target="_top"
              rounded
              dark
              :style="`--categories-index: ${activeCategoryIndex}`"
              class="px-5"
            >
              {{ btnText }}
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  components: {},

  async asyncData({ $content }) {
    const { categories, btnText } = await $content(`data`).fetch()

    return { categories, btnText }
  },

  data: () => ({
    activeCategoryIndex: null,
  }),

  computed: {
    activeCategory() {
      if (this.activeCategoryIndex === null) return null
      return this.categories[this.activeCategoryIndex]
    },
  },
}
</script>
