<template>
  <div>
    <base-dialog
      :show="!!error"
      title="An error occurred!"
      @close="handleError"
    >
      <p>{{ error }}</p>
    </base-dialog>
    <section>
      <coach-filter @change-filters="setFilter" />
    </section>
    <section>
      <base-card>
        <div class="controls">
          <base-button mode="outline" @click="loadCoaches(true)">Refresh</base-button>
          <base-button
            v-if="!isCoach && !isLoading"
            link
            to='/register'
          >
            Register as Coach
          </base-button>
        </div>
        <div v-if="isLoading">
          <base-spinner></base-spinner>
        </div>
        <ul v-else-if="hasCoaches">
          <coach-item
            v-for="coach in filteredCoaches"
            :key="coach.id"
            :id="coach.id"
            :firstName="coach.firstName"
            :lastName="coach.lastName"
            :rate="coach.hourlyRate"
            :areas="coach.areas"
          />
        </ul>
        <h3 v-else>No coaches found.</h3>
      </base-card>
    </section>
  </div>
</template>

<script>
import CoachItem from '@/components/coaches/CoachItem';
import BaseCard from '@/components/ui/BaseCard';
import BaseButton from '@/components/ui/BaseButton';
import CoachFilter from '@/components/coaches/CoachFilter';
import BaseSpinner from '@/components/ui/BaseSpinner';
import BaseDialog from '@/components/ui/BaseDialog';

export default {

  name: 'CoachesList',

  components: { BaseDialog, BaseSpinner, BaseButton, BaseCard, CoachItem, CoachFilter },

  data() {
    return {
      isLoading: false,
      error: null,
      activeFilters: {
        frontend: true,
        backend: true,
        career: true,
      }
    };
  },

  computed: {
    isCoach() {
      return !this.isLoading && this.$store.getters['coaches/isCoach'];
    },

    filteredCoaches() {
      const coaches = this.$store.getters['coaches/coaches'];
      //TODO: need to refactor it!
      return coaches.filter(coach => {
        if (this.activeFilters.frontend && coach.areas.includes('frontend')) {
          return true;
        }
        if (this.activeFilters.backend && coach.areas.includes('backend')) {
          return true;
        }
        if (this.activeFilters.career && coach.areas.includes('career')) {
          return true;
        }
        return false;
      })
    },

    hasCoaches() {
      return this.$store.getters['coaches/hasCoaches'];
    }
  },

  created() {
    this.loadCoaches();
  },

  methods: {
    setFilter(updateFilters) {
      this.activeFilters = updateFilters;
    },

    async loadCoaches(refresh = false) {
      this.isLoading = true;
      try {
        await this.$store.dispatch('coaches/loadCoachesAction', { forceRefresh: refresh});
      } catch (error) {
        this.error = error.message || 'Something went wrong!';
      }
      this.isLoading = false;
    },

    handleError() {
      this.error = null;
    },
  }
};
</script>

<style scoped>
  ul {
    list-style: none;
    margin: 0;
    padding: 0;
  }

  .controls {
    display: flex;
    justify-content: space-between;
  }
</style>