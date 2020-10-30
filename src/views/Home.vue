<template>
<div class="home">
    <h1>Cuenta regresiva</h1>
    <h3>{{minutos}}:{{segundos}} min/seg</h3>
    <button v-for="(boton,indice) in botones" v-bind:key="indice" @click="inicio(boton.time)">{{boton.texto}}</button>

</div>
</template>

<script>
export default {
    name: 'Home',
    components: {

    },
    data() {
        return {
            intervalo: null,
            objetivo: null,
            minutos: "00",
            segundos: "00",
            botones: [{
                    texto: "3s",
                    time: 1000 * 3
                },
                {
                    texto: "1m",
                    time: 1000 * 60
                },
                {
                    texto: "5m",
                    time: 1000 * 60 * 5
                },
                {
                    texto: "10m",
                    time: 1000 * 60 * 10
                },
                {
                    texto: "30m",
                    time: 1000 * 60 * 30
                },
            ]
        }
    },
    methods: {
        inicio(time) {
            var that = this;
            // si ya existe el intervalo debemos detenerlo
            if (that.intervalo != null) {
                clearInterval(that.intervalo);
                that.intervalo = null;
            }

            //deifnir fecha a la que se tiene que llegar: ahora + milisegundos en variable time
            var fechatemporal = new Date();
            that.objetivo = fechatemporal.setTime(fechatemporal.getTime() + time);

            //generar un intervalo: funcion que se ejecuta cada un segundo
            that.actualizarTiempo();
            setTimeout(() => {
                that.intervalo = setInterval(that.actualizarTiempo, 1000);
            },100);
        },
        actualizarTiempo() {
            var that = this;
            //dentro de intervalo calcular la diferencia entre el nuevo ahora y la fecha definida en el paso 1
            var ahora = new Date().getTime();
            var diferencia = that.objetivo - ahora;
            var minutosTemp = Math.floor((diferencia % (1000 * 60 * 60)) / (1000 * 60));
            var segundosTemp = Math.floor((diferencia % (1000 * 60)) / 1000);
            that.minutos = minutosTemp > 0 && minutosTemp < 10 ? "0" + minutosTemp : minutosTemp <= 0 ? "00" : minutosTemp;
            that.segundos = segundosTemp > 0 && segundosTemp < 10 ? "0" + segundosTemp : segundosTemp <= 0 ? "00" : segundosTemp;
            if (diferencia <= 0) {
                clearInterval(that.intervalo);
                that.intervalo = null;
            }
        }
    }
}
</script>
