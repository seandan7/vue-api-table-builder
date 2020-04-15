<template>
  <div>
    <form v-if="!submitted">
      <div clas="form-group">
        <label for="email">Email</label>
        <input type="email" v-model="email" class="form-control" />
      </div>
      <button
        @click.prevent="process"
        class="btn btn-primary mt-3"
        type="submit"
      >
        Submit
      </button>
    </form>
    <div v-show="submitted" v-cloak>
      Thank you for submitting the form
    </div>
  </div>
</template>

<script>
export default {
  name: "Form",
  methods: {
    process: function() {
      this.submitted = true;
    },
  },
  data: function() {
    return {
      email: "test@test.com",
      submitted: false,
    };
  },
};
</script>
<style>
[v-cloak] {
  display: none;
}
</style>
<!-- VkhjpIiZWLF0DVij5GbHSRPVQFvNLHLZiQj1w6Lr
-->
