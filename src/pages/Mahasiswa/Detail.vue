<template>
    <div>
        <div class="col mb-3">
            <span class="fw-bold">Nama: </span>
            <span>{{ dataDetail.nama_lengkap }}</span>
        </div>
        <div class="col mb-3">
            <span class="fw-bold">Umur: </span>
            <span>{{ dataDetail.umur }} Tahun</span>
        </div>
        <div class="col mb-3">
            <span class="fw-bold">Jurusan: </span>
            <span>{{ dataDetail.jurusan }}</span>
        </div>

        <div class="d-flex justify-content-end gap-1">
            <b-button variant="warning" @click="formEdit">Edit</b-button>
            <b-button variant="danger" @click="deleteData(dataDetail.id)">Delete</b-button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'detail-view',
    emits: ['form-edit', 'delete-data'],
    props: {
        dataDetail: {
            type: Object,
            required: false
        }
    },
    methods: {
        formEdit: function () {
            this.$emit('form-edit')
        },
        deleteData: function (id) {
            this.$emit('delete-data', id)
        }
    }
}
</script>