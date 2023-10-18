<template>
    <div>
        <b-form @submit.prevent="onSubmit">
            <div v-if="errors.length > 0" class="bg-danger p-2 mb-2">
                <ul class="mb-0">
                    <li v-for="error in errors" :key="error" class="text-white">{{ error }}</li>
                </ul>
            </div>
            <b-form-group id="input-nama-lengkap" label="Nama lengkap" class="mb-3">
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
            'form': {
                'id': '',
                'nama_lengkap': '',
                'umur': 0,
                'jurusan': ''
            },
            'errors': []
        }
    },
    emits: ['submit-form', 'close-modal'],
    props: {
        data: {
            type: Object,
            required: false
        }
    },
    mounted: function () {
        if (this.data) {
            this.form.id = this.data.id != null ? this.data.id : ''
            this.form.nama_lengkap = this.data.nama_lengkap != null ? this.data.nama_lengkap : ''
            this.form.umur = this.data.umur != null ? this.data.umur : 0
            this.form.jurusan = this.data.jurusan != null ? this.data.jurusan : ''
        }
    },
    methods: {
        onSubmit: function () {
            if (this.ValidationInput(this.form)) {
                this.$emit('submit-form', this.form)
                this.closeModal()
            }
            console.log(this.errors)
        },
        ValidationInput: function (formInput) {
            this.errors = []
            if (!formInput.nama_lengkap.trim()) {
                this.errors.push('Nama perlu diisi')
            }
            let parseUmur = parseInt(formInput.umur);
            if (!formInput.umur) {
                this.errors.push('Umur perlu diisi')
            } else if (parseUmur == 0) {
                this.errors.push('Umur harus lebih besar dari 0')
            }
            if (!formInput.jurusan) {
                this.errors.push('Jurusan perlu diisi')
            }

            if (this.errors.length > 0) return false
            return true
        },
        closeModal: function () {
            this.$emit('close-modal')
        }
    }
}
</script>