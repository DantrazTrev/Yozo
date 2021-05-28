<template>
  <v-container fluid grid-list-md>
    <v-layout row>
      <v-flex v-for="list in lists" :key="list.id" xs4>
        <list :list="list" />
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
import list from '../components/list'
export default {
  components: {
    list,
  },
  data() {
    return {
      lists: [
        { id: 1, title: 'To Start' },
        { id: 2, title: 'In Progress' },
        { id: 3, title: 'Done' },
      ],
    }
  },
}
</script>
