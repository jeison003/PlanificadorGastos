<script setup>
    import {ref} from 'vue'
    import Alerta from './Alerta.vue';
    
    const presupuesto = ref(0)
    const error = ref('')

    const emit = defineEmits(['definir-presupuesto'])

    const definirPresupuesto = ()=>{
        if(presupuesto.value <= 0 || presupuesto.value === ''){
            error.value = 'Numero no permitido'

            setTimeout(() => {
                error.value = ''
            }, 2000);
            return
        }
        emit('definir-presupuesto', presupuesto.value)
    }
    
</script>

<template>
    <form action="" class="presupuesto" 
        @submit.prevent="definirPresupuesto">
        
        <!-- Si un componente tiene cierre y apertura de etiqueta, es que es un slot -->
        <Alerta v-if="error">
            {{error}}
        </Alerta>
        <div class="campo">

            <label for="">Definir Presupuesto</label>
            
            <input 
                v-model.number="presupuesto" 
                id="nuevo-presupuesto" 
                class="nuevo-presupuesto"
                placeholder="Añade tu presupuesto"
                min="0"
                type="number">
        </div>
        <input type="submit" value="Definir Presupuesto">
    </form>
</template>


<style  scoped>
    .presupuesto{
        width: 100%;
    }
    .campo{
        display: grid;
        gap: 2rem;
    }
    .presupuesto label {
        font-size: 2.8rem;
        text-align: center;
        color: var(--azul);
    }
    .presupuesto input[type="number"]{
        background-color: var(--gris-claro);
        border-radius: 1rem;
        padding: 1rem;
        border: none;
        font-size: 2.2rem;
        text-align: center;
    }
    .presupuesto input[type="submit"]{
        background-color: var(--azul);
        border-radius: 1rem;
        padding: 1rem;
        border: none;
        font-size: 2rem;
        margin-top: 2rem;
        text-align: center;
        color: var(--blanco);
        font-weight: 900;
        width: 100%;
        transition: background-color 300ms ease;
    }
    .presupuesto input[type="submit"]:hover{
        background-color: #1048A4;
        cursor: pointer;
    }
</style>