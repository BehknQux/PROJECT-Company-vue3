<template>
<div class="mt-5">
    <div class="d-flex justify-content-end mb-2">
        <button @click="openModal" style="font-size: 20px;font-weight: bold;" class="btn btn-warning">+</button>
    </div>
    <table class="table table-striped table-dark table-hover text-center">
        <thead class="table-bordered border-warning">
            <tr>
                <th class="text-warning">AD</th>
                <th class="text-warning">Çalışan Sayısı</th>
                <th class="text-warning">Kuruluş Tarihi</th>
                <th class="text-warning">Sahibi</th>
                <th class="text-warning">İşlemler</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="(data, index) in dataList" :key="index">
                
                <td>{{ data.companyName }}</td>
                <td>{{ data.employeeNumber }}</td>
                <td>{{ data.companyDate }}</td>
                <td>{{ data.companyOwner }}</td>
                <td>
                    <button @click="editCompany(data)" class="h-btn-1 w-50"> Düzenle </button>
                    <button @click="deleteCompany(data.id)" class="h-btn-2 w-50"> Sil </button>
                </td>
            </tr>
        </tbody>
    </table>
    <ModalWindow :isEdit="isEdit" :editCompanyDetails="editCompanyDetails" @companyData="addData($event)" @close="closeModal" v-if="modal == true" />
</div>
</template>

<script>
import ModalWindow from "@/components/ModalWindow.vue"
export default {
    data() {
        return {
            modal: false,
            dataList: [],
            isEdit: false,
            editCompanyDetails: {}
        }
    },
    mounted() {
        const localData = localStorage.getItem('CompanyData');
        if (localData){
            this.dataList = JSON.parse(localData);
        }
        const sessionData = sessionStorage.getItem('CompanyData');
        if (sessionData){
            this.dataList = JSON.parse(sessionData);
        }
    },
    components: {
        ModalWindow,
    },
    methods: {
        openModal() {
            this.modal = true;
        },
        closeModal() {
            this.modal = false;
            this.isEdit = false
            this.editCompanyDetails = {}
        },
        deleteCompany(companyId) {

            this.dataList = this.dataList.filter(company => company.id != companyId)

            
            this.removeFromStorages(companyId);
            
        },
        removeFromStorages(companyId){

            

            const storedData = JSON.parse(localStorage.getItem('CompanyData')) || [];
            const updatedData = storedData.filter(data => data.id !== companyId);

            localStorage.setItem('CompanyData', JSON.stringify(updatedData));

            // const storedData2 = JSON.parse(sessionStorage.getItem('CompanyData')) || [];
            // const updatedData2 = storedData2.filter(data => data.id !== companyId);
            // sessionStorage.setItem('CompanyData', JSON.stringify(updatedData2));
        },
        editCompany(data) {
            this.isEdit = true
            this.editCompanyDetails = data
            this.modal = true;
        },
        addData(companyDetails) {
            if (this.isEdit) {
                this.dataList = this.dataList.map(function (company) {
                    if (companyDetails.id == company.id) {
                        company = companyDetails
                    }
                    return company
                })
            } else {
                this.dataList.push({
                    ...companyDetails
                });

                /**
                 * *Local ve Session Storage'a burada ekleniyor.
                 */

                localStorage.setItem('CompanyData',JSON.stringify(this.dataList));
                //sessionStorage.setItem('CompanyData', JSON.stringify(this.dataList));
            }
        }
    }
}
</script>
