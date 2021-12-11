<template>
    <div>
        <div class="card">
            <TabMenu :model="items" />
            <router-view/>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            active: 3,
            items: [
                {
                    label: 'Productos',
                    icon: 'pi pi-fw pi-box',
                    to: '@/vista'
                },
                {
                    label: 'Compras',
                    icon: 'pi pi-fw pi-shopping-cart',
                    to: '@/vista2'
                },
                {
                    label: 'Ventas',
                    icon: 'pi pi-fw pi-briefcase',
                    to: '@/vista3'
                },
                {
                    label: 'Usuarios',
                    icon: 'pi pi-fw pi-user',
                    to: '@/vista4'
                },
            ],
           
        }
    }
}
</script>

<style scoped lang="scss">
::v-deep(.tabmenudemo-content) {
    padding: 2rem 1rem;
}
</style>