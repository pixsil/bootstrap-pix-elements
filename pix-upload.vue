<!-- v2: from expr to github -->

<template>
    <!-- false = delete-->
    <!-- string / string = keep-->
    <!-- file = replace-->
    <div class="custom-file-input-div">
        <!--<div v-bind:class="{ 'has-error' : vueFormParent.vueErrors.get(field.column_name) }" @keydown="vueFormParent.vueErrors.clear(field.column_name)" class="form-group">-->
        <!--    <div class="row">-->
        <!--        <div class="col-md-12  font-weight-bold">-->
        <!--            <label class="col-form-label" :for="field.column_name">{{ field.label }}</label>-->
        <!--        </div>-->
        <!--    </div>-->
        <!--    <div class="row">-->
        <!--        <div class="col-md-12 ">-->
        <!--            <div v-if="$root.has_ftp">-->

        <!--<div class="mb-3">-->
        <!--    <label for="formFile" class="form-label">Default file input example</label>-->
        <!--    <input class="form-control" type="file" id="formFile">-->
        <!--</div>-->

        <div class="custom-file ">
            <div class="input-group mb-3">
                <label data-browse="Browse"
                       :for="this.field_name"
                       class="custom-file-label fake-form-control form-control"
                >
                    <span v-if="!has_file" class="d-block form-file-text" style="pointer-events: none;">
                        <template v-if="is_deleted"><i class="mdi mdi-close-thick me-1"></i> Delete uploaded file.</template>
                        <template v-else>No file selected.</template>
                    </span>
                    <span v-else-if="has_file && !is_uploaded_file" class="d-block form-file-text" style="pointer-events: none;">
                        <i class="mdi mdi-upload me-1"></i> File selected for upload.
                    </span>
                    <span v-else class="d-block form-file-text" style="pointer-events: none;">
                        <i class="mdi mdi-check-bold me-1"></i> File uploaded.
                    </span>
                </label>
                <input type="file"
                       class="custom-file-input"
                       @change="handeFileUpload"
                       :id="this.field_name"
                       :accept="accept">
                <span v-if="is_uploaded_file" class="input-group-text mouse-pointer" @click="deleteFile">
                    <i class="mdi mdi-delete mdi-18px"></i>
                </span>
                <span v-else-if="has_file || is_deleted" class="input-group-text mouse-pointer" @click="resetField">
                    <i class="mdi mdi-undo mdi-18px"></i>
                </span>
                <span v-if="is_uploaded_file" class="input-group-text mouse-pointer" @click="deleteFile">
                    <i class="mdi mdi-download mdi-18px"></i>
                </span>
            </div>


        </div>


        <!--<div class="input-group mb-3">-->

        <!--    <input class="form-control"-->
        <!--        type="file"-->
        <!--        :ref="this.field_name"-->
        <!--        :value="getFile(vueFormParent.formData, field_name)"-->
        <!--        @change="setFile(vueFormParent.formData, field_name, $event)"-->
        <!--    >-->
        <!--    <label for=""></label>-->
        <!--    <span v-if="has_file" class="input-group-text mouse-pointer" @click="deleteFile">-->
        <!--        <i class="mdi mdi-delete mdi-18px"></i>-->
        <!--    </span>-->
        <!--    <span v-if="is_uploaded_file" class="input-group-text mouse-pointer" @click="deleteFile">-->
        <!--        <i class="mdi mdi-download mdi-18px"></i>-->
        <!--    </span>-->
        <!--</div>-->

        <!--<b-input-group>-->
        <!--    <b-form-file-->
        <!--        ref="upload_field"-->
        <!--        @change="handeFileUpload"-->
        <!--        :placeholder="placeholder"-->
        <!--        drop-placeholder="Drop file here..."-->
        <!--        class="mouse-pointer"-->
        <!--    ></b-form-file>-->
        <!--    <b-input-group-append v-if="has_file" @click="deleteFile" is-text class="mouse-pointer">-->
        <!--        <i class="fa fa-sm fa-trash"></i>-->
        <!--    </b-input-group-append>-->
        <!--    <b-input-group-append v-if="is_uploaded_file" @click="openFile" is-text class="mouse-pointer">-->
        <!--        <i class="fas fa-file-download"></i>-->
        <!--    </b-input-group-append>-->
        <!--</b-input-group>-->


        <!--                </div>-->
        <!--                <div v-else>-->
        <!--                    <div v-if="$root.is_admin" class="alert alert-light" role="alert">-->
        <!--                        There is no FTP connection configured.-->
        <!--                    </div>-->
        <!--                </div>-->
        <!--            </div>-->
        <!--        </div>-->
        <!--        <div class="row" v-if="vueFormParent.vueErrors.get(field.column_name)">-->
        <!--            <div class="col-md-12">-->
        <!--                <span class="text-danger small">-->
        <!--                    {{ vueFormParent.vueErrors.get(field.column_name) }}-->
        <!--                </span>-->
        <!--            </div>-->
        <!--        </div>-->
        <!--    </div>-->
    </div>
</template>

<script>
export default {

    mixins: [
    ],

    props: {
        modelValue: [Object],
        field_name: String,
        vueFormParent: Object,
        meta_object: {
            required: false,
            default: null,
            type: Object,
        },
        accept: {
            required: false,
            default: null,
            type: String,
        },
    },

    emits: [
        'update:modelValue',
    ],

    data() {
        return {
        };
    },

    methods: {
        handeFileUpload(event) {
            this.modelValue = event.target.files[0];

            if (this.hasMetaObject) {
                delete this.meta_object['delete'];
            }
        },
        resetField() {
            // this.$refs[this.field_name].reset();
            this.modelValue = this.vueFormParent.originalFormData[this.field_name];

            if (this.hasMetaObject) {
                this.meta_object['delete'] = true;
            }
        },
        deleteFile() {
            // this.$refs[this.field_name].reset();
            this.modelValue = false;

            if (this.hasMetaObject) {
                this.meta_object['delete'] = true;
            }
        },
        openFile() {
            window.open('/serve/'+ this.field.table_config_id +'/'+ this.field.id +'/'+ this.vueFormParent.formData.id, '_blank');
        },
        setFile(object, key, event) {
            object[key]['file'] = event.target.files[0];
            object[key]['delete'] = false;
            // this.$emit('on-set-file');
        },
        getFile(object, key) {
            if (!object[key]) {
                return;
            }

            // give back null when it is a string
            // make a nice 'file connected' message to the field
            if (!(object[key]['file'] instanceof File)) {
                return null;
            }

            return object[key]['file'];
        },
        hasAlreadyFile(object, key) {
            if (!object[key]) {
                return false;
            }
            if (typeof object[key]['saved_name'] === 'string') {
                return true;
            }

            return false;
        },
    },

    mounted() {
    },

    created() {
    },

    computed: {
        hasMetaObject() {
            return !!this.meta_object;
        },
        // placeholder() {
        //     //
        //     if (typeof this.modelValue !== 'string') {
        //         return '';
        //     }
        //     //
        //     if (!this.modelValue) {
        //         return ''
        //     }
        //
        //     return this.modelValue;
        // },
        has_file() {
            return this.modelValue ? true : false;
        },
        is_uploaded_file() {
            // guard false
            if (!this.modelValue) {
                return false;
            }
            //
            if (typeof this.modelValue !== 'string') {
                return false;
            }

            return true;
        },
        is_deleted() {
            return this.modelValue === false;
        },
        // field_var() {
        //     return this.modelValue;
        // }
    },
    watch: {
        // 'field_var': function (new_value, old_value) {
        //     console.log('delete res3et');
        //     // it muset always reset detele except when delete is in place
        //
        //     // // guard only clear if new value is null or empty
        //     // if (new_value) {
        //     //     console.log('updated by design reset delete');
        //     //     return;
        //     // }
        //
        //     // // clear deleted
        //     // if (this.hasMetaObject) {
        //     //     delete this.meta_object['delete'];
        //     // }
        // }
    },
}
</script>

<style>
.custom-file-input-div .custom-file-label {
    cursor: pointer;
}
.custom-file-input-div .custom-file {
    position: relative;
    display: inline-block;
    width: 100%;
    height: calc(1.6em + 0.75rem + 2px);
    margin-bottom: 0;
}
.custom-file-input-div .custom-file-input {
    position: relative;
    z-index: 2;
    width: 100%;
    height: calc(1.6em + 0.75rem + 2px);
    margin: 0;
    overflow: hidden;
    opacity: 0;
    display: none;
}
.custom-file-input-div .custom-file-label {
    position: absolute;
    top: 0;
    right: 0;
    left: 0;
    z-index: 1;
    height: calc(1.6em + 0.75rem + 2px);
    padding-top: 0.375rem;
    padding-right: 0.75rem;
    padding-bottom: 0.375rem;
    padding-left: 1.75rem;
    overflow: hidden;
    font-weight: 400;
    line-height: 1.6;
    color: #495057;
    background-color: #fff;
    white-space: nowrap;
    overflow-x: hidden;
}
.custom-file-input-div .custom-file-label::after {
    position: absolute;
    top: 0;
    /*left: 0;*/
    bottom: 0;
    z-index: 3;
    display: block;
    height: calc(1.6em + 0.75rem);
    padding-top: 0.375rem;
    padding-bottom: 0.375rem;
    line-height: 1.6;
    color: #495057;
    content: "Browse…";
    background-color: #e9ecef;
    border-left: inherit;
    left: -1px;
    color: black;
}
.custom-file-input-div .fake-form-control {
    position: relative;
    flex: 1 1 auto;
    width: 1%;
    min-width: 0;
    margin-bottom: 0;
}
.form-file-text {
    padding-left: 88px;
}

</style>
