<template>
    <div>
        <b-button v-b-modal class="btn-sm mt-3" variant="primary" @click="openModal">{{ titleButton }}</b-button>

        <b-modal v-model="showModal" :title="titleModal" hide-footer>
            <slot></slot>
        </b-modal>
    </div>
</template>

<script>
export default {
    name: 'modal-component',
    data: function () {
        return {
            showModal: false
        }
    },
    props: {
        titleButton: {
            type: String,
            required: true
        },
        titleModal: {
            type: String,
            required: true
        },
    },
    provide: function () {
        return {
            closeModal: this.CloseModal
        }
    },
    methods: {
        openModal: function () {
            this.showModal = true
        },
        CloseModal: function () {
            this.showModal = false
        }
    }
}
</script>

<style>
.modal-backdrop {
    opacity: 0.5 !important;
}

.close {
    display: none;
}
</style>