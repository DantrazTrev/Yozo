<template>
  <v-card>
    <v-card-title>
      <div class="headline">{{ list.title }}</div>
      <v-chip color="grey" class="number" pill x-small>{{
        cards.length
      }}</v-chip>
    </v-card-title>
    <v-container>
      <v-layout column>
        <draggable
          v-model="cards"
          :options="draggableOptions"
          style="min-height: 10px"
        >
          <v-flex v-for="card in cards" :key="card.id" class="draggable-item">
            <card :card="card" @del="del(card)" />
          </v-flex>
        </draggable>
      </v-layout>
      <v-layout>
        <v-flex>
          <v-btn elevation="2" center @click="dialog = true">Add</v-btn>
        </v-flex>
      </v-layout>
    </v-container>
    <v-dialog v-model="dialog" max-width="669px">
      <v-card>
        <v-card-title>New Task</v-card-title>
        <v-card-text>
          <v-text-field v-model="nyt.value" label="Task Name"></v-text-field>
          <v-textarea v-model="nyt.description" label="About"></v-textarea>
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn text color="primary" @click="submit"> Submit </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-card>
</template>

<script>
import draggable from 'vuedraggable'
import card from '~/components/card'
export default {
  components: {
    draggable,
    card,
  },
  props: {
    list: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      name: this.list.title,
      dialog: false,
      cards: [],
      nyt: {
        id: Date.now(),
        value: '',
        description: '',
        status: this.list.title,
      },
    }
  },
  created() {
    this.ger()
  },
  methods: {
    submit() {
      this.$data.cards.push({
        value: this.$data.nyt.value,
        desciption: this.$data.nyt.description,
        status: this.$data.nyt.status,
      })
      this.$data.dialog = false
    },
    ger() {
      fetch('https://my-json-server.typicode.com/DantrazTrev/trelldem/cards')
        .then((response) => response.json())
        .then((json) => {
          this.$data.cards = json.filter((j) => {
            if (j.status === this.$data.name) return 1
            else return 0
          })
        })
    },
    del(card) {
      const car = this.$data.cards.filter((j) => {
        if (j.id === card.id) return 0
        else return 1
      })
      this.$data.cards = car
    },
  },
  computed: {
    draggableOptions() {
      return {
        group: {
          name: 'cards',
        },
        draggable: '.draggable-item',
        ghostClass: 'ghost',
      }
    },
  },
}
</script>

<style scoped>
.draggable-item {
  cursor: pointer;
}
.ghost {
  opacity: 0.4;
}
.number {
  font-size: small;
  opacity: 0.6;
  margin: 50px;
}
</style>
