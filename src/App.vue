<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <v-toolbar-title>Decimal to Binary/Octal/Hex</v-toolbar-title>
      <v-spacer></v-spacer>

      <v-btn href="https://github.com/kavehcsharp/d2" target="_blank" text>
        <span class="mr-2">Github</span>
        <v-icon>mdi-github</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <D2BOH />
    </v-main>
  </v-app>
</template>

<script>
import D2BOH from "./components/D2BOH";

export default {
  name: "App",

  components: {
    D2BOH,
  },

  data: () => ({
    //
  }),
  created() {
    document.title = "Decimal to ?";
  },
};
</script>
