<template>
  <div>
    <content-with-heading>
      <template #heading-left>
        <p class="title is-4" v-text="$t('page.radio.title')" />
        <p
          class="heading"
          v-text="$t('page.radio.count', { count: tracks.total })"
        />
      </template>
      <template #content>
        <list-tracks :items="tracks" />
      </template>
    </content-with-heading>
  </div>
</template>

<script>
import ContentWithHeading from '@/templates/ContentWithHeading.vue'
import { GroupedList } from '@/lib/GroupedList'
import ListTracks from '@/components/ListTracks.vue'
import webapi from '@/webapi'

const dataObject = {
  load(to) {
    return webapi.library_radio_streams()
  },

  set(vm, response) {
    vm.tracks = new GroupedList(response.data.tracks)
  }
}

export default {
  name: 'PageRadioStreams',
  components: { ContentWithHeading, ListTracks },

  beforeRouteEnter(to, from, next) {
    dataObject.load(to).then((response) => {
      next((vm) => dataObject.set(vm, response))
    })
  },

  data() {
    return {
      tracks: new GroupedList()
    }
  }
}
</script>

<style></style>
