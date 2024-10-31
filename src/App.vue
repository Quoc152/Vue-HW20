<script setup>
import { h } from "vue";
import { useForm } from "vee-validate";
import { toTypedSchema } from "@vee-validate/zod";
import * as z from "zod";

import { Button } from "@/components/ui/button";
import {
  FormControl,
  FormDescription,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from "@/components/ui/form";
import { Input } from "@/components/ui/input";
import { toast, Toaster } from "@/components/ui/toast";

const formSchema = toTypedSchema(
  z.object({
    email: z
      .string()
      .min(1, { message: "This is required" })
      .email({ message: "Must be a valid email" }),
    password: z
      .string()
      .min(1, { message: "This is required" })
      .min(8, { message: "Too short" }),
  })
);

const { handleSubmit } = useForm({
  validationSchema: formSchema,
});

const onSubmit = handleSubmit((values) => {
  toast({
    title: "You submitted the following values:",
    description: h(
      "pre",
      { class: "mt-2 w-[340px] rounded-md bg-slate-950 p-4" },
      h("code", { class: "text-white" }, JSON.stringify(values, null, 2))
    ),
  });
});

const vFocus = {
  mounted(el) {
    el.focus()
  }
}
</script>

<template>
  <div class="container h-screen flex justify-center items-center py-8">
    <div
      class="flex flex-col gap-2 w-1/3 border border-slate-300 px-10 py-8 rounded-lg"
    >
      <h3 class="mb-3 font-bold text-center text-2xl">Login Form</h3>
      <Toaster />
      <form class="space-y-6" @submit="onSubmit">
        <FormField v-slot="{ componentField }" name="email">
          <FormItem>
            <FormLabel class="font-bold">Email</FormLabel>
            <FormControl>
              <Input
                v-focus
                type="text"
                placeholder="Email..."
                v-bind="componentField"
              />
            </FormControl>
            <FormMessage />
          </FormItem>
        </FormField>
        <FormField v-slot="{ componentField }" name="password">
          <FormItem>
            <FormLabel class="font-bold">Password</FormLabel>
            <FormControl>
              <Input
                type="password"
                placeholder="Password..."
                v-bind="componentField"
              />
            </FormControl>
            <FormMessage />
          </FormItem>
        </FormField>
        <div class="flex justify-center">
          <Button type="submit">Submit</Button>
        </div>
      </form>
    </div>
  </div>
</template>
