<template>
    <form-group
        v-bind="$attrs"
        :id="id"
    >
        <template #default>
            <slot
                name="default"
                v-bind="{ id, field, model }"
            >
                <div class="form-check form-switch">
                    <form-checkbox
                        v-bind="inputAttrs"
                        :id="id"
                        v-model="model"
                        role="switch"
                        :name="name"
                    />
                    <label
                        class="form-check-label"
                        :for="id"
                    >
                        <form-label
                            :is-required="isRequired"
                            v-bind="pickProps(props, formLabelProps)"
                        >
                            <slot name="label">{{ label }}</slot>
                        </form-label>
                    </label>
                </div>
            </slot>

            <vuelidate-error
                v-if="isVuelidateField"
                :field="field"
            />
        </template>

        <template
            v-if="description || slots.description"
            #description="slotProps"
        >
            <slot
                name="description"
                v-bind="slotProps"
            >
                {{ description }}
            </slot>
        </template>
    </form-group>
</template>

<script setup lang="ts">
import VuelidateError from "./VuelidateError.vue";
import FormLabel from "~/components/Form/FormLabel.vue";
import FormGroup from "~/components/Form/FormGroup.vue";
import {formFieldProps, useFormField} from "~/components/Form/useFormField";
import {useSlots} from "vue";
import FormCheckbox from "~/components/Form/FormCheckbox.vue";
import formLabelProps from "~/components/Form/formLabelProps.ts";
import {pickProps} from "~/functions/pickProps.ts";

const props = defineProps({
    ...formFieldProps,
    ...formLabelProps,
    id: {
        type: String,
        required: true
    },
    name: {
        type: String,
        default: null
    },
    label: {
        type: String,
        default: null
    },
    description: {
        type: String,
        default: null
    },
    inputAttrs: {
        type: Object,
        default() {
            return {};
        }
    },
});

const slots = useSlots();

const emit = defineEmits(['update:modelValue']);

const {model, isVuelidateField, isRequired} = useFormField(props, emit);
</script>
