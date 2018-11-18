<template>
  <v-layout row wrap>
    <v-flex xs12 sm6 md4 lg3 v-for="headline in headlines"
            v-bind:key="headline.title"
    >

      <!-- Card -->
      <v-hover>
        <v-card
          height="100%" slot-scope="{ hover }"
          :class="`elevation-${hover ? 12 : 2}`"
          :href="headline.url" target="_blank"
        >

          <!-- Title -->
          <v-img
            height="150px"
            class="white--text"
            :src="headline.urlToImage ? headline.urlToImage : `https://via.placeholder.com/150/CCCCCC/CCCCCC`"
            gradient="to top, rgba(0,0,0,.5), rgba(0,0,0,0)"
          >
            <v-container fill-height>
              <v-layout>
                <v-flex layout align-end pb-0>
                  <span class="subheading">
                    {{ headline.title.substr(0, 48) + '...' }}
                  </span>
                </v-flex>
              </v-layout>
            </v-container>
          </v-img>

          <!-- Description -->
          <v-card-title style="padding-bottom: 0;">
            <span class="caption grey--text">
              {{ new Intl.DateTimeFormat('fr-CH').format(new Date(headline.publishedAt)) }}
            </span>
            <span>
              {{ (headline.content === null) ? headline.content : headline.content.substr(0, 88) + '...' }}
            </span>
          </v-card-title>

          <!-- Button -->
          <v-card-actions>
            <!-- <v-btn icon>
              <v-icon class="grey--text">share</v-icon>
            </v-btn> -->
            <v-flex text-xs-right pa-0>
              <v-btn flat color="primary">
                Read more
              </v-btn>
            </v-flex>
          </v-card-actions>

        </v-card>
      </v-hover>

    </v-flex>
  </v-layout>
</template>

<script>
export default {
  props: ['headlines'],
}
</script>
