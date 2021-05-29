<template>
    <div class="page-sucess"    >
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Gracias!</h1>

                <p>La orden está siendo procesada</p>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    name: 'Success',
    mounted() {
        document.title = 'Orden Realizada | DjanGoRide'
    },
}
</script>