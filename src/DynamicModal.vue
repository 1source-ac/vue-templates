<template>
  <form>
    <div v-for="(field, index) in fields" :key="index">
      <label :for="field.name">{{ field.label }}</label>
      <component :is="field.type" :name="field.name" v-model="formData[field.name]" :options="field.options"></component>
    </div>
    <button type="submit">Submit</button>
  </form>
</template>

<script>
export default {
  props: {
    fields: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      formData: {},
    };
  },
  components: {
    InputField: {
      props: {
        name: {
          type: String,
          required: true,
        },
        value: {
          type: String,
          default: "",
        },
      },
      template: `
        <input :type="type" :name="name" :value="value" @input="$emit('input', $event.target.value)">
      `,
    },
    SelectField: {
      props: {
        name: {
          type: String,
          required: true,
        },
        value: {
          type: String,
          default: "",
        },
        options: {
          type: Array,
          default: () => [],
        },
      },
      template: `
        <select :name="name" :value="value" @input="$emit('input', $event.target.value)">
          <option v-for="(option, index) in options" :key="index" :value="option.value">{{ option.label }}</option>
        </select>
      `,
    },
    CheckboxField: {
      props: {
        name: {
          type: String,
          required: true,
        },
        value: {
          type: Boolean,
          default: false,
        },
      },
      template: `
        <input type="checkbox" :name="name" :checked="value" @change="$emit('input', $event.target.checked)">
      `,
    },
  },
};
</script>
