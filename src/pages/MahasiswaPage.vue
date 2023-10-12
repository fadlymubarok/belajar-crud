<template>
    <div class="mt-4">
        <div class="d-flex justify-content-between">
            <h1>Data Mahasiswa</h1>
            <modal-component :titleButton="titleButtonModal" :titleModal="titleModal">
                <form-mahasiswa @submitForm="CreateData"></form-mahasiswa>
            </modal-component>
        </div>

        <div class="mt-3">
            <table-component :items="dataMahasiswa" :fields="fieldsTable"></table-component>
        </div>
    </div>
</template>

<script>
import ModalComponent from "@/components/ModalComp.vue";
import TableComponent from "@/components/TableComp.vue";
import FormMahasiswa from "@/components/FormMahasiswa.vue";
import Vue from 'vue';


export default {
    name: 'mahasiswa-page',
    data: function () {
        return {
            'titleButtonModal': '+ Create',
            'titleModal': 'Form Create',
            'dataMahasiswa': [
                { id: 1, 'nama_lengkap': 'fadly Mubarok', 'umur': 19, 'jurusan': 'RPL' },
                { id: 2, 'nama_lengkap': 'Ucup Sholehah', 'umur': 11, 'jurusan': 'RPL' },
                { id: 3, 'nama_lengkap': 'Nande', 'umur': 12, 'jurusan': 'TKJ' }
            ],
            'fieldsTable': [
                'nama_lengkap', 'umur', 'jurusan'
            ]
        }
    },
    mounted() {
        Vue.prototype.$globalData = {
            dataMahasiswa: this.dataMahasiswa,
        };
    },
    methods: {
        CreateData: function (dataform) {
            let id = 1
            if (this.dataMahasiswa.length > 0) {
                id = this.dataMahasiswa[this.dataMahasiswa.length - 1].id + 1
            }
            dataform.id = id
            this.dataMahasiswa.push(dataform)
        }
    },
    components: {
        ModalComponent,
        TableComponent,
        FormMahasiswa
    },
}
</script>