<template>
  <form class="columns">
    <input id="question-id" v-model="valueLocal.id" type="hidden" />
    <b-field grouped>
      <b-input
        v-model="valueLocal.text"
        placeholder="Enter the choice text"
        type="text"
        icon="tag"
        required
        expanded
      ></b-input>
      <p class="control">
        <b-button type="is-info" icon-left="check" @click="saveChoice">
          Save
        </b-button>
      </p>
    </b-field>
  </form>
</template>

<script>
export default {
  props: {
    value: {
      type: Object,
      required: true,
    },
  },
  computed: {
    valueLocal: {
      get() {
        return this.value
      },
      set(value) {
        this.$emit('update:value', value)
      },
    },
  },
  methods: {
    saveChoice() {
      this.$emit('saveChoice', this.value)
    },
  },
}
</script>
