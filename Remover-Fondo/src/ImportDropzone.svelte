<script lang="ts">
    import { Cloudinary } from '@cloudinary/url-gen'
    import { backgroundRemoval } from '@cloudinary/url-gen/actions/effect'

    import { ImageStatus } from './types.d'
    import{ imageStatus, modifiedImage, originalImage } from './store'
    import Dropzone from 'dropzone'
    import 'dropzone/dist/dropzone.css'

    import { onMount } from 'svelte'

    const cloudinary = new Cloudinary({
        cloud:{
            cloudName: 'alexisdam',
        },
        url:{
            secure: true
        }
    })

    onMount(() => {
        const dropzone = new Dropzone('#dropzone', {
            uploadMultiple: false,
            acceptedFiles: '.jpg,.png,.webp',
            maxFiles: 1 ,
        })

        dropzone.on('sending', (file, xhr, formData) =>{
            imageStatus.set(ImageStatus.UPLOADING)
             
            formData.append('upload_preset', 'n9oqt0fs')
            formData.append('timestamp', Date.now() / 1000)
            formData.append('api_key', 358112691499629)
        })

        dropzone.on('success', (file, response) =>{
            const { public_id: publicId, secure_url: url} = response


            const imageWithoutBackground = cloudinary
            .image(publicId)
            .effect(backgroundRemoval())

            console.log(imageWithoutBackground.toURL());
            
            imageStatus.set(ImageStatus.DONE)
            modifiedImage.set(imageWithoutBackground.toURL())
            originalImage.set(url)

        })

        dropzone.on('error', (file, response) =>{
            console.log('Mal')
            console.log(response)
        })
    })
    
</script>


<form id="dropzone" 
    class="shadow-2xl border-dashed border-2 border-gray-300 rounded-lg
    aspect-video w-full flex items-center justify-center flex-col"
    action="https://api.cloudinary.com/v1_1/alexisdam/image/upload"
    >

    {#if $imageStatus === ImageStatus.READY }
        <button class="pointer-events-none bg-blue-400 rounded-full text-bold text-white text-xl
        px-6 py-4 font-medium">
        Upload Image    
        </button>

        <p class="text-xl mt-4 text-gray-800 italic font-medium">Or drop a file</p>
    {/if}

    {#if $imageStatus === ImageStatus.UPLOADING}
    <p class="text-xl mt-4 text-gray-800">Uploading files...</p>

        
    {/if}
</form>
