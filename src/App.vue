<template>
  <component :is="layout">
    <router-view />
  </component>
</template>

<script lang="ts">
import { defineComponent, ref, computed } from "vue";
import { useRouter } from "vue-router";

const defaultLayout = "default";
import axios from 'axios';

export default {
  computed:{
    layout(){
        const { currentRoute } = useRouter();
        return `${currentRoute.value.meta.layout || defaultLayout}-layout`;
    }
  },
};
</script>
