<template>
  <Form  :formFields="formFields" :schema="rules" @input-change="handleChange" @set-error="handleSetError" @delete-error="handleDeleteError" @submit="handleSubmit" />
</template>
<script lang="ts">
import { defineComponent, ref } from "vue";
import Form from "@/components/Form.vue";
import { FormField } from "@/interfaces/FormField.interface";
import {
  maxLength,
  required,
  minLength,
  email,
  helpers,
} from "@vuelidate/validators";

export default defineComponent({
  components: {
    Form,
  },
  setup() {
    const formFields = ref<{ [key: string]: FormField }>({
      login: {
        name: "login",
        label: "Логин",
        type: "text",
        value: "",
        error: "",
      },
      email: {
        name: "email",
        label: "Email",
        type: "email",
        value: "",
        error: "",
      },
      password: {
        name: "password",
        label: "Пароль",
        type: "password",
        value: "",
        error: "",
      },
    });

    const rules = {
      login: {
        value: {
          required,
        },
      },
      email: {
        value: {
          email: helpers.withMessage(
            "Введите корректный email",
            email
          ),
          required: helpers.withMessage(
            "Введите корректный email",
            required
          ),
        },
      },
      password: {
        value: {
          required,
          minLength: helpers.withMessage(
            "Введите более 8 символов",
            minLength(8)
          ),
          maxLength: helpers.withMessage(
            "В пароле не должно быть больше 30 символов",
            maxLength(30)
          ),
        },
      },
    };

    const handleChange = (data: { name: string; value: string }) =>
      (formFields.value[data.name].value = data.value);

    const handleSetError = (data: { name: string; message: string }) =>
      (formFields.value[data.name].error = data.message);

    const handleDeleteError = (name: string) =>
      (formFields.value[name].error = "");

    const handleSubmit = () => console.log("form submitted", formFields.value);

    return {
      formFields,
      rules,
      handleChange,
      handleSetError,
      handleDeleteError,
      handleSubmit,
    };
  },
});
</script>
