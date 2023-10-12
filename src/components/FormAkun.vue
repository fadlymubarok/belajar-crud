<template>
    <div>
        <b-form @submit.prevent="onSubmit">
            <b-form-group id="label-username" label="Username" class="mb-3">
                <div class="d-flex gap-1">
                    <input type="hidden" value="form.id">
                    <b-form-select :options="optionMahasiswa" v-model="form.idMahasiswa" @change="changeIdMahasiswa($event)"
                        class="form-select w-50"></b-form-select>
                    <b-form-input id="input-username" v-model="form.username" type="text"
                        :disabled="disabledInput"></b-form-input>
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
    emits: ['create-data'],
    inject: ['closeModal'],
    data: function () {
        return {
            optionMahasiswa: [
                { value: '', text: '-- Pilih Mahasiswa --' }
            ],
            form: {
                id: '',
                idMahasiswa: '',
                username: '',
                password: ''
            },
            typePassword: 'password',
            btnPassword: 'eye-slash',
            disabledInput: true
        }
    },
    methods: {
        onSubmit: function () {
            this.$emit('create-data', this.form)
            this.closeModal()
        },
        changeIdMahasiswa: function (e) {
            let firstMhs = '';
            let isFalse = true
            if (parseInt(e) >= 0) {
                isFalse = false
                firstMhs = this.optionMahasiswa[e]
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
        }
    },
    created: function () {
        this.$globalData.dataMahasiswa.forEach(data => {
            let newData = { value: data.id, text: data.nama_lengkap }
            this.optionMahasiswa.push(newData)
        });
    },
}
</script>