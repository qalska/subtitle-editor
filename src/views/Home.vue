<template>
    <div>
        <div>
            <router-link tag="button" :to="'/create'">Create new subtitle</router-link>
        </div>
        <Table />
    </div>
</template>

<script>
import Table from '@/components/Table'
export default {
    components: {
        Table,
    },
}
</script>