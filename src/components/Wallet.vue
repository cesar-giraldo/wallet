<!-- Plantilla del Componente Wallet -->
<template>
    <div>
        <h2><small>Total Dinero: </small> $ {{moneyFormat(total)}}</h2>
        <h3><small># Billetes: </small> {{numberBills}}</h3>
    </div>
</template>

<script>
export default {
    data() {
        return {
            // Monto total de dinero en la billetera
            total: 0,
            // Numero total de Billetes
            numberBills: 0
        }
    },
    created() {
        // Escucha para el evento de cargar los totales
        this.$root.$data.eventBus.$on('load', (money) => {
            money.forEach((bill) => {
                if (bill.amount > 0) {
                    this.total += bill.total;
                    this.numberBills += bill.amount;
                }
            });
        });
        // Escucha para el evento de adicionar un billete
        this.$root.$data.eventBus.$on('add', (bill) => {
            if (this.total >= 0) {
                this.total += bill.denomination;
                this.numberBills +=1;
            }
        });
        // Escucha para el evento de remover un billete
        this.$root.$data.eventBus.$on('remove', (bill) => {
            if (this.total >= bill.denomination) {
                this.total -= bill.denomination;
                this.numberBills -=1;
            }
        });
    },
    methods: {
        // Función para formatear un numero en formato moneda
        moneyFormat(value) {
            let val = (value/1).toFixed(0).replace('.', ',');
            return val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".");
        }
    }
    
}
</script>

<style>

</style>
