<template>
    <div>
        <Link :href="route('dashboard')">Home</Link>
    </div>
</template>

<script>
import { Link } from '@inertiajs/vue3'
export default {
    components:{
        Link
    },
    setup () {


        return {}
    }
}
</script>

