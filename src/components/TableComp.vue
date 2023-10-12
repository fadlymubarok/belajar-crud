<template>
    <div>
        <b-table responsive striped hover :items="items" :fields="fields" show-empty>
            <!-- <template #cell(id)="data">
                {{ data.index + 1 }}
            </template> -->
        </b-table>
    </div>
</template>

<script>
export default {
    name: 'table-component',
    props: {
        'items': {
            type: Array,
            required: true
        },
        'fields': {
            type: Array
        }
    },
}
</script>