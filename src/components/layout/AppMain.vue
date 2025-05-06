<template>
  <main class="app-main container">
    <HeroSection />
    <ContentBlocksSection />
    <QuizSection />
    <CommunitySection />
    <SubscriptionSection />
  </main>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import HeroSection from '../sections/HeroSection.vue';
import ContentBlocksSection from '../sections/ContentBlocksSection.vue';
import QuizSection from '../sections/QuizSection.vue';
import CommunitySection from '../sections/CommunitySection.vue';
import SubscriptionSection from '../sections/SubscriptionSection.vue';

export default defineComponent({
  name: 'AppMain',
  components: {
    HeroSection,
    ContentBlocksSection,
    QuizSection,
    CommunitySection,
    SubscriptionSection
  }
});
</script>

<style lang="scss">
@use '@/style/base/variables' as vars;

.app-main {
  display: flex;
  flex-direction: column;
  gap: map-get(vars.$spacing, xxxl);
  padding-bottom: map-get(vars.$spacing, xxxl);
}
</style>