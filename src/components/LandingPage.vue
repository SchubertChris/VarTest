<template>
  <div class="landing-page" :class="{ 'theme-light': isLightTheme, 'theme-dark': !isLightTheme }">
    <!-- Blätter-Animation -->
    <div class="leaves"></div>
    
    <div class="container">
      <header>
        <div class="logo">Anmeldung</div>
        <div class="site-name">Seiten name</div>
      </header>
      
      <div class="nav-tabs">
        <div 
          v-for="(tab, index) in tabs" 
          :key="index" 
          class="nav-tab" 
          :class="{ active: activeTab === index }"
          @click="activeTab = index"
        >
          {{ tab }}
        </div>
      </div>
      
      <section class="section" v-for="(section, index) in sections" :key="index" ref="sections">
        <h2>{{ section.title }}</h2>
        <div v-html="section.content"></div>
        
        <!-- Dynamische Komponenten je nach Abschnittstyp -->
        <div class="content-blocks" v-if="section.type === 'blocks'">
          <div class="content-block" v-for="(block, blockIndex) in section.blocks" :key="blockIndex">
            <h3>{{ block.title }}</h3>
            <p v-for="(paragraph, pIndex) in block.paragraphs" :key="pIndex">{{ paragraph }}</p>
          </div>
        </div>
        
        <div class="quiz-container" v-if="section.type === 'quiz'">
          <div class="quiz-question" v-for="(question, qIndex) in section.questions" :key="qIndex">
            <p v-html="question.text"></p>
            <p v-if="question.options" v-html="question.options"></p>
          </div>
        </div>
        
        <div class="community-profiles" v-if="section.type === 'profiles'">
          <div class="profile" v-for="(profile, pIndex) in section.profiles" :key="pIndex">
            <div class="profile-icon">{{ profile.icon }}</div>
            <h3>{{ profile.title }}</h3>
            <p>{{ profile.description }}</p>
          </div>
        </div>
        
        <div class="subscription" v-if="section.type === 'subscription'">
          <div class="subscription-info">
            <p>{{ section.info.text }}</p>
            <p v-html="section.info.bulletPoints"></p>
          </div>
          
          <div class="form-container">
            <div class="form-group" v-for="(field, fIndex) in section.form.fields" :key="fIndex">
              <label :for="field.id">{{ field.label }}</label>
              <input v-if="field.type !== 'select'" :type="field.type" :id="field.id" :placeholder="field.placeholder">
              <select v-else :id="field.id">
                <option value="">{{ field.placeholder }}</option>
                <option v-for="(option, oIndex) in field.options" :key="oIndex" :value="option.value">
                  {{ option.text }}
                </option>
              </select>
            </div>
            <button class="btn">{{ section.form.buttonText }}</button>
          </div>
        </div>
        
        <p class="footnote" v-if="section.footnote">{{ section.footnote }}</p>
      </section>
    </div>
    
    <!-- Theme-Toggle -->
    <div class="theme-toggle-container">
      <button id="themeToggle" title="Theme wechseln" @click="toggleTheme">
        <span class="dark-icon" v-if="isLightTheme">🌙</span>
        <span class="light-icon" v-else>☀️</span>
      </button>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted } from 'vue';

export default defineComponent({
  name: 'LandingPage',
  setup() {
    const isLightTheme = ref(false);
    const activeTab = ref(0);
    const sections = ref<HTMLElement[]>([]);
    
    const tabs = [
      'Über',
      'Was wir tun',
      'Bücher',
      'Community',
      'Über Autoren'
    ];
    
    // Inhaltsdaten (diese könnten auch über eine API geladen werden)
    const sectionsData = [
      {
        title: 'Die Idee',
        content: `
          <p>Immer wieder stellen sich Eltern dieselbe Frage: "Mache ich alles richtig?" oder: "Warum verhält sich mein Kind so?"</p>
          <p>Der Alltag von Eltern ist oft geprägt von Stress, Unsicherheit und Überforderung.</p>
          <p>Wir von <span class="highlighted">Eltern und Kind</span> haben es uns zur Aufgabe gemacht, Eltern und Kindern zu guten Zeiten zu begleiten und ebenso bei den schwierigen und großen Fragen des Lebens.</p>
          <p>In enger Zusammenarbeit mit Pädagogen und Bildungseinrichtungen entwickeln wir eine stetig wachsende Bibliothek. Sie bietet:</p>
          
          <ul>
            <li>Wissenschaftlich fundierte Inhalte für Eltern zu Themen wie Entwicklung, Erziehung, Beziehung und Kommunikation</li>
            <li>Kindgerecht formulierte Artikel, Geschichten und Erklärungen, die Kindern helfen, ihre Welt besser zu verstehen</li>
          </ul>
          
          <p>Ob frühkindliche Trotz, Pubertät oder die ersten Erfahrungen mit Konflikten, unsere Plattform bietet Orientierung und stärkt die Verbindung zwischen Eltern und Kindern.</p>
        `
      },
      {
        title: 'Was ist das?',
        type: 'blocks',
        blocks: [
          {
            title: 'Für Eltern',
            paragraphs: [
              'Eine Sammlung an Texten, die dir helfen, dein Kind besser zu verstehen',
              'Hilfestellungen, wie du deinem Kind bei verschiedenen Herausforderungen helfen kannst',
              'Eine Community für den Austausch mit anderen Eltern, die ähnliche Erfahrungen machen'
            ]
          },
          {
            title: 'Kinder',
            paragraphs: [
              'Alles, was du über Gefühle, Freundschaften und die große Welt wissen möchtest',
              'Lustige Geschichten, die dir helfen, dich und andere besser zu verstehen',
              'Spannende Erklärungen zu den Dingen, die dich neugierig machen'
            ]
          }
        ]
      },
      {
        title: 'Quiz',
        type: 'quiz',
        questions: [
          {
            text: 'Kennst du dich auch nicht immer gut mit deinem Kind aus? Du bist nicht allein! Hier findest du hilfreiche Tipps und Ideen, was du tun kannst, wenn du mal nicht weiter weißt.<br>Probier doch mal unser Quiz aus und finde heraus, welcher Erziehungstyp du bist!'
          },
          {
            text: 'Weißt du, in welchen Phasen dein Kind natürlich schwieriger wird? Die berühmten Trotzphasen kommen mit:',
            options: 'a) ungefähr mit 2-3 Jahren<br>b) nur bei schlecht erzogenen Kindern<br>c) etwa mit 13-14 Jahren'
          },
          {
            text: 'Was war für dich die spannendste Artikelreihe? Lass uns wissen, was dir gefallen hat und wir erweitern unser Angebot für euch!'
          },
          {
            text: 'Du bist auf der Suche nach konkreten und positiven Hilfen für deinen Alltag mit Kind?',
            options: 'a) Ja, ich bin oft überfordert<br>b) Nur manchmal bei schwierigen Situationen<br>c) Nein, mir fällt immer eine gute Lösung ein'
          }
        ]
      },
      {
        title: 'Community',
        type: 'profiles',
        content: `
          <p>Kein Projekt von dieser Größe kann allein stehen und die beste geteilte Weisheit ist die, die durch Erfahrung gewachsen ist. Darum laden wir alle ein, sich hier einzubringen!</p>
          <p>Gemeinsam mit Pädagogen und Familientherapeuten sorgen wir dafür, dass unsere Inhalte geprüft und hilfreich zu lesen sind. Deine Beiträge tragen dazu bei, dass auch andere von diesen Erfahrungen profitieren können.</p>
          <p>Wir stärken nur Inhalte und nicht institutionelle Bindungen. Freiheit und Selbstbestimmung stehen für uns an erster Stelle.</p>
        `,
        profiles: [
          {
            icon: '👨‍👩‍👧',
            title: 'Neu (Kleinkinder)',
            description: 'Erste Schritte in der Erziehung'
          },
          {
            icon: '👨‍👩‍👦',
            title: 'Erfahren',
            description: 'Ratschläge für die mittlere Kindheit'
          },
          {
            icon: '👨‍👩‍👧‍👦',
            title: 'Profi (Jugendliche)',
            description: 'Umgang mit heranwachsenden Kindern'
          },
          {
            icon: '👨‍👨‍👧‍👧',
            title: 'Experten',
            description: 'Pädagogen und Familientherapeuten'
          }
        ]
      },
      {
        title: 'Mehr dazu erfahren',
        type: 'subscription',
        info: {
          text: 'Melde dich für den Newsletter an und erhalte regelmäßig neue Artikel und Informationen zu für dich relevanten Themen.',
          bulletPoints: '• Zweimal im Monat neue Inhalte<br>• Kostenlos und jederzeit kündbar<br>• Du bleibst immer auf dem neuesten Stand'
        },
        form: {
          fields: [
            {
              type: 'text',
              id: 'name',
              label: 'Vorname',
              placeholder: 'Dein Name'
            },
            {
              type: 'email',
              id: 'email',
              label: 'E-Mail',
              placeholder: 'Deine E-Mail'
            },
            {
              type: 'select',
              id: 'age',
              label: 'Alter der Kinder',
              placeholder: 'Bitte wählen',
              options: [
                { value: '0-3', text: '0-3 Jahre' },
                { value: '4-6', text: '4-6 Jahre' },
                { value: '7-12', text: '7-12 Jahre' },
                { value: '13+', text: '13+ Jahre' }
              ]
            }
          ],
          buttonText: 'Anmelden'
        },
        footnote: '* Deine Daten werden vertraulich und ausschließlich für die Zusendung des Newsletters verwendet. Du kannst dich jederzeit mit einem Klick einfach abmelden, wenn diese gegen die Bedingungen verstoßen.'
      }
    ];
    
    const toggleTheme = () => {
      isLightTheme.value = !isLightTheme.value;
      // Optional: Speichern der Präferenz im localStorage
      localStorage.setItem('theme', isLightTheme.value ? 'light' : 'dark');
    };
    
    // Blätter-Animation
    const createLeaf = () => {
      const leaf = document.createElement('div');
      leaf.classList.add('leaf');
      
      // Zufällige Blattform und -farbe wählen
      const leafTypes = [
        "data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'%3E%3Cpath fill='%234AD295' d='M17,8C8,10 5.9,16.17 3.82,21.34L5.71,22L6.66,19.7C7.14,19.87 7.64,20 8,20C19,20 22,3 22,3C21,5 14,5.25 9,6.25C4,7.25 2,11.5 2,13.5C2,15.5 3.75,17.25 3.75,17.25C7,8 17,8 17,8Z'/%3E%3C/svg%3E",
        "data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'%3E%3Cpath fill='%239BE15D' d='M17,8C8,10 5.9,16.17 3.82,21.34L5.71,22L6.66,19.7C7.14,19.87 7.64,20 8,20C19,20 22,3 22,3C21,5 14,5.25 9,6.25C4,7.25 2,11.5 2,13.5C2,15.5 3.75,17.25 3.75,17.25C7,8 17,8 17,8Z'/%3E%3C/svg%3E",
        "data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'%3E%3Cpath fill='%2335CCD0' d='M17,8C8,10 5.9,16.17 3.82,21.34L5.71,22L6.66,19.7C7.14,19.87 7.64,20 8,20C19,20 22,3 22,3C21,5 14,5.25 9,6.25C4,7.25 2,11.5 2,13.5C2,15.5 3.75,17.25 3.75,17.25C7,8 17,8 17,8Z'/%3E%3C/svg%3E"
      ];
      
      // Zuweisung der CSS-Variablen für Animation
      const randomX = Math.random() * 200 - 100;
      const randomRotationStart = Math.random() * 360;
      const randomRotationEnd = randomRotationStart + 360;
      const duration = Math.random() * 10 + 5; // 5-15 Sekunden
      
      leaf.style.setProperty('--random-x', `${randomX}px`);
      leaf.style.setProperty('--start-rotation', `${randomRotationStart}deg`);
      leaf.style.setProperty('--end-rotation', `${randomRotationEnd}deg`);
      leaf.style.setProperty('--duration', `${duration}s`);
      
      leaf.style.backgroundImage = `url(${leafTypes[Math.floor(Math.random() * leafTypes.length)]})`;
      leaf.style.top = '-30px';
      leaf.style.left = `${Math.random() * 100}vw`;
      
      document.querySelector('.leaves')?.appendChild(leaf);
      
      // Blatt nach Ende der Animation entfernen
      setTimeout(() => {
        leaf.remove();
      }, duration * 1000);
    };
    
    onMounted(() => {
      // Theme aus localStorage laden, falls vorhanden
      const savedTheme = localStorage.getItem('theme');
      if (savedTheme) {
        isLightTheme.value = savedTheme === 'light';
      }
      
      // Scroll-Animation für Sektionen
      const observer = new IntersectionObserver((entries) => {
        entries.forEach(entry => {
          if (entry.isIntersecting) {
            entry.target.classList.add('visible');
          } else {
            entry.target.classList.remove('visible');
          }
        });
      }, { threshold: 0.1 });
      
      const sectionElements = document.querySelectorAll('.section');
      sectionElements.forEach(section => {
        observer.observe(section);
      });
      
      // Blätter-Animation starten
      // Initial ein paar Blätter erzeugen
      for (let i = 0; i < 5; i++) {
        setTimeout(createLeaf, i * 1000);
      }
      
      // Blätter in Intervallen erzeugen
      setInterval(createLeaf, 3000);
    });
    
    return {
      isLightTheme,
      activeTab,
      tabs,
      sections: sectionsData,
      toggleTheme
    };
  }
});
</script>

<style lang="scss">
@use '@/style/base/variables' as *;
@use '@/style/base/mixins' as *;
@use '@/style/base/animations' as *;

// Hier kommen die spezifischen Styles für die Landing Page
.landing-page {
  min-height: 100vh;
  
  &.theme-light {
    background-color: theme-color('light', primary-bg);
    color: theme-color('light', text-primary);
    background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='260' height='260' viewBox='0 0 260 260'%3E%3Cg fill-rule='evenodd'%3E%3Cg fill='%2326BB77' fill-opacity='0.2'%3E%3Cpath d='M24.37 16c.2.65.39 1.32.54 2H21.17l1.17 2.34.45.9-.24.11V28a5 5 0 0 1-2.23 8.94l-.02.06a8 8 0 0 1-7.75 6h-20a8 8 0 0 1-7.74-6l-.02-.06A5 5 0 0 1-17.45 28v-6.76l-.79-1.58-.44-.9.9-.44.63-.32H-20a23.01 23.01 0 0 1 44.37-2zm-36.82 2a1 1 0 0 0-.44.1l-3.1 1.56.89 1.79 1.31-.66a3 3 0 0 1 2.69 0l2.2 1.1a1 1 0 0 0 .9 0l2.21-1.1a3 3 0 0 1 2.69 0l2.2 1.1a1 1 0 0 0 .9 0l2.21-1.1a3 3 0 0 1 2.69 0l2.2 1.1a1 1 0 0 0 .86.02l2.88-1.27a3 3 0 0 1 2.43 0l2.88 1.27a1 1 0 0 0 .85-.02z'/%3E%3C/g%3E%3C/svg%3E");
  }
  
  &.theme-dark {
    background-color: theme-color('dark', primary-bg);
    color: theme-color('dark', text-primary);
    background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='260' height='260' viewBox='0 0 260 260'%3E%3Cg fill-rule='evenodd'%3E%3Cg fill='%23285F42' fill-opacity='0.4'%3E%3Cpath d='M24.37 16c.2.65.39 1.32.54 2H21.17l1.17 2.34.45.9-.24.11V28a5 5 0 0 1-2.23 8.94l-.02.06a8 8 0 0 1-7.75 6h-20a8 8 0 0 1-7.74-6l-.02-.06A5 5 0 0 1-17.45 28v-6.76l-.79-1.58-.44-.9.9-.44.63-.32H-20a23.01 23.01 0 0 1 44.37-2zm-36.82 2a1 1 0 0 0-.44.1l-3.1 1.56.89 1.79 1.31-.66a3 3 0 0 1 2.69 0l2.2 1.1a1 1 0 0 0 .9 0l2.21-1.1a3 3 0 0 1 2.69 0l2.2 1.1a1 1 0 0 0 .9 0l2.21-1.1a3 3 0 0 1 2.69 0l2.2 1.1a1 1 0 0 0 .86.02l2.88-1.27a3 3 0 0 1 2.43 0l2.88 1.27a1 1 0 0 0 .85-.02z'/%3E%3C/g%3E%3C/svg%3E");
  }
}

// Basis-Styling
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  transition: all map-get($transitions, default);
}

.container {
  max-width: map-get($layout, max-width);
  margin: 0 auto;
  padding: map-get($spacing, xl);
}

// Layout-Komponenten
header {
  @include flex(row, space-between, center);
  padding: map-get($spacing, m) 0;
  margin-bottom: map-get($spacing, xl);
  
  .logo {
    font-size: map-get(map-get($fonts, sizes), xxl);
    font-weight: map-get(map-get($fonts, weights), bold);
    
    @each $theme in ('light', 'dark') {
      .theme-#{$theme} & {
        @include text-gradient('primary', $theme);
        
        &::after {
          content: '';
          position: absolute;
          bottom: -5px;
          left: 0;
          width: 100%;
          height: 2px;
          background: theme-gradient($theme, primary);
          border-radius: 2px;
        }
      }
    }
  }
  
  .site-name {
    font-size: map-get(map-get($fonts, sizes), large);
    position: relative;
    overflow: hidden;
    
    @each $theme in ('light', 'dark') {
      .theme-#{$theme} & {
        color: theme-color($theme, text-secondary);
        
        &::after {
          content: '';
          position: absolute;
          bottom: 0;
          left: -100%;
          width: 100%;
          height: 2px;
          background: linear-gradient(to right, theme-color($theme, accent-teal), transparent);
          @include animate(shimmer, 3s, ease, 0s, infinite);
        }
      }
    }
  }
}

// Navigation
.nav-tabs {
  @include flex(row, center, center, wrap);
  gap: map-get($spacing, m);
  margin: map-get($spacing, xl) 0;
  
  .nav-tab {
    position: relative;
    overflow: hidden;
    padding: map-get($spacing, s) map-get($spacing, xl);
    border-radius: map-get(map-get($layout, border-radius), pill);
    font-weight: map-get(map-get($fonts, weights), bold);
    font-size: map-get(map-get($fonts, sizes), medium);
    cursor: pointer;
    
    @each $theme in ('light', 'dark') {
      .theme-#{$theme} & {
        background-color: theme-color($theme, nav-item-bg);
        color: theme-color($theme, text-primary);
        border: 2px solid theme-color($theme, border-light);
        @include shadow('small', $theme);
        
        &::before {
          content: '';
          position: absolute;
          top: 0;
          left: -100%;
          width: 100%;
          height: 100%;
          background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.1), transparent);
          transition: left map-get($transitions, slow);
        }
        
        &:hover, &.active {
          background: theme-gradient($theme, nav-active);
          color: white;
          transform: translateY(-4px);
          @include glow('green', 'medium', $theme);
          border-color: transparent;
        }
        
        &:hover::before {
          left: 100%;
        }
      }
    }
  }
}

// Sektionen
.section {
  position: relative;
  overflow: hidden;
  margin-bottom: map-get($spacing, xxxl);
  border-radius: map-get(map-get($layout, border-radius), xlarge);
  @include scroll-fade-in;
  
  @each $theme in ('light', 'dark') {
    .theme-#{$theme} & {
      @include card-style($theme, 'large', false);
      
      &::before {
        content: '';
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 5px;
        background: theme-gradient($theme, header);
        border-radius: map-get(map-get($layout, border-radius), xlarge) map-get(map-get($layout, border-radius), xlarge) 0 0;
      }
    }
  }
  
  h2 {
    font-size: map-get(map-get($fonts, sizes), xxxl);
    font-weight: map-get(map-get($fonts, weights), extra-bold);
    
    @each $theme in ('light', 'dark') {
      .theme-#{$theme} & {
        @include section-header($theme);
      }
    }
  }
  
  p {
    font-size: map-get(map-get($fonts, sizes), medium);
    margin-bottom: map-get($spacing, l);
    line-height: 1.8;
  }
  
  .highlighted {
    font-weight: map-get(map-get($fonts, weights), extra-bold);
    position: relative;
    display: inline-block;
    padding: 0 4px;
    
    @each $theme in ('light', 'dark') {
      .theme-#{$theme} & {
        color: theme-color($theme, accent-green);
        
        &::before {
          content: '';
          position: absolute;
          width: 100%;
          height: 30%;
          bottom: 0;
          left: 0;
          background-color: rgba(theme-color($theme, accent-green), 0.15);
          z-index: -1;
          border-radius: 4px;
        }
      }
    }
  }
  
  ul {
    margin-left: map-get($spacing, xxl);
    margin-bottom: map-get($spacing, l);
    
    li {
      margin-bottom: map-get($spacing, s);
      position: relative;
      
      &::before {
        content: '🌿';
        position: absolute;
        left: -1.8rem;
        top: 0.2rem;
      }
    }
  }
}

// Content Blocks
.content-blocks {
  @include flex(row, space-between, stretch, wrap);
  gap: map-get($spacing, xxl);
  margin-top: map-get($spacing, xxl);
  
  @include responsive('tablet') {
    flex-direction: column;
  }
  
  .content-block {
    flex: 1;
    position: relative;
    overflow: hidden;
    
    @each $theme in ('light', 'dark') {
      .theme-#{$theme} & {
        background-color: theme-color($theme, secondary-bg);
        padding: map-get($spacing, xl);
        border-radius: map-get(map-get($layout, border-radius), large);
        border: 2px solid theme-color($theme, border-light);
        @include shadow('medium', $theme);
      }
    }
    
    &:nth-child(1) {
      transform: rotate(-2deg);
      
      @each $theme in ('light', 'dark') {
        .theme-#{$theme} & {
          border-top: 4px solid theme-color($theme, accent-green);
        }
      }
    }
    
    &:nth-child(2) {
      transform: rotate(2deg);
      
      @each $theme in ('light', 'dark') {
        .theme-#{$theme} & {
          border-top: 4px solid theme-color($theme, accent-teal);
        }
      }
    }
    
    &:nth-child(1):hover {
      transform: rotate(0deg) translateY(-10px);
      
      @each $theme in ('light', 'dark') {
        .theme-#{$theme} & {
          @include glow('green', 'large', $theme);
          border-color: theme-color($theme, accent-green);
        }
      }
    }
    
    &:nth-child(2):hover {
      transform: rotate(0deg) translateY(-10px);
      
      @each $theme in ('light', 'dark') {
        .theme-#{$theme} & {
          @include glow('teal', 'large', $theme);
          border-color: theme-color($theme, accent-teal);
        }
      }
    }
    
    h3 {
      font-size: map-get(map-get($fonts, sizes), xl);
      margin-bottom: map-get($spacing, m);
      position: relative;
      display: inline-block;
      
      &::after {
        content: '';
        position: absolute;
        width: 100%;
        height: 3px;
        bottom: -5px;
        left: 0;
        border-radius: 3px;
      }
      
      @each $theme in ('light', 'dark') {
        .theme-#{$theme} & {
          color: theme-color($theme, text-primary);
        }
      }
    }
    
    &:nth-child(1) h3::after {
      @each $theme in ('light', 'dark') {
        .theme-#{$theme} & {
          background: theme-color($theme, accent-green);
        }
      }
    }
    
    &:nth-child(2) h3::after {
      @each $theme in ('light', 'dark') {
        .theme-#{$theme} & {
          background: theme-color($theme, accent-teal);
        }
      }
    }
  }
}

// Quiz-Container
.quiz-container {
  @include grid(1fr 1fr, auto, map-get($spacing, xl));
  margin-top: map-get($spacing, xxl);
  
  @include responsive('tablet') {
    grid-template-columns: 1fr;
  }
  
  .quiz-question {
    @each $theme in ('light', 'dark') {
      .theme-#{$theme} & {
        background-color: theme-color($theme, secondary-bg);
        padding: map-get($spacing, xl);
        border-radius: map-get(map-get($layout, border-radius), large);
        border: 2px solid theme-color($theme, border-light);
        @include shadow('medium', $theme);
        transition: all map-get($transitions, default);
      }
    }
    
    &:hover {
      transform: translateY(-8px) scale(1.02);
      
      @each $theme in ('light', 'dark') {
        .theme-#{$theme} & {
          @include glow('green', 'medium', $theme);
          border-color: theme-color($theme, accent-green);
        }
      }
    }
    
    @for $i from 1 through 4 {
      &:nth-child(#{$i}) {
        @each $theme in ('light', 'dark') {
          .theme-#{$theme} & {
            @if $i == 1 {
              border-left: 4px solid theme-color($theme, accent-green);
            } @else if $i == 2 {
              border-left: 4px solid theme-color($theme, accent-teal);
            } @else if $i == 3 {
              border-left: 4px solid theme-color($theme, accent-lime);
            } @else if $i == 4 {
              border-left: 4px solid theme-color($theme, accent-yellow);
            }
          }
        }
      }
    }
  }
}

// Community Profiles
.community-profiles {
  @include flex(row, space-between, stretch, wrap);
  gap: map-get($spacing, l);
  margin-top: map-get($spacing, xxl);
  
  @include responsive('tablet') {
    flex-direction: column;
  }
  
  .profile {
    flex: 1;
    min-width: 200px;
    text-align: center;
    position: relative;
    overflow: hidden;
    
    @each $theme in ('light', 'dark') {
      .theme-#{$theme} & {
        background-color: theme-color($theme, secondary-bg);
        padding: map-get($spacing, xl);
        border-radius: map-get(map-get($layout, border-radius), large);
        border: 2px solid theme-color($theme, border-light);
        @include shadow('medium', $theme);
        
        &::before {
          content: '';
          position: absolute;
          top: 0;
          left: 0;
          width: 100%;
          height: 100%;
          z-index: 0;
          
          @if $theme == 'dark' {
            background: linear-gradient(135deg, rgba(255, 255, 255, 0.05), transparent);
          } @else {
            background: linear-gradient(135deg, rgba(255, 255, 255, 0.5), transparent);
          }
        }
        
        &:hover {
          transform: translateY(-8px) scale(1.05);
          @include glow('green', 'medium', $theme);
          border-color: theme-color($theme, accent-green);
        }
      }
    }
    
    .profile-icon {
      position: relative;
      z-index: 1;
      margin: 0 auto map-get($spacing, l);
      @include pulse;
      
      @each $theme in ('light', 'dark') {
        .theme-#{$theme} & {
          @include icon-container($theme, 'medium', 'primary');
        }
      }
    }
    
    @for $i from 1 through 4 {
      &:nth-child(#{$i}) .profile-icon {
        @each $theme in ('light', 'dark') {
          .theme-#{$theme} & {
            @if $i == 1 {
              background: linear-gradient(135deg, theme-color($theme, accent-green), theme-color($theme, accent-lime));
              box-shadow: 0 5px 15px theme-color($theme, glow-green);
            } @else if $i == 2 {
              background: linear-gradient(135deg, theme-color($theme, accent-teal), theme-color($theme, accent-lime));
              box-shadow: 0 5px 15px theme-color($theme, glow-teal);
              animation-delay: 0.5s;
            } @else if $i == 3 {
              background: linear-gradient(135deg, theme-color($theme, accent-green), theme-color($theme, accent-teal));
              box-shadow: 0 5px 15px theme-color($theme, glow-green);
              animation-delay: 1s;
            } @else if $i == 4 {
              background: linear-gradient(135deg, theme-color($theme, accent-yellow), theme-color($theme, accent-lime));
              box-shadow: 0 5px 15px rgba(theme-color($theme, accent-yellow), 0.4);
              animation-delay: 1.5s;
            }
          }
        }
      }
    }
    
    h3 {
      font-size: map-get(map-get($fonts, sizes), large);
      margin-bottom: map-get($spacing, s);
      position: relative;
      z-index: 1;
      
      @each $theme in ('light', 'dark') {
        .theme-#{$theme} & {
          color: theme-color($theme, text-primary);
        }
      }
    }
    
    p {
      font-size: map-get(map-get($fonts, sizes), small);
      position: relative;
      z-index: 1;
      
      @each $theme in ('light', 'dark') {
        .theme-#{$theme} & {
          color: theme-color($theme, text-secondary);
        }
      }
    }
  }
}

// Subscription
.subscription {
  @include grid(1fr 1fr, auto, map-get($spacing, xxl));
  margin-top: map-get($spacing, xxl);
  
  @include responsive('tablet') {
    grid-template-columns: 1fr;
  }
  
  .subscription-info, .form-container {
    position: relative;
    
    @each $theme in ('light', 'dark') {
      .theme-#{$theme} & {
        background-color: theme-color($theme, secondary-bg);
        padding: map-get($spacing, xl);
        border-radius: map-get(map-get($layout, border-radius), large);
        border: 2px solid theme-color($theme, border-light);
        @include shadow('medium', $theme);
        transition: all map-get($transitions, default);
      }
    }
    
    &:hover {
      transform: translateY(-5px);
    }
  }
  
  .subscription-info:hover {
    @each $theme in ('light', 'dark') {
      .theme-#{$theme} & {
        @include glow('green', 'medium', $theme);
        border-color: theme-color($theme, accent-green);
      }
    }
  }
  
  .form-container:hover {
    @each $theme in ('light', 'dark') {
      .theme-#{$theme} & {
        @include glow('teal', 'medium', $theme);
        border-color: theme-color($theme, accent-teal);
      }
    }
  }
}

// Formular-Elemente
.form-group {
  margin-bottom: map-get($spacing, l);
  
  label {
    display: block;
    margin-bottom: map-get($spacing, s);
    font-weight: map-get(map-get($fonts, weights), bold);
    
    @each $theme in ('light', 'dark') {
      .theme-#{$theme} & {
        color: theme-color($theme, text-secondary);
      }
    }
  }
  
  input, select {
    @each $theme in ('light', 'dark') {
      .theme-#{$theme} & {
        @include form-element($theme);
      }
    }
  }
}

// Button
.btn {
  display: inline-block;
  margin-top: map-get($spacing, m);
  @include shine-effect;
  
  @each $theme in ('light', 'dark') {
    .theme-#{$theme} & {
      @include button-style($theme, 'medium', true);
    }
  }
}

// Footnote
.footnote {
  font-size: map-get(map-get($fonts, sizes), small);
  margin-top: map-get($spacing, xxl);
  text-align: center;
  line-height: 1.6;
  
  @each $theme in ('light', 'dark') {
    .theme-#{$theme} & {
      color: theme-color($theme, text-tertiary);
    }
  }
}

// Theme Toggle
.theme-toggle-container {
  position: fixed;
  bottom: 20px;
  right: 20px;
  z-index: 100;
  
  button {
    width: 50px;
    height: 50px;
    border-radius: 50%;
    border: none;
    cursor: pointer;
    font-size: 24px;
    display: flex;
    align-items: center;
    justify-content: center;
    
    @each $theme in ('light', 'dark') {
      .theme-#{$theme} & {
        background-color: theme-color($theme, secondary-bg);
        box-shadow: 0 3px 10px theme-color($theme, shadow-color);
        
        &:hover {
          transform: scale(1.1);
        }
      }
    }
  }
}

// Animierte Blätter
.leaves {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: map-get($z-index, below);
  
  .leaf {
    position: absolute;
    opacity: 0;
    
    @each $theme in ('light', 'dark') {
      .theme-#{$theme} & {
        @include animated-leaf(5, 15);
      }
    }
  }
}
</style>