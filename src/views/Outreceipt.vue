<template>
  <v-app id="outp.cloveropen.com">
    <Basepage />
    <v-container fluid fill-height>
      <v-layout justify-start>
        <outreceipt />
      </v-layout>
    </v-container>
  </v-app>
</template>

<script>
import Basepage from "../components/Basepage";
import outreceipt from "../components/OutreceiptC";
export default {
  components: {
    Basepage,
    outreceipt
  }
};
</script>
