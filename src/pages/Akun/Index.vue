<template>
    <div class="mt-4">
        <div class="d-flex justify-content-between">
            <h1>Akun page</h1>
            <b-button class="btn-sm my-3" variant="primary" @click="showPopupCreate">+ Create</b-button>
        </div>

        <div class="mt-3">
            <b-table responsive stripped :items="dataAkun" :fields="fields" show-empty>
                <template #head(id)>
                    No
                </template>
                <template #cell(username)="data">
                    <b-button variant="link" @click="showPopUpDetail(data.item)" class="m-0 p-0">{{ data.item.username
                    }}</b-button>
                </template>
            </b-table>
        </div>

        <!-- Modal -->
        <b-modal v-model="showPopUp" :title="titlePopup" hide-footer>
            <div v-if="titlePopup.toLowerCase().includes('create')">
                <input-view :data-akun="dataAkun" @create-data="createUpdateData" @close-modal="closeModal"></input-view>
            </div>
            <div v-else-if="titlePopup.toLowerCase().includes('edit')">
                <input-view :data-akun="dataAkun" :form-data="dataDetail" @create-data="createUpdateData"
                    @close-modal="closeModal"></input-view>
            </div>
            <div v-else>
                <detail-view :data-detail="dataDetail" @form-edit="formEdit" @delete-data="deleteData"></detail-view>
            </div>
        </b-modal>
    </div>
</template>

<script>
import InputView from '@/pages/Akun/Input.vue'
import DetailView from '@/pages/Akun/Detail.vue'

export default {
    name: 'akun-page',
    components: { InputView, DetailView },
    data: function () {
        return {
            titlePopup: 'Form Create',
            dataAkun: [],
            fields: ['id', 'username', 'name'],
            dataDetail: {},
            showPopUp: false
        }
    },
    methods: {
        createUpdateData: function (form) {
            if (form.id) {
                this.updateData(form)
            } else {
                this.createData(form)
            }
        },
        createData: function (form) {
            let id = 1
            let countId = this.dataAkun.length
            if (countId > 0) {
                id = countId + 1
            }
            form.id = id
            this.dataAkun.push(form)
        },
        updateData: function (form) {
            let data = this.dataAkun[form.id - 1]
            data = form;
            this.$set(this.dataAkun, data.id - 1, data)
        },
        showPopupCreate: function () {
            this.showPopUp = true;
            this.titlePopup = 'Form Create'
        },
        showPopUpDetail: function (data) {
            this.dataDetail = data;
            this.showPopUp = true;
            this.titlePopup = 'Detail';
        },
        formEdit: function () {
            this.titlePopup = 'Edit Form'
        },
        deleteData: function (id) {
            let indexForDelete = this.dataAkun.findIndex(mhs => mhs.id == id);
            this.dataAkun.splice(indexForDelete, 1)
            this.showPopUp = false
        },
        closeModal: function () {
            this.showPopUp = false
        }
    }
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