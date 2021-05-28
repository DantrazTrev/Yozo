<template>
  <v-card @click="dialog = !dialog">
    <v-card-text
      >{{ value }} <v-icon x-small right> mdi-close </v-icon>
    </v-card-text>
    <v-dialog v-model="dialog" max-wdith="690px">
      <v-card>
        <v-card-title>Edit Task</v-card-title>
        <v-card-text>
          <v-text-field v-model="temp.value" label="Task Name"></v-text-field>
          <v-textarea v-model="temp.description" label="About"></v-textarea>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn text color="red" @click="deel"> Delete </v-btn>
          <v-btn text color="primary" @click="submit"> Submit </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-card>
</template>

<script>
export default {
  props: ['card'],
  data() {
    return {
      dialog: false,
      id: this.card.id,
      value: this.card.value,
      description: this.card.description,
      status: this.card.status,
      temp: {
        id: Date.now(),
        value: this.card.value,
        description: this.card.description,
        status: this.card.status,
      },
    }
  },
  methods: {
    submit(e) {
      e.preventDefault()
      this.$data.value = this.$data.temp.value
      this.$data.description = this.$data.temp.description
      this.$data.status = this.$data.temp.status

      this.$data.dialog = false
    },
    deel(e) {
      this.$emit('del')
      this.$data.dialog = false
    },
  },
}
</script>
