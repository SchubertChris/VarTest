<!-- src/pages/member/Dashboard.vue (Aktualisiert) -->
<template>
  <MemberLayout 
    :page-title="getPageTitle()" 
    :active-menu="activeMenu"
    @menu-change="changeMenu"
  >
    <!-- Dynamische Komponente basierend auf aktivem Menü -->
    <component :is="currentComponent" />
  </MemberLayout>
</template>

<script lang="ts">
import { defineComponent, ref, computed, onMounted, markRaw, PropType } from 'vue';
import { useRouter, useRoute } from 'vue-router';
import MemberLayout from '@/components/layout/MemberLayout.vue';

// Dashboard-Unterkomponenten
import Overview from '@/components/member/dashboard/Overview.vue';
import MyArticles from '@/components/member/dashboard/MyArticles.vue';
import Favorites from '@/components/member/dashboard/Favorites.vue';
import Friends from '@/components/member/dashboard/Friends.vue';
import Notifications from '@/components/member/dashboard/Notifications.vue';
import Settings from '@/components/member/dashboard/Settings.vue';
import Library from '@/components/member/dashboard/Library.vue'; // Neue Bibliothek-Komponente

// Auth-Service für Benutzerüberprüfung
import { authService } from '@/services/auth.service';

export default defineComponent({
  name: 'MemberDashboard',
  components: {
    MemberLayout,
    Overview,
    MyArticles,
    Favorites,
    Friends,
    Notifications,
    Settings,
    Library // Neue Komponente registrieren
  },
  props: {
    // Standardtab aus der Routerkonfiguration
    defaultTab: {
      type: String as PropType<string>,
      default: 'overview'
    }
  },
  setup(props) {
    const router = useRouter();
    const route = useRoute();
    
    // Aktiver Menüpunkt
    const activeMenu = ref(props.defaultTab || 'overview');
    
    // Komponenten-Mapping
    const componentMap = markRaw({
      'overview': Overview,
      'my-articles': MyArticles,
      'favorites': Favorites,
      'friends': Friends,
      'notifications': Notifications,
      'settings': Settings,
      'library': Library // Neue Komponente im Mapping
    });
    
    // Aktuelle Komponente basierend auf Menüauswahl
    const currentComponent = computed(() => {
      return componentMap[activeMenu.value as keyof typeof componentMap] || Overview;
    });
    
    // Seitentitel basierend auf aktivem Menü
    const getPageTitle = () => {
      switch (activeMenu.value) {
        case 'overview':
          return 'Dashboard';
        case 'my-articles':
          return 'Meine Artikel';
        case 'favorites':
          return 'Favoriten';
        case 'friends':
          return 'Freunde';
        case 'notifications':
          return 'Benachrichtigungen';
        case 'settings':
          return 'Einstellungen';
        case 'library':
          return 'Bibliothek';
        default:
          return 'Dashboard';
      }
    };
    
    // Menü ändern
    const changeMenu = (menuItem: string) => {
      activeMenu.value = menuItem;
      
      // URL aktualisieren ohne Neuladen
      const query = { ...route.query, tab: menuItem };
      router.replace({ query });
    };
    
    // Initialisierung
    onMounted(() => {
      // Authentifizierung überprüfen (hier wird die authService.isLoggedIn() Methode verwendet, 
      // die in der Router-Konfiguration verwendet wird)
      if (!authService.isLoggedIn()) {
        router.push('/login-register');
        return;
      }
      
      // Tab aus URL-Query-Parameter laden, falls vorhanden
      const tabParam = route.query.tab as string;
      if (tabParam && Object.keys(componentMap).includes(tabParam)) {
        activeMenu.value = tabParam;
      } else if (props.defaultTab && Object.keys(componentMap).includes(props.defaultTab)) {
        // Ansonsten den defaultTab verwenden, falls gültig
        activeMenu.value = props.defaultTab;
      }
    });
    
    return {
      activeMenu,
      currentComponent,
      getPageTitle,
      changeMenu
    };
  }
});
</script>