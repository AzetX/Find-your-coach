<template>
  <section>
    <base-card>
      <h2>Register as a coach now</h2>
      <coach-form @save-data="saveData"/>
    </base-card>
  </section>
</template>

<script>
import CoachForm from '@/components/coaches/CoachForm';
import BaseCard from '@/components/ui/BaseCard';

export default {
  name: 'RequestReceived',

  components: { BaseCard, CoachForm },

  methods: {
    saveData(data) {
      this.$store.dispatch('coaches/registerCoachAction', data);
      this.$router.replace('/coaches');
    }
  }
};

</script>

<style scoped>

</style>