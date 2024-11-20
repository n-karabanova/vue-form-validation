<template>
  <form>
    <div class="form-field" v-for="field in formFields" :key="field.name">
      <label :for="field.name">{{ field.label }}</label>
      <input v-if="field.type !== 'textarea'" :type="field.type" :name="field.name" @input="handleChange($event, field.name)" />
      <textarea v-else :name="field.name" id="" cols="30" rows="10" @input="handleChange($event, field.name)"></textarea>
      <p class="error" v-if="field.error">{{ field.error }}</p>
    </div>
    <button @click="handleSubmit">Submit</button>
  </form>
</template>

<script lang="ts">
import { defineComponent, PropType } from "vue";
import useValidate, { ValidationArgs } from "@vuelidate/core";
import { FormField } from "@/interfaces/FormField.interface";

export default defineComponent({
  props: {
    formFields: {
      type: Object as PropType<{ [key: string]: FormField }>,
      required: true,
    },
    schema: {
      type: Object as PropType<ValidationArgs>,
      required: true,
    },
  },
  setup(props, { emit }) {
    const handleChange = (e: Event, name: string) => {
      const input = e.currentTarget as HTMLInputElement;
      const value = input.value;
      emit("input-change", { name, value });
      validateField(name);
    };

    const handleSubmit = async (e: Event) => {
      e.preventDefault();
      const valid = await v$.value.$validate();
      if (!valid) {
        const fields = Object.keys(props.formFields);
        fields.forEach((fieldName) => {
          if (v$.value[fieldName].$error) {
            emit("set-error", {
              name: fieldName,
              message: v$.value[fieldName].$errors[0].$message,
            });
          }
        });
      } else emit("submit");
    };

    const validateField = (name: string) => {
      const field = v$.value[name];
      if (field) {
        field.$touch();
        if (field.$error)
          emit("set-error", { name, message: field.$errors[0].$message });
        else emit("delete-error", name);
      }
    };

    const v$ = useValidate(props.schema, props.formFields);

    return {
      handleChange,
      handleSubmit,
      v$,
    };
  },
});
</script>

<style scoped>
form {
	width: 50%;
	margin: 20px auto;
}

.form-field {
	display: flex;
    row-gap: 8px;
    flex-direction: column;
    padding-bottom: 20px;
}

label {
	width: 100%;
    text-align: left;
    font-size: 20px;
    font-weight: 600;
    color: #4b4a4a;
}

input {
	border: solid 2px black;
    border-radius: 7px;
    background-color: white;
    padding: 8px 4px;
}

input:hover,
input:focus {
  border: solid 2px gray;
}

.error {
  text-align: left;
  color: red;
  //margin-left: 5px;
}

button {
    width: 50%;
    margin: auto;
    padding: 12px;
    font-size: 18px;
    background: red;
    color: white;
    border-radius: 36px;
    cursor: pointer;

}

button:hover {
  opacity: 0.7;
}
</style>
