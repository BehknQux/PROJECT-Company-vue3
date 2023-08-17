<template>
<div class="row mt-5 border border-warning rounded-5 rounded-top-0 p-5 border-top-0">

    <div class="col-6">
        <button @click="ActiveComponent = 'MainPage'" class="h-btn-1 w-100">Ana Sayfa</button>
    </div>
    <div class="col-6">
        <button @click="ActiveComponent = 'CompaniesPage'" class="h-btn-2 w-100">Şirketler</button>
    </div>
    <!-- <MainPage v-if="ActiveComponent == 'MainPage'"></MainPage>
    <CompaniesPage v-if="ActiveComponent == 'CompaniesPage'"></CompaniesPage> -->
    <keep-alive>
        <component :is="ActiveComponent"></component>
    </keep-alive>

</div>
</template>

<script>
import MainPage from "@/components/MainPage.vue"
import CompaniesPage from "@/components/CompaniesPage.vue"

export default {
    data() {
        return {
            ActiveComponent: 'MainPage',
        }
    },
    components: {
        MainPage,
        CompaniesPage
    },
}
</script>