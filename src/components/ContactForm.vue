<template>
  <Form @submit="submitContact" :validation-schema="contactFormSchema">
    <!-- Tên -->
    <div class="form-group">
      <label for="name">Tên</label>
      <Field name="name" type="text" class="form-control" v-model="contactLocal.name" />
      <ErrorMessage name="name" class="error-feedback" />
    </div>

    <!-- E-mail -->
    <div class="form-group">
      <label for="email">E-mail</label>
      <Field name="email" type="email" class="form-control" v-model="contactLocal.email" />
      <ErrorMessage name="email" class="error-feedback" />
    </div>

    <!-- Địa chỉ -->
    <div class="form-group">
      <label for="address">Địa chỉ</label>
      <Field name="address" type="text" class="form-control" v-model="contactLocal.address" />
      <ErrorMessage name="address" class="error-feedback" />
    </div>

    <!-- Điện thoại -->
    <div class="form-group">
      <label for="phone">Điện thoại</label>
      <Field name="phone" type="tel" class="form-control" v-model="contactLocal.phone" />
      <ErrorMessage name="phone" class="error-feedback" />
    </div>

    <!-- Công việc -->
    <div class="form-group">
      <label for="job">Công việc</label>
      <select id="job" class="form-select" v-model="contactLocal.job" name="job">
        <option disabled value="">-- Chọn công việc --</option>
        <option value="Sinh viên">Sinh viên</option>
        <option value="Giáo viên">Giáo viên</option>
        <option value="Kỹ sư">Kỹ sư</option>
        <option value="Nhân viên văn phòng">Nhân viên văn phòng</option>
        <option value="Khác">Khác</option>
      </select>
      <ErrorMessage name="job" class="error-feedback" />
    </div>

    <!-- Sở thích -->
    <div class="form-group">
      <label>Sở thích</label>
      <div class="form-check" v-for="(hobby, index) in hobbyOptions" :key="index">
        <input
          class="form-check-input"
          type="checkbox"
          :value="hobby"
          v-model="contactLocal.hobbies"
          :id="'hobby-' + index"
          name="hobbies"
        />
        <label class="form-check-label" :for="'hobby-' + index">{{ hobby }}</label>
      </div>
      <ErrorMessage name="hobbies" class="error-feedback" />
    </div>

    <!-- Liên hệ yêu thích -->
    <div class="form-group form-check">
      <input
        name="favorite"
        type="checkbox"
        class="form-check-input"
        v-model="contactLocal.favorite"
      />
      <label for="favorite" class="form-check-label">
        <strong>Liên hệ yêu thích</strong>
      </label>
    </div>

    <!-- Nút hành động -->
    <div class="form-group mt-3">
      <button class="btn btn-primary">Lưu</button>
      <button
        v-if="contactLocal._id"
        type="button"
        class="ml-2 btn btn-danger"
        @click="deleteContact"
      >
        Xóa
      </button>
      <button type="button" class="ml-2 btn btn-danger" @click="Cancel">Thoát</button>
    </div>
  </Form>
</template>

<script>
import * as yup from "yup";
import { Form, Field, ErrorMessage } from "vee-validate";

export default {
  components: { Form, Field, ErrorMessage },
  emits: ["submit:contact", "delete:contact"],
  props: {
    contact: { type: Object, required: true },
  },
  data() {
    const contactFormSchema = yup.object().shape({
      name: yup
        .string()
        .required("Tên phải có giá trị.")
        .min(2, "Tên phải ít nhất 2 ký tự.")
        .max(50, "Tên có nhiều nhất 50 ký tự."),
      email: yup
        .string()
        .email("E-mail không đúng.")
        .max(50, "E-mail tối đa 50 ký tự."),
      address: yup.string().max(100, "Địa chỉ tối đa 100 ký tự."),
      phone: yup
        .string()
        .matches(/((09|03|07|08|05)+([0-9]{8})\b)/g, "Số điện thoại không hợp lệ."),
      job: yup.string().required("Vui lòng chọn công việc."),
      hobbies: yup
        .array()
        .min(1, "Hãy chọn ít nhất một sở thích.")
        .required("Hãy chọn ít nhất một sở thích."),
    });

    return {
      contactLocal: {
        name: this.contact.name || "",
        email: this.contact.email || "",
        address: this.contact.address || "",
        phone: this.contact.phone || "",
        job: this.contact.job || "",
        hobbies: this.contact.hobbies || [],
        favorite: this.contact.favorite || false,
      },
      hobbyOptions: ["Đọc sách", "Nghe nhạc", "Du lịch", "Thể thao", "Nấu ăn", "Chơi game"],
      contactFormSchema,
    };
  },
  methods: {
    submitContact() {
      this.$emit("submit:contact", this.contactLocal);
    },
    deleteContact() {
      this.$emit("delete:contact", this.contactLocal.id);
    },
    Cancel() {
      const reply = window.confirm("Bạn có chắc muốn thoát mà không lưu thay đổi?");
      if (reply) this.$router.push({ name: "contactbook" });
    },
  },
};
</script>

<style scoped>
@import "@/assets/form.css";

.error-feedback {
  color: red;
  font-size: 0.9em;
}
</style>
