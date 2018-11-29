<!-- Template principal que se va a renderizar en el index -->
<template>
    <div class="row">
        <div class="col-lg-12">
            <h1 class="col-lg-12">Billetera Virtual</h1>
            <money class="col-lg-6"></money>
            <wallet class="col-lg-6 text-right"></wallet>
        </div>
    </div>
</template>

<script>
import money from "./components/Money.vue";
import wallet from "./components/Wallet.vue";

export default {
  name: "app",
  // Cargamos los componentes de la aplicacion
  components: {
    money,
    wallet
  }
};
</script>

<style>

</style>
