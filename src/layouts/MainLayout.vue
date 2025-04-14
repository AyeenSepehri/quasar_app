<!--<template>-->
<!--  <q-layout view="lHh Lpr lFf">-->
<!--    <q-header elevated class="bg-primary text-white">-->
<!--      <q-toolbar>-->
<!--        <q-toolbar-title>داشبورد خودرو</q-toolbar-title>-->
<!--      </q-toolbar>-->
<!--    </q-header>-->

<!--    <q-page-container>-->
<!--      <router-view />-->
<!--    </q-page-container>-->
<!--  </q-layout>-->
<!--</template>-->
<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated class="bg-primary text-white">
      <q-toolbar>

        <q-toolbar-title>
          داشبورد خودرو
        </q-toolbar-title>

        <q-btn
          flat
          dense
          label="داشبورد"
          to="/dashboard"
          class="q-ml-sm text-white"
        />

        <q-btn
          flat
          dense
          label="نقشه"
          to="/map"
          class="q-ml-sm text-white"
        />

      </q-toolbar>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>
