<script lang="ts">
    import 'two-up-element'
    import { originalImage, modifiedImage } from "./store";

    let processingImage = true
    let tries = 0
    let intervalId: any

    $:{
        if (processingImage){
            clearInterval(intervalId)
            intervalId = setInterval(()=>{
                tries++
            }, 500)
        }
    }
</script>


<two-up>
    <img src={$originalImage} alt="Imagen Original subida por el usuario">
    <img
     on:load={() => (processingImage = false)}
     on:error={() => (processingImage = true)}
     src={`${$modifiedImage}&t=${tries}`}
     alt="Imagen sin Fondo subida por el usuario"
    />
</two-up>

<a
 download
 href={$modifiedImage} 
 class="block bg-gray-600 hover:bg-gray-500 text-l text-center w-full font-bold text-white rounded-full px-4 py-4 mt-10">
    Descargar imagen sin fondo
</a>