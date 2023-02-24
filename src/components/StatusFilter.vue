<script>
export default {
  name: 'StatusFilter',
  props: {
    status: String,
  },
  emits: ['update'],
  methods: {
    setStatus(newStatus) {
      this.$emit('update', newStatus);
    }
  }
}
</script>

<template>
  <nav class="filter">
    <a
      href="#/"
      class="filter__link"
      :class="{ selected: status === 'all' }"
      @click="setStatus('all')"
    >
      All
    </a>

    <a
      href="#/active"
      class="filter__link"
      :class="{ selected: status === 'active' }"
      @click="setStatus('active')"
    >
      Active
    </a>

    <a
      href="#/completed"
      class="filter__link"
      :class="{ selected: status === 'completed' }"
      @click="setStatus('completed')"
    >
      Completed
    </a>
  </nav>
</template>
