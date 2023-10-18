<template>
    <div class="mt-4">
        <div class="d-flex justify-content-between">
            <h1>Data Mahasiswa</h1>
            <b-button class="btn-sm my-3" variant="primary" @click="showPopUpCreate">+ Create</b-button>
        </div>

        <div class="mt-3">
            <b-table responsive stripped :items="dataMahasiswa" :fields="fields" show-empty>
                <template #head(id)>
                    No
                </template>
                <template #cell(id)="data">
                    <span>{{ data.index + 1 }}</span>
                </template>
                <template #cell(nama_lengkap)="data">
                    <b-button variant="link" class="text-dark" @click="showPopUpDetail(data.item)">
                        {{ data.item.nama_lengkap }}
                    </b-button>
                </template>
                <template #cell(umur)="data">
                    <span>{{ data.item.umur }} Tahun</span>
                </template>
            </b-table>
            <div v-if="showPopUp">
                <b-modal v-model="showPopUp" id="detailPopUp" :title="titlePopup" hide-footer>
                    <div v-if="titlePopup.toLowerCase().includes('create')">
                        <input-view @submit-form="formData" @close-modal="CloseModal"></input-view>
                    </div>
                    <div v-else-if="titlePopup.toLowerCase().includes('edit')">
                        <input-view :data="dataDetail" @submit-form="formData" @close-modal="CloseModal"></input-view>
                    </div>
                    <div v-else>
                        <detail-view :data-detail="dataDetail" @form-edit="formEdit"
                            @delete-data="deleteData"></detail-view>
                    </div>
                </b-modal>
            </div>
        </div>
    </div>
</template>

<script>
import InputView from '@/pages/Mahasiswa/Input.vue'
import DetailView from '@/pages/Mahasiswa/Detail.vue'

import Vue from 'vue';

export default {
    name: 'mahasiswa-page',
    data: function () {
        return {
            'titleButtonModal': '+ Create',
            'titleModal': 'Form Create',
            'fields': ['id', 'nama_lengkap', 'jurusan'],
            'dataMahasiswa': [],
            'titlePopup': 'Details',
            'showPopUp': false,
            'dataDetail': {}
        }
    },
    mounted() {
        Vue.prototype.$globalData = {
            dataMahasiswa: this.dataMahasiswa,
        };
    },
    methods: {
        formData: function (dataform) {
            if (dataform.id) {
                this.updateData(dataform)
            }
            else {
                this.createData(dataform)
            }
        },
        createData: function (dataform) {
            let id = 1
            if (this.dataMahasiswa.length > 0) {
                id = this.dataMahasiswa.length + 1
            }
            dataform.id = id
            this.dataMahasiswa.push(dataform)
        },
        updateData: function (dataform) {
            let existMhs = this.dataMahasiswa[dataform.id - 1];
            existMhs = dataform
            this.$set(this.dataMahasiswa, existMhs.id - 1, existMhs)
        },
        deleteData: function (id) {
            let indexForDelete = this.dataMahasiswa.findIndex(mhs => mhs.id == id);
            this.dataMahasiswa.splice(indexForDelete, 1)
            this.showPopUp = false
        },
        showPopUpDetail: function (data) {
            this.titlePopup = 'detail'
            this.showPopUp = true
            this.dataDetail = data
        },
        showPopUpCreate: function () {
            this.titlePopup = 'Form Create'
            this.showPopUp = true
        },
        formEdit: function () {
            this.titlePopup = 'Form Edit'
        },
        CloseModal: function () {
            this.showPopUp = false
        }
    },
    components: {
        InputView,
        DetailView
    },
}
</script>

<style scoped>
:deep(.modal-backdrop) {
    opacity: 0.5 !important;
}

:deep(.close) {
    border: 0;
    background-color: transparent;
    font-size: 20px;
}
</style>