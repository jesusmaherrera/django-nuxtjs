<template>
  <div class="box column is-10 is-offset-1">
    <form>
      <input id="question-id" v-model="valueLocal.id" type="hidden" />
      <b-field label="Question">
        <b-input
          v-model="valueLocal.text"
          placeholder="Enter the question text"
          type="text"
          icon="tag"
          required
        />
      </b-field>
      <div class="columns">
        <b-field class="column" label="Start date">
          <b-datetimepicker
            v-model="valueLocal.start_date"
            icon="calendar-arrow-right"
            required
          />
        </b-field>
        <b-field class="column" label="End date">
          <b-datetimepicker
            v-model="valueLocal.end_date"
            icon="calendar-arrow-left"
            required
          />
        </b-field>
      </div>
      <h1 class="subtitle">Choices</h1>
      <form-choice v-model="choice" @saveChoice="saveChoice" />
      <hr />
      <list-choices
        v-model="valueLocal.choices"
        @removeChoice="removeChoice"
        @loadChoice="loadChoice"
      />

      <div>
        <b-button type="is-info" icon-left="check" @click="save">
          Save
        </b-button>
        <b-button type="is-dark" icon-left="redo" @click="reset">
          Cancel
        </b-button>
      </div>
    </form>
  </div>
</template>

<script>
import FormChoice from './FormChoice'
import ListChoices from './ListChoices'
export default {
  components: { FormChoice, ListChoices },
  props: {
    value: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      choice: {},
    }
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
    save() {
      this.$emit('save', this.value)
    },
    reset() {
      this.$emit('reset')
    },
    saveChoice(choice) {
      if (choice.edit) {
        const objIndex = this.valueLocal.choices.findIndex(
          (obj) => obj.id === choice.id
        )
        this.valueLocal.choices[objIndex].text = choice.text
      } else {
        this.valueLocal.choices.push(choice)
      }
      this.choice = {}
    },
    removeChoice(choice) {
      const objIndex = this.valueLocal.choices.findIndex(
        (obj) => obj.id === choice.id
      )
      this.valueLocal.choices.splice(objIndex, 1)
    },
    loadChoice(choice) {
      this.choice = {
        id: choice.id,
        text: choice.text,
        edit: true,
      }
    },
  },
}
</script>
