<template>
  <div>
    <table class="uk-table uk-table-divider">
      <thead>
        <tr v-if="datatype === 'album'">
          <th></th>
          <th class="uk-text-primary">Album</th>
        </tr>
        <tr v-else-if="datatype === 'track'">
          <th class="uk-text-primary">Track</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(data, key) in dataList" v-bind:key="key">
          <td v-if="data.image">
            <img
              class="uk-border-circle cursor-pointer"
              width="150"
              height="150"
              :src="endPointUrl + data.image.url"
              @click="navigateTo(data)"
            />
          </td>
          <td v-if="data.title">
            <span class="uk-link" @click="navigateTo(data)">{{
              data.title
            }}</span>
          </td>
          <td v-if="data.name">
            <span class="uk-link" @click="navigateTo(data)">{{
              data.name
            }}</span>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<script>
import gql from 'graphql-tag'
export default {
  data() {
    return {}
  },
  props: ['datatype'],
  apollo: {
    dataList: {
      query() {
        if (this.datatype === 'track') {
          return gql`
            query getFromArtist($name: String!) {
              tracks(where: { artists: { name: $name } }) {
                title
                artists {
                  name
                  jp_name
                }
              }
            }
          `
        } else if (this.datatype === 'album') {
          return gql`
            query getFromArtist($name: String!) {
              albums(where: { artists: { name: $name } }) {
                name
                image {
                  url
                }
                artists {
                  name
                  jp_name
                }
              }
            }
          `
        }
      },
      update: (data) => data.tracks || data.albums,
      variables() {
        return {
          name: this.$route.params.artist,
        }
      },
      skip() {
        return !this.datatype
      },
    },
  },
  methods: {
    navigateTo(data) {
      let url = '/' + this.$route.params.artist

      if (this.datatype === 'track') url += '/tracks/' + data.title
      if (this.datatype === 'album') url += '/albums/' + data.name

      this.$router.push({
        path: url,
      })
    },
  },
}
</script>
