<template>
  <section v-show="assignments.length" class="w-60">
    <div class="flex justify-between items-start">
      <h2 class="font-bold mb-2">
        {{ title }}
        <span>({{ assignments.length }})</span>
      </h2>
      <button v-show="canToggle" @click="$emit('toggle')">&times;</button>
    </div>

    <!-- We use v-model to bind the value and listen for when the value changes. 
    We also explicitly tell v-model that we want to call that specific model "currentTags". modelValue is kind of arbitrary.  -->
    <assignment-tags
      v-model:currentTag="currentTag"
      :initial-tags="assignments.map((a) => a.tag)"
    />

    <ul class="border border-gray-600 divide-y divide-gray-600 mt-6">
      <!-- Pass in the prop assignment which is an object coming from the 'Assignment'-component -->
      <assignment
        :assignment="assignment"
        v-for="assignment in filteredAssignments"
        :key="assignment.id"
      ></assignment>
    </ul>

    <slot></slot>
  </section>
</template>

<script>
import Assignment from './Assignment.vue'
import AssignmentTags from './AssignmentTags.vue'

export default {
  components: { Assignment, AssignmentTags },

  props: {
    assignments: Array,
    title: String,
    canToggle: { type: Boolean, default: false }
  },

  data() {
    return {
      currentTag: 'all'
    }
  },

  computed: {
    filteredAssignments() {
      if (this.currentTag === 'all') {
        return this.assignments
      }

      return this.assignments.filter((a) => a.tag === this.currentTag)
    }
  }
}
</script>
