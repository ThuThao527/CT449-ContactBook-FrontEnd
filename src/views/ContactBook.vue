<template>
    <div class="page row">
        <div class="col-md-7">
            <!-- Thành phần tìm kiếm-->
            <InputSearch v-model="searchText" />
        </div>
        <div class="alert alert-info col-md-6">
            <i class="fas fa-address-book"></i> Danh bạ
            <span class="badge bg-info text-dark">
                {{ filteredContacts.length }}
            </span>
            <ul v-if="filteredContacts.length > 0">
                <ContactCard v-for="contact in filteredContacts" :key="contact._id" :contact="contact"
                    :isSelected="contact._id === activeIndex" />
            </ul>
            <p v-else> Không có liên hệ nào.</p>
        </div>

        <div class="row justify-content-around align-items-center">
            <button class="btn btn-primary" @click="refreshList">
                <i class="fas fa-redo"></i> Làm mới
            </button>
            <button class="btn btn-success" @click="goToAddConTact">
                <i class="fas fa-plus"></i> Thêm mới
            </button>
        </div>
    </div>
</template>

<script setup>
import ContactCard from '@/components/ContactCard.vue';
import InputSearch from '@/components/InputSearch.vue';
import {ref, computed, onMounted, watch} from 'vue';
import {useRouter} from "vue-router";
import ContactService from "@/services/contact.service";


// Dữ liệu và trạng thái
const searchText = ref(''); //lưu danh sách các liên hệ
const contacts = ref([]); //chỉ mục của đối tượng contactCard
const activeIndex = ref(null); //chứa dữ liệu nhập vào từ thanh tìm kiếm
const router = useRouter();

// Giám sát các thay đổi của biến searchText.
// Bỏ chọn phần tử đang được chọn trong danh sách.
watch(searchText, () =>{
    activeIndex.value = -1;
});

// Chuyển các đối tượng contact thành chuỗi để tiện cho tìm kiếm.
const contactStrings = computed(()=>{
    return contacts.value.map((contact)=>{
        const{name, email, address, phone} = contact;
        return [name, email, address, phone].join("");
    });
});
//Lấy contact hiện tại
const activeContact = computed(()=>{
    if(activeIndex.value < 0) return null;
    return filteredContacts.value[activeIndex.value];
});

//Đếm số lượng contact đã lộc
const filteredContactsCount = computed(()=> {
    return filteredContacts.value.length;
});
//Lấy danh sách contact từ API
const retrieveContacts = async() => {
    try {
        contacts.value = await ContactService.getAll();
    }
    catch (error) {
        console.log(error);
    }
};

// Làm mới danh sách contact
const refreshList = async() =>{
    contacts.value = await ContactService.getAll();
}

//Xóa tất cả contact
const removeAllContacts = async() =>{
    if(confirm("Bạn muốn xóa tất cá liên hệ?")){
        try{
            await ContactService.deleteAll();
            refreshList();
        } catch(error) {
            console.log(error);
        }
    }
};

// Tính toán các liên hệ đã lọc
const filteredContacts = computed(() => {
    if (!searchText.value) return contacts.value;
    return contacts.value.filter(contact =>
        contact.name.toLowerCase().includes(searchText.value.toLowerCase())
    );
});

// Điều hướng đến trang thêm liên hệ
const goToAddContact = () => {
    // Điều hướng đến trang thêm liên hệ 
    console.log('Đi đến trang thêm liên hệ');
};

onMounted(()=>{
    refreshList();
});

</script>

<style scoped>
h3 {
    text-align: left;
}
</style>