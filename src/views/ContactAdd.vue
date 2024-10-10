<template>
  <div class="">
    <h4>Thêm Liên hệ mới</h4>
    <ContactForm :contact="contact" @submit:contact="createContact" />
    <p>{{ message }}</p>
  </div>
</template>
<script>
import ContactForm from "@/components/ContactForm.vue";
import ContactService from "@/services/contact.service";
export default {
  components: {
    ContactForm,
  },
  data() {
    return {
      contact: {
        name: "",
        email: "",
        address: "",
        phone: "",
        favorite: false,
      },
      message: "",
    };
  },
  methods: {
    async createContact(){
      try {
        await ContactService.create(this.contact);
        alert('Liên hệ được tạo thành công.');
        this.$router.push({ name: "contactbook" });
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>