<template>
    <modal-form
        ref="$modal"
        :loading="loading"
        :title="langTitle"
        :error="error"
        :disable-save-button="v$.$invalid"
        @submit="doSubmit"
        @hidden="clearContents"
    >
        <tabs content-class="mt-3">
            <form-basic-info v-model:form="form" />
        </tabs>
    </modal-form>
</template>

<script setup lang="ts">
import FormBasicInfo from "~/components/Stations/HlsStreams/Form/BasicInfo.vue";
import {BaseEditModalEmits, BaseEditModalProps, useBaseEditModal} from "~/functions/useBaseEditModal";
import {computed, useTemplateRef} from "vue";
import {useNotify} from "~/functions/useNotify";
import {useTranslate} from "~/vendor/gettext";
import ModalForm from "~/components/Common/ModalForm.vue";
import Tabs from "~/components/Common/Tabs.vue";

const props = defineProps<BaseEditModalProps>();

const emit = defineEmits<BaseEditModalEmits & {
    (e: 'needs-restart'): void
}>();

const $modal = useTemplateRef('$modal');

const {notifySuccess} = useNotify();

const {
    loading,
    error,
    isEditMode,
    form,
    v$,
    clearContents,
    create,
    edit,
    doSubmit,
    close
} = useBaseEditModal(
    props,
    emit,
    $modal,
    {},
    {},
    {
        onSubmitSuccess: () => {
            notifySuccess();
            emit('relist');
            emit('needs-restart');
            close();
        },
    }
);

const {$gettext} = useTranslate();

const langTitle = computed(() => {
    return isEditMode.value
        ? $gettext('Edit HLS Stream')
        : $gettext('Add HLS Stream');
});

defineExpose({
    create,
    edit,
    close
});
</script>
