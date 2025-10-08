<template>
  <div class="page">
    <h4>Thêm Liên hệ</h4>
    <ContactForm :contact="{}" @submit:contact="addContact" />
  </div>
</template>

<script>
import ContactForm from "@/components/ContactForm.vue";
import ContactService from "@/services/contact.service";

export default {
  name: "AddContact",
  components: {
    ContactForm,
  },
  methods: {
    async addContact(data) {
      try {
        await ContactService.create(data);
        alert("Liên hệ mới đã được thêm thành công!");
        this.$router.push({ name: "contactbook" });
      } catch (error) {
        console.error(error);
      }
    },
  },
};
</script>

<style scoped>
.add-contact-page {
  display: flex;
  min-height: 100vh;
}
</style>
