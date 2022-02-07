<template>
  <div>
    <section>
      <base-card>
        <h2> {{ fullName }}</h2>
        <h3>{{ rate }}</h3>
      </base-card>
    </section>
    <section>
      <base-card>
        <header>
          <h2>Interested? Reach out now!</h2>
          <router-view></router-view>
        </header>
      </base-card>
    </section>
    <section>
      <base-card>
        <base-badge
          v-for="area in areas"
          :key="area"
          :type="area"
          :title="area"
        />
        <p>{{ description }}</p>
      </base-card>
    </section>
  </div>
</template>

<script>
import BaseCard from '@/components/ui/BaseCard';
import BaseBadge from '@/components/ui/BaseBadge';
export default {
  name: 'CoachDetail',

  components: { BaseBadge, BaseCard },

  props: ['id'],

  data() {
    return {
      selectedCoach: null,
    };
  },

  created() {
    this.selectedCoach = this.$store.getters['coaches/coaches'].find(
      coach => coach.id === this.id
    );
  },

  computed: {
    areas() {
      return this.selectedCoach.areas;
    },

    rate() {
      return this.selectedCoach.hourlyRate;
    },

    description() {
      return this.selectedCoach.description;
    },

    fullName() {
      return this.selectedCoach.firstName + ' ' + this.selectedCoach.lastName;
    },
  },
};
</script>

<style scoped>

</style>