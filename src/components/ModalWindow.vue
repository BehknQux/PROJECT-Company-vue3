<template>
<div class="modal">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-header">
                <h1 class="modal-title">Yeni şirket ekle</h1>
                <div class="d-flex justify-content-end">
                    <button type="button" class="close h-btn-2" @click="closeModal">
                        <span>&times;</span>
                    </button>
                </div>
            </div>
            <div class="modal-body">
                <p>Şirket Adı</p>
                <input v-model="companyDetails.companyName" type="text" class="h-input mb-3">
                <p>Çalışan Sayısı</p>
                <input v-model="companyDetails.employeeNumber" type="text" class="h-input mb-3">
                <p>Kuruluş Tarihi</p>
                <input v-model="companyDetails.companyDate" type="text" class="h-input mb-3">
                <p>Sahibi</p>
                <input v-model="companyDetails.companyOwner" type="text" class="h-input mb-3">
                <hr>
                <button @click="addCompany" class="h-btn-2 w-100">Ekle</button>
            </div>
        </div>
    </div>
</div>
</template>

<script>
export default {
    props : [
        "isEdit",
        "editCompanyDetails"
    ],
    data() {
        return {
            companyDetails: {
                id : null,
                companyName: null,
                employeeNumber: null,
                companyDate: null,
                companyOwner: null
            }
        }
    },
    methods: {
        closeModal() {
            this.$emit('close');
        },
        addCompany() {

            if(this.isEdit == false){
                this.companyDetails.id = (new Date()).getTime()
            }

            const result = Object.values(this.companyDetails).filter(value => value == null)
            if (result.length > 0) {
                window.alert("Lütfen bilgileri eksiksiz doldurunuz.")
                return;
            }

            this.$emit('companyData',this.companyDetails)
            this.$emit('close');
        }
    },
    mounted() {
        if(this.isEdit){
            this.companyDetails = {...this.editCompanyDetails}
        }
    },
};
</script>

<style scoped>
.modal {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
    transition: 1s ease;
}

.modal-dialog {
    max-width: 600px;
    transition: 1s ease;

}

.modal-content {
    border: none;
    transition: 1s ease;

}

.modal-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    border-bottom: 1px solid #ffc107;
    background-color: #212529;
    transition: 1s ease;

}

.modal-title {
    margin: 0;
    background-color: #212529;
    transition: 1s ease;
}

.modal-body {
    padding: 20px;
    background-color: #212529;
    width: 600px;
    transition: 1s ease;
}
</style>
