<template>
  <section class="section hero-section" id="hero">
    <div class="hero-container">
      <div class="hero-text">
        <h1>Begleitung für die wichtigsten Momente</h1>
        <p class="hero-subtext">Experten-Wissen und praktische Hilfestellungen für den Familienalltag – von Trotzphase nach der Pubertät.</p>
        <div class="hero-features">
          <div class="feature">
            <div class="feature-icon">🧠</div>
            <p>Ausführliche Artikel</p>
          </div>
          <div class="feature">
            <div class="feature-icon">👨‍👩‍👧‍👦</div>
            <p>Von Eltern und Pädagogen entwickelt</p>
          </div>
          <div class="feature">
            <div class="feature-icon">📱</div>
            <p>Jederzeit abrufbare Ratschläge</p>
          </div>
        </div>
        <div class="hero-cta">
          <button class="btn-primary">Loslegen</button>
          <button class="btn-secondary">Mehr erfahren</button>
        </div>
      </div>
      <div class="hero-visual">
        <div class="illustration">
          <div class="illustration-element leaf1">🍃</div>
          <div class="illustration-element leaf2">🌿</div>
          <div class="illustration-element leaf3">🌱</div>
          <div class="main-illustration">👨‍👩‍👧</div>
        </div>
      </div>
    </div>
  </section>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'HeroSection'
});
</script>

<style lang="scss" scoped>
@use 'sass:map';
@use '@/style/base/variables' as vars;
@use '@/style/base/mixins' as mixins;
@use '@/style/base/animations' as animations;

.hero-section {
  min-height: 85vh;
  display: flex;
  align-items: center;
  position: relative;
  overflow: hidden;

  
  &::before {
    content: '';
    position: absolute;
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
    z-index: -1;
    opacity: 0.4;
    
    @each $theme in ('light', 'dark') {
      .theme-#{$theme} & {
        background: radial-gradient(circle at 70% 30%, 
          rgba(mixins.theme-color($theme, accent-lime), 0.15) 0%, 
          rgba(mixins.theme-color($theme, accent-green), 0.05) 50%, 
          transparent 70%);
      }
    }
  }
  
  .hero-container {
    display: grid;
    grid-template-columns: 1.2fr 0.8fr;
    gap: map.get(vars.$spacing, xxl);
    width: 100%;
    
    @media (max-width: map.get(map.get(vars.$layout, breakpoints), desktop)) {
      grid-template-columns: 1fr;
    }
  }
  
  .hero-text {
    h1 {
      font-size: 3.5rem;
      font-weight: map.get(map.get(vars.$fonts, weights), extra-bold);
      line-height: 1.2;
      margin-bottom: map.get(vars.$spacing, l);
      position: relative;
      
      @each $theme in ('light', 'dark') {
        .theme-#{$theme} & {
          color: mixins.theme-color($theme, text-primary);
          
          &::after {
            content: '';
            position: absolute;
            width: 100px;
            height: 4px;
            bottom: -16px;
            left: 0;
            background: mixins.theme-gradient($theme, primary);
            border-radius: 4px;
          }
        }
      }
      
      @media (max-width: map.get(map.get(vars.$layout, breakpoints), tablet)) {
        font-size: 2.5rem;
      }
    }
    
    .hero-subtext {
      font-size: map.get(map.get(vars.$fonts, sizes), xl);
      margin-bottom: map.get(vars.$spacing, xl);
      max-width: 600px;
      line-height: 1.6;
      
      @each $theme in ('light', 'dark') {
        .theme-#{$theme} & {
          color: mixins.theme-color($theme, text-secondary);
        }
      }
    }
  }
  
  .hero-features {
    display: flex;
    flex-direction: column;
    gap: map.get(vars.$spacing, l);
    margin-bottom: map.get(vars.$spacing, xxl);
    
    .feature {
      display: flex;
      align-items: center;
      gap: map.get(vars.$spacing, s);
      
      .feature-icon {
        width: 50px;
        height: 50px;
        display: flex;
        align-items: center;
        justify-content: center;
        border-radius: 50%;
        font-size: 1.5rem;
        
        @each $theme in ('light', 'dark') {
          .theme-#{$theme} & {
            background-color: mixins.theme-color($theme, secondary-bg);
            box-shadow: 0 4px 12px rgba(mixins.theme-color($theme, shadow-color), 0.15);
          }
        }
      }
      
      p {
        font-size: map.get(map.get(vars.$fonts, sizes), medium);
        font-weight: map.get(map.get(vars.$fonts, weights), medium);
        margin: 0;
        
        @each $theme in ('light', 'dark') {
          .theme-#{$theme} & {
            color: mixins.theme-color($theme, text-primary);
          }
        }
      }
      
      &:nth-child(1) .feature-icon {
        @each $theme in ('light', 'dark') {
          .theme-#{$theme} & {
            background-color: rgba(mixins.theme-color($theme, accent-teal), 0.15);
          }
        }
      }
      
      &:nth-child(2) .feature-icon {
        @each $theme in ('light', 'dark') {
          .theme-#{$theme} & {
            background-color: rgba(mixins.theme-color($theme, accent-lime), 0.15);
          }
        }
      }
      
      &:nth-child(3) .feature-icon {
        @each $theme in ('light', 'dark') {
          .theme-#{$theme} & {
            background-color: rgba(mixins.theme-color($theme, accent-green), 0.15);
          }
        }
      }
    }
  }
  
  .hero-cta {
    display: flex;
    gap: map.get(vars.$spacing, m);
    margin-top: map.get(vars.$spacing, xl);
    padding: 1rem;
    
    @media (max-width: map.get(map.get(vars.$layout, breakpoints), mobile)) {
      flex-direction: column;
    }
    
    .btn-primary, .btn-secondary {
      padding: map.get(vars.$spacing, m) map.get(vars.$spacing, xl);
      border-radius: map.get(map.get(vars.$layout, border-radius), pill);
      font-weight: map.get(map.get(vars.$fonts, weights), bold);
      font-size: map.get(map.get(vars.$fonts, sizes), medium);
      cursor: pointer;
      transition: transform map.get(vars.$transitions, default),
                 box-shadow map.get(vars.$transitions, default);
                 transform: scale(1.05)
    }
    
    .btn-primary {
      @each $theme in ('light', 'dark') {
        .theme-#{$theme} & {
          @include mixins.button-style($theme, 'medium', true);
        }
      }
    }
    
    .btn-secondary {
      @each $theme in ('light', 'dark') {
        .theme-#{$theme} & {
          background-color: transparent;
          color: mixins.theme-color($theme, text-primary);
          border: 2px solid mixins.theme-color($theme, border-medium);
          
          &:hover {
            transform: translateY(-4px);
            @include mixins.shadow('small', $theme);
            border-color: mixins.theme-color($theme, accent-teal);
          }
        }
      }
    }
  }
  
  .hero-visual {
    position: relative;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    
    @media (max-width: map.get(map.get(vars.$layout, breakpoints), desktop)) {
      display: none;
    }
    
    .illustration {
      position: relative;
      width: 300px;
      height: 300px;
      
      .main-illustration {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        font-size: 8rem;
        z-index: 2;
      }
      
      .illustration-element {
        position: absolute;
        font-size: 3rem;
        z-index: 1;
        
        &.leaf1 {
          top: 10%;
          left: 0;
        }
        
        &.leaf2 {
          top: 70%;
          left: 10%;
        }
        
        &.leaf3 {
          top: 20%;
          right: 10%;
        }
      }
      
      &::before {
        content: '';
        position: absolute;
        width: 250px;
        height: 250px;
        border-radius: 50%;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        z-index: 0;
        
        @each $theme in ('light', 'dark') {
          .theme-#{$theme} & {
            background: radial-gradient(
              circle,
              rgba(mixins.theme-color($theme, accent-green), 0.15) 0%,
              rgba(mixins.theme-color($theme, accent-lime), 0.08) 60%,
              transparent 80%
            );
          }
        }
      }
    }
  }
}
</style>