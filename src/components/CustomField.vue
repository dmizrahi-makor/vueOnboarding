<template>
  <v-text-field
    v-model="email"
    :rules="emailRules"
    :label="label"
    required
    outlined
  ></v-text-field>
</template>

<script scoped>
export default {
  name: "CustomField",
  props: {
    label: {
      type: String,
      required: true,
    },
    type: {
      type: String,
      required: false,
    },
  },
  components: {},
  data: () => {},
  methods: {},
};
</script>
