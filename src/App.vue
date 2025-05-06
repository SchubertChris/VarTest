<template>
  <div class="landing-page" :class="{ 'theme-light': isLightTheme, 'theme-dark': !isLightTheme }">
    <!-- Header -->
    <header class="app-header container">
      <div class="logo">Anmeldung</div>
      <div class="site-name">Seiten name</div>
      
      <nav class="nav-tabs">
        <button 
          v-for="(tab, index) in tabs" 
          :key="index" 
          class="nav-tab" 
          :class="{ active: activeTab === index }"
          @click="activeTab = index"
        >
          {{ tab }}
        </button>
      </nav>
    </header>

    <!-- Main Content -->
    <main class="container main-content">
      <!-- Hero Section -->
      <section class="section hero-section">
        <h2>Die Idee</h2>
        <div class="hero-content">
          <p>Immer wieder stellen sich Eltern dieselbe Frage: "Mache ich alles richtig?" oder: "Warum verhält sich mein Kind so?"</p>
          <p>Der Alltag von Eltern ist oft geprägt von Stress, Unsicherheit und Überforderung.</p>
          <p>Wir von <span class="highlighted">Eltern und Kind</span> haben es uns zur Aufgabe gemacht, Eltern und Kindern zu guten Zeiten zu begleiten und ebenso bei den schwierigen und großen Fragen des Lebens.</p>
        </div>
      </section>

      <!-- Content Blocks Section -->
      <section class="section content-blocks-section">
        <h2>Was ist das?</h2>
        
        <div class="content-blocks">
          <div class="content-block">
            <h3>Für Eltern</h3>
            <p>Eine Sammlung an Texten, die dir helfen, dein Kind besser zu verstehen</p>
          </div>
          
          <div class="content-block">
            <h3>Kinder</h3>
            <p>Alles, was du über Gefühle, Freundschaften und die große Welt wissen möchtest</p>
          </div>
        </div>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      isLightTheme: true,
      activeTab: 0,
      tabs: [
        'Über',
        'Was wir tun',
        'Bücher',
        'Community',
        'Über Autoren'
      ]
    }
  }
}
</script>

<style>
/* Basis-Styling */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Nunito', sans-serif;
  line-height: 1.6;
  background-color: #f8fff9;
}

.landing-page {
  min-height: 100vh;
  background-color: #f8fff9;
  color: #005F40;
  display: flex;
  flex-direction: column;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 2rem;
  width: 100%;
}

.main-content {
  display: flex;
  flex-direction: column;
  flex: 1;
  position: relative;
  z-index: 1; /* Stellt sicher, dass der Inhalt über dem Hintergrund liegt */
}

/* Header-Styling */
.app-header {
  padding: 1rem 0;
  margin-bottom: 2rem;
  display: flex;
  flex-direction: column;
  position: relative;
}

.logo {
  font-size: 1.8rem;
  font-weight: 600;
  color: #26BB77;
  position: relative;
  margin-bottom: 5px;
}

.logo::after {
  content: '';
  position: absolute;
  bottom: -5px;
  left: 0;
  width: 100%;
  height: 2px;
  background: #26BB77;
  border-radius: 2px;
}

.site-name {
  position: absolute;
  top: 33px;
  right: 33px;
  font-size: 1rem;
  color: #2A7857;
}

/* Navigation */
.nav-tabs {
  width: 100%;
  margin: 40px 0 20px;
  display: flex;
  justify-content: center;
  gap: 15px;
}

.nav-tab {
  padding: 8px 24px;
  border-radius: 30px;
  font-weight: 600;
  font-size: 1rem;
  cursor: pointer;
  background: transparent;
  border: none;
  background-color: rgba(230, 245, 235, 0.7);
  color: #005F40;
  transition: all 0.3s ease;
}

.nav-tab.active {
  background-color: #26BB77;
  color: white;
}

.nav-tab:hover:not(.active) {
  background-color: #E6F5EB;
  transform: translateY(-2px);
}

/* Sections */
.section {
  position: relative;
  overflow: visible; /* Wichtig, damit der Inhalt sichtbar ist */
  margin-bottom: 3.5rem;
  border-radius: 20px;
  background-color: #FFFFFF;
  border: 2px solid rgba(38, 187, 119, 0.3);
  padding: 2rem;
  box-shadow: 0 8px 20px rgba(0, 95, 64, 0.1);
  z-index: 2; /* Höher als der Container */
  display: block; /* Explizit als Block anzeigen */
  visibility: visible; /* Explizit sichtbar setzen */
  opacity: 1; /* Volle Sichtbarkeit */
}

.section h2 {
  font-size: 2rem;
  font-weight: 800;
  margin-bottom: 2rem;
  position: relative;
  display: inline-block;
  color: #005F40; /* Textfarbe explizit setzen */
}

.section h2::after {
  content: '';
  position: absolute;
  width: 70%;
  height: 4px;
  bottom: -8px;
  left: 0;
  background: linear-gradient(to right, #26BB77, #17A2B8);
  border-radius: 4px;
}

.section p {
  font-size: 1.05rem;
  margin-bottom: 1.5rem;
  line-height: 1.8;
  color: #005F40; /* Textfarbe explizit setzen */
}

.highlighted {
  font-weight: 800;
  color: #26BB77;
  position: relative;
  display: inline-block;
  padding: 0 4px;
}

.highlighted::before {
  content: '';
  position: absolute;
  width: 100%;
  height: 30%;
  bottom: 0;
  left: 0;
  background-color: rgba(38, 187, 119, 0.15);
  z-index: -1;
  border-radius: 4px;
}

/* Content Blocks */
.content-blocks {
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: 2.5rem;
  margin-top: 2.5rem;
}

.content-block {
  flex: 1;
  position: relative;
  background-color: #E6F5EB;
  padding: 2rem;
  border-radius: 16px;
  border: 2px solid rgba(38, 187, 119, 0.2);
  box-shadow: 0 8px 20px rgba(0, 95, 64, 0.1);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  z-index: 2; /* Stellt sicher, dass die Blöcke sichtbar sind */
  min-width: 200px; /* Mindestbreite für kleine Bildschirme */
}

.content-block:nth-child(1) {
  transform: rotate(-2deg);
  border-top: 4px solid #26BB77;
}

.content-block:nth-child(2) {
  transform: rotate(2deg);
  border-top: 4px solid #17A2B8;
}

.content-block:hover {
  transform: rotate(0deg) translateY(-10px);
  box-shadow: 0 15px 30px rgba(38, 187, 119, 0.3);
  border-color: #26BB77;
}

.content-block h3 {
  font-size: 1.4rem;
  margin-bottom: 1rem;
  position: relative;
  display: inline-block;
  color: #005F40;
}

.content-block h3::after {
  content: '';
  position: absolute;
  width: 100%;
  height: 3px;
  bottom: -5px;
  left: 0;
  border-radius: 3px;
}

.content-block:nth-child(1) h3::after {
  background: #26BB77;
}

.content-block:nth-child(2) h3::after {
  background: #17A2B8;
}

/* Responsive Design */
@media (max-width: 768px) {
  .content-blocks {
    flex-direction: column;
  }
  
  .nav-tabs {
    flex-wrap: wrap;
  }
}
</style>