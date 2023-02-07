<template>
  <q-layout view="lHh Lpr lFf">
    <q-header>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer"
        />

        <q-toolbar-title> Password BOT </q-toolbar-title>

        <div>Quasar v{{ $q.version }}</div>
      </q-toolbar>
      <div class="q-px-lg q-pt-xl q-mb-md">
        <div class="text-h3">Password Bot</div>
        <div class="text-subtitle1">{{ todaysDate() }}</div>
      </div>
      <q-img class="header-image absolute-top" src="~/assets/banner.png" />
    </q-header>
    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      :width="300"
      :breakpoint="600"
      dark
    >
      <q-scroll-area
        style="
          height: calc(100% - 192px);
          margin-top: 192px;
          border-right: 1px solid #ddd;
        "
      >
        <q-list padding>
          <EssentialLink
            v-for="link in essentialLinks"
            :key="link.title"
            v-bind="link"
          />
        </q-list>
      </q-scroll-area>

      <q-img
        class="drawer-image absolute-top"
        src="~/assets/drawer.png"
        style="height: 192px"
      >
        <div class="drawer-avatar-box absolute-center bg-transparent">
          <q-avatar size="72px" class="q-mb-md drawer-avatar">
            <img src="~/assets/avatar.png" />
          </q-avatar>
          <div class="text-weight-bold text-h5 q-mb-none">Jolly Robot</div>
          <div class="text-h6 text-weight-bold q-mt-none">@huement</div>
        </div>
      </q-img>
    </q-drawer>

    <q-page-container>
      <keep-alive>
        <router-view />
      </keep-alive>
    </q-page-container>
  </q-layout>
</template>

<script>
import { defineComponent, ref } from "vue";
import EssentialLink from "components/EssentialLink.vue";
import { date } from "quasar";

const linksList = [
  {
    title: "Generate",
    caption: "password builder",
    icon: "lock",
    link: "/",
  },
  {
    title: "Check Up",
    caption: "security recommendations",
    icon: "task_alt",
    link: "/tasks",
  },
  {
    title: "Hackability Index",
    caption: "cracking estimation chart",
    icon: "score",
    link: "/hack-chart",
  },
  {
    title: "Mnemonics",
    caption: "recollection assistance",
    icon: "psychology",
    link: "/mnemonics",
  },
  {
    title: "About & Reach Out",
    caption: "all about @password-bot",
    icon: "public",
    link: "/hello",
  },
];

export default defineComponent({
  name: "MainLayout",

  components: {
    EssentialLink,
  },

  setup() {
    const leftDrawerOpen = ref(false);

    return {
      essentialLinks: linksList,
      leftDrawerOpen,
      toggleLeftDrawer() {
        leftDrawerOpen.value = !leftDrawerOpen.value;
      },
      todaysDate() {
        const timeStamp = Date.now();
        const formattedString = date.formatDate(timeStamp, "MMM Do YYYY");
        return formattedString;
      },
    };
  },
});
</script>

<style lang="scss">
.header-image {
  height: 100%;
  z-index: -1;
  opacity: 0.5;
  background-size: cover;
}

.drawer-image {
  background-size: contain;
  background-color: $primary;
}

.drawer-avatar-box {
  min-width: 180px;
  text-align: center;
  opacity: 1;
}

.drawer-avatar {
  margin: 0 auto;
  display: block;
  width: 72px;
  height: 72px;
  border-radius: 0px;
}

.q-drawer {
  .text-caption {
    color: #f0f0f0;
  }
}

.q-item.q-router-link--active {
  color: #4afbc3 !important;
  font-weight: bold !important;
}
</style>
