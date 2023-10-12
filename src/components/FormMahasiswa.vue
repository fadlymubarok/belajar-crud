<template>
    <div>
        <b-form @submit.prevent="onSubmit">
            <b-form-group id="input-nama-lengkap" label="Nama lengkap" class="mb-3">
                <input type="hidden" :value="form.id">
                <b-form-input id="input-nama-lengkap" v-model="form.nama_lengkap" type="text"></b-form-input>
            </b-form-group>

            <b-form-group id="input-umur" label="Umur" class="mb-3">
                <b-form-input id="input-umur" v-model="form.umur" type="number"></b-form-input>
            </b-form-group>

            <b-form-group id="input-jurusan" label="jurusan" class="mb-3">
                <b-form-textarea id="input-jurusan" v-model="form.jurusan"></b-form-textarea>
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
    name: 'form-mahasiswa',
    data: function () {
        return {
            form: {
                id: '',
                nama_lengkap: '',
                umur: 0,
                jurusan: ''
            }
        }
    },
    emits: ['submitForm'],
    inject: ['closeModal'],
    methods: {
        onSubmit: function () {
            this.$emit('submitForm', this.form)
            this.closeModal()
        },
    }
}
</script>