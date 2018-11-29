<!-- Plantilla del Money -->
<template>
    <div>
        <table v-if="money" class="table table-bordered">
            <thead>
                <th width="200px">Denominación</th>
                <th>Opciones</th>
                <th>Cant.</th>
                <th width="160px">Total</th>
            </thead>
            <tbody>
                <!-- Listamos las diferentes denominaciones de Billetes -->
                <tr v-for="bill in money">
                    <td>$ {{moneyFormat(bill.denomination)}}</td>
                    <td>
                        <button @click="addToWallet(bill);" class="btn btn-sm btn-primary">+</button>
                        <button @click="removeFromWallet(bill);" class="btn btn-sm btn-danger">-</button>
                    </td>
                    <td>{{bill.amount}}</td>
                    <td>$ {{moneyFormat(bill.total)}}</td>
                </tr>
            </tbody>
        </table>
        <span v-else>Cargando Billetera...</span>
    </div>
</template>

<script>

import axios from 'axios';

export default {
    mounted() {
        console.log('Instancia montada');
        this.loadMoney();
    },
    data() {
        return {
            // Listado de billetes
            money : [], 
        }
    },
    methods: {
        loadMoney(){
            axios.get('http://localhost:3000/wallet').then((resp) => {
                this.money = resp.data.data;
                this.$root.$data.eventBus.$emit('load', this.money);
            });
        },
        // Funcion para aumentar la cantidad de un billete especifico
        addToWallet(bill) {
            bill.amount += 1;
            bill.total += bill.denomination;

            this.updateBill(bill);

            // Se dispara un evento persolanizado para leerlo en otro componente
            this.$root.$data.eventBus.$emit('add', bill);
            
        },
        // Funcion para reducir la cantidad de un billete especifico
        removeFromWallet(bill) {
            if (bill.amount > 0) {
                bill.amount -= 1;
                bill.total -= bill.denomination;

                this.updateBill(bill);

                // Se dispara un evento persolanizado para leerlo en otro componente
                this.$root.$data.eventBus.$emit('remove', bill);
            } else {
                alert('No hay más billetes de esta denominación');
            }
            
        },
        // Función para formatear un numero en formato moneda
        moneyFormat(value) {
            let val = (value/1).toFixed(0).replace('.', ',');
            return val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".");
        },
        updateBill(bill) {
            // Enviamos a modificar el registro en la API
            axios.post('http://localhost:3000/wallet/update', {
                denomination: bill.denomination,
                amount: bill.amount,
                total: bill.total
            })
            .then(function (response) {
                console.log('Billetera Actualizada');
            })
            .catch(function (error) {
                alert('Ocurrió un error actualizando la billetera');
            });
        }
    }
}
</script>

<style>

</style>
