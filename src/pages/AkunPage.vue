<template>
    <div class="mt-4">
        <div class="d-flex justify-content-between">
            <h1>Akun page</h1>
            <modal-component :title-button="titleButton" :title-modal="titleModal">
                <form-akun @create-data="CreateData"></form-akun>
            </modal-component>
        </div>

        <div class="mt-3">
            <table-component :items="dataAkun" :fields="fieldsTable"></table-component>
        </div>
    </div>
</template>

<script>
import ModalComponent from '@/components/ModalComp.vue'
import TableComponent from '@/components/TableComp.vue'
import FormAkun from '@/components/FormAkun.vue'

export default {
    name: 'akun-page',
    data: function () {
        return {
            titleButton: '+ Create',
            titleModal: 'Create Account',
            dataAkun: [],
            fieldsTable: ['username', 'password']
        }
    },
    methods: {
        CreateData: function (form) {
            let id = 1
            let countId = this.dataAkun.length
            if (countId > 0) {
                id = this.dataAkun[countId - 1].id + 1
            }
            form.id = id
            this.dataAkun.push(form)
        }
    },
    components: {
        ModalComponent,
        FormAkun,
        TableComponent
    }
}
</script>