<script setup>
    import { ref, computed } from 'vue'; 
    import Alerta from './Alerta.vue';
    import  cerrarModal from '../assets/img/cerrar.svg'

    const error = ref('')
    const emit = defineEmits(['ocultar-modal','guardar-gasto','update:nombre','update:cantidad','update:categoria', 'eliminar-gasto'])

    const props = defineProps({
        modal:{
            type: Object,
            required: true
        },
        nombre:{
            type: String,
            required: true
        },
        cantidad:{
            type: [String, Number],
            required: true
        },
        categoria:{
            type: String,
            required: true
        },
        disponible:{
            type: Number,
            required: true
        },
        id:{
            type: [String,null],
            required: true
        },
    })
    const oldCantidad = props.cantidad;

    const agregarGasto = ()=>{
        const {nombre,cantidad,categoria, disponible, id} = props
        if([nombre,cantidad,categoria, disponible].includes('')){
            error.value = 'Todos los campos son obligatorios'
            setTimeout(() => {
                error.value =''
            }, 3000);
            return
        }
        if(cantidad <=0){
            error.value = 'Cantidad no permitida'
            setTimeout(() => {
                error.value =''
            }, 3000);
            return
        }
        //Verficar el dato ya diligenciado
        if(id){
            if(cantidad > oldCantidad+disponible){
                error.value = 'Has excedido el presupuesto'
                setTimeout(() => {
                    error.value =''
                }, 3000);
                return
            }
        }else{
            //Validar que no gaste mas de los disponible
            if(cantidad > disponible){
                error.value = 'Has excedido el presupuesto'
                setTimeout(() => {
                    error.value =''
                }, 3000);
                return
            }
        }
       
        emit('guardar-gasto')
    }
  const isEditing = computed(()=>{
    return props.id
  })
</script>

<template>
    <div class="modal">
        <div class="cerrar-modal">
            <img 
            :src="cerrarModal"
            @click="$event=>$emit('ocultar-modal')"
             >
        </div>

        <div class="contenedor contenedor-formulario"
            :class="[modal.animar ? 'animar': 'cerrar']">
            <form 
                class="nuevo-gasto"
                @submit.prevent="agregarGasto"
            >
                <legend>{{ isEditing ? 'Guardar Cambios': 'Añadir gastos' }}</legend>
                <Alerta v-if="error">{{ error }}</Alerta>
                <div class="campo">
                    <label for="nombre">Nombre Gasto:</label>
                    <input type="text"
                    placeholder="Añade nombre del gasto"
                    id="nombre"
                    :value="nombre"
                    @input="$event=>$emit('update:nombre', $event.target.value)"
                    >
                </div>

                
                <div class="campo">

                    <label for="cantidad">Cantidad:</label>
                    <input type="number"
                    placeholder="Añade la cantidad"
                    id="cantidad"
                    :value="cantidad"
                    @input="$event=>$emit('update:cantidad', +$event.target.value)"
                    >
                    
                </div>

                <div class="campo">
                    <label for="categoria">Categoria:</label>
                    <select 
                        id="categoria"
                        :value="categoria"
                        @input="$event=>$emit('update:categoria', $event.target.value)"
                        >
                        <option value=""> Seleccione --</option>
                        <option value="ahorro"> Ahorro</option>
                        <option value="comida"> Comida</option>
                        <option value="casa"> Casa</option>
                        <option value="gastos"> Gastos varios</option>
                        <option value="ocio"> Ocio</option>
                        <option value="salud"> Salud</option>
                        <option value="suscripciones"> Suscripciones</option>
                    </select>
                </div>
                
                <input 
                    type="submit"
                    :value="isEditing ? 'Guardar': 'Crear' "
                >
            </form>
            <button
                v-if="isEditing"
                type="button"
                class="btn-eliminar"
                @click="$event=>$emit('eliminar-gasto')"
            >
                Eliminar
            </button>
        </div>
    </div>
</template>



<style scoped>
    .modal{
        position: absolute;
        background-color: rgb(0 0 0 /0.8);
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
    }
    .cerrar-modal{
        position: absolute;
        right: 3rem;
        top: 3rem;
    }
    .cerrar-modal img{
        width: 3rem;
        cursor: pointer;
    }
    .contenedor-formulario{
        transition-property: all;
        transition-duration: 300ms;
        transition-timing-function: ease-in;
        opacity: 0;
    }
    .contenedor-formulario.animar{
        opacity: 1;
    }
    .contenedor-formulario.cerrar{
        opacity: 0;
    }
    .nuevo-gasto{
        margin: 10rem auto 0 auto;
        display: grid;
        gap: 2rem;
    }
    .nuevo-gasto legend{
        text-align: center;
        color: var(--blanco);
        font-size: 3rem;
        font-weight: 700;

    }
    .campo{
        display: grid;
        gap: 2rem;
    }
    .nuevo-gasto input, .nuevo-gasto select{
        background-color: var(--gris-claro);
        border-radius: 1rem;
        padding: 1rem;
        border: none;
        font-size: 2.2rem;
    }
    .nuevo-gasto label{
        font-size: 3rem;
        color: var(--blanco);
    }
    .nuevo-gasto input[type="submit"]{
        background-color: var(--azul);
        color: var(--blanco);
        font-weight: 700;
        cursor: pointer;
    }
    .btn-eliminar{
        padding: 1rem;
        width: 100%;
        background-color: #ef4444;
        font-weight: 700;
        font-size: 2.2rem;
        color: var(--blanco);
        margin-top: 10rem;
        cursor: pointer;
        border: none;
        border-radius: 1rem;
    }
   @media(max-width:600px){
        .nuevo-gasto input, .nuevo-gasto select, option{
            font-size: 1.5rem !important;
        }
        .nuevo-gasto label{
            font-size: 1.8rem !important;
        } 
        option{
            font-size: 1.3rem !important;
        }  
   }
</style>