<template>
    <div>
        <b-form @submit.prevent="onSubmit">
            <div v-if="errors.length > 0" class="bg-danger p-2 mb-2">
                <ul class="m-0">
                    <li v-for="error in errors" :key="error" class="text-white">{{ error }}</li>
                </ul>
            </div>
            <b-form-group id="label-username" label="Username" class="mb-3">
                <div class="d-flex gap-1">
                    <b-form-select :options="optionMahasiswa" v-model="form.idMahasiswa"
                        @change="changeInputMahasiswa($event)" class="form-select w-50" v-if="!form.id"></b-form-select>
                    <b-form-input id="input-username" v-model="form.username" type="text" :disabled="disabledInput"
                        placeholder="Saran akun" @blur="validUsername"></b-form-input>
                </div>
            </b-form-group>

            <b-form-group id="label-password" label="Password" class="mb-3">
                <div class="d-flex justify-content-between gap-1">
                    <b-form-input id="input-password" v-model="form.password" :type="typePassword"
                        :disabled="disabledInput"></b-form-input>
                    <button type="button" class="btn btn-outline-none" @click="showInputPassword"><b-icon
                            :icon="btnPassword"></b-icon></button>
                </div>
            </b-form-group>

            <div class="d-flex justify-content-end gap-2">
                <button type="submit" class="btn btn-primary btn-sm">Submit</button>
                <button type="button" class="btn btn-secondary btn-sm" @click="closeModal">Close</button>
            </div>
        </b-form>
    </div>
</template>

<script>
export default {
    name: 'form-akun',
    emits: ['create-data', 'close-modal'],
    props: {
        dataAkun: {
            type: Array,
            required: true
        },
        formData: {
            type: Object,
            required: false
        }
    },
    data: function () {
        return {
            'optionMahasiswa': [
                { 'value': '', 'text': '-- Pilih Mahasiswa --' }
            ],
            'form': {
                'id': '',
                'idMahasiswa': '',
                'name': '',
                'username': '',
                'password': ''
            },
            'typePassword': 'password',
            'btnPassword': 'eye-slash',
            'disabledInput': true,
            'errors': []
        }
    },
    methods: {
        onSubmit: function () {
            if (this.validateInput(this.form)) {
                this.$emit('create-data', this.form)
                this.closeModal()
            }
        },
        validateInput: function (form) {
            this.errors = []
            if (!form.name.trim()) {
                this.errors.push('Nama harus diisi')
            }
            if (!form.username.trim()) {
                this.errors.push('Username harus diisi')
            }
            if (!form.password.trim()) {
                this.errors.push('Password harus diisi')
            }

            if (this.errors.length > 0) return false;
            return true;
        },
        closeModal: function () {
            this.$emit('close-modal')
        },
        changeInputMahasiswa: function (e) {
            let firstMhs = '';
            let isFalse = true
            if (parseInt(e) >= 0) {
                isFalse = false
                firstMhs = this.optionMahasiswa.find(opt => opt.value == e);
                this.form.name = firstMhs.text
                firstMhs = firstMhs.text.replace(/\s/g, '.').toLowerCase() + Math.floor(Math.random() * 1999)
            }
            this.disabledInput = isFalse
            this.form.username = firstMhs
        },
        showInputPassword: function () {
            if (this.typePassword == 'password') {
                this.typePassword = 'text'
            } else {
                this.typePassword = 'password'
            }

            if (this.btnPassword == 'eye-slash') {
                this.btnPassword = 'eye'
            } else {
                this.btnPassword = 'eye-slash'
            }
        },
        validUsername: function () {
            this.form.username = this.form.username.replace(/\s/g, '.').toLowerCase()
        }
    },
    created: function () {
        if (this.$globalData.dataMahasiswa) {
            this.$globalData.dataMahasiswa.forEach(data => {
                let newData = { value: data.id, text: data.nama_lengkap }
                this.optionMahasiswa.push(newData)
            });
        }

        if (this.dataAkun.length > 0) {
            this.dataAkun.forEach(data => {
                this.optionMahasiswa = this.optionMahasiswa.filter(opt => opt.value != data.idMahasiswa)
            });
        }

        if (this.optionMahasiswa.length == 1) {
            this.optionMahasiswa[0].text = '-- Data tidak tersedia --'
        }

        if (this.formData) {
            this.form.id = this.formData.id
            this.form.idMahasiswa = this.formData.idMahasiswa
            this.form.name = this.formData.name
            this.form.username = this.formData.username
            this.form.password = this.formData.password
            this.disabledInput = false
        }
    },
}
</script>