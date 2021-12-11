<template>
    <div>
        <div class="card">
            <!--Botones Agregar, eliminar, importar y exportar-->
            <Toolbar class="p-mb-4">
                <template #start>
                    <Button label="Nuevo" icon="pi pi-plus" class="p-button-success p-mr-2" @click="openNew" />
                    <Button label="Eliminar" icon="pi pi-trash" class="p-button-danger" @click="confirmDeleteSelected" :disabled="!selectedProducts || !selectedProducts.length" />
                </template>
            </Toolbar>
            <!--Fin Botones Agregar, eliminar, importar y exportar-->

            <!--Tabla-->
            <DataTable ref="dt" :value="products" v-model:selection="selectedProducts" dataKey="id" 
                :paginator="true" :rows="10" :filters="filters"
                paginatorTemplate="FirstPageLink PrevPageLink PageLinks NextPageLink LastPageLink CurrentPageReport RowsPerPageDropdown" :rowsPerPageOptions="[5,10,25]"
                currentPageReportTemplate="Mostrando {first} a {last} de {totalRecords} productos" responsiveLayout="scroll">
                <template #header>
                    <div class="table-header p-d-flex p-flex-column p-flex-md-row p-jc-md-between">
						<h5 class="p-mb-2 p-m-md-0 p-as-md-center">Usuarios</h5>
						<span class="p-input-icon-left">
                            <i class="pi pi-search" />
                            <InputText v-model="filters['global'].value" placeholder="Buscar..." />
                        </span>
					</div>
                </template>

                <Column selectionMode="multiple" style="width: 3rem" :exportable="false"></Column>
                <Column field="username" header="Usuario" :sortable="true" style="min-width:10rem"></Column>
                <Column field="password" header="Contraseña" :sortable="true" style="min-width:10rem"></Column>
                <Column field="name" header="Nombre" :sortable="true" style="min-width:10rem"></Column>
                <Column field="lastName" header="Apellidos" :sortable="true" style="min-width:10rem"></Column>
                <Column field="document" header="Numero Documento" :sortable="true" style="min-width:10rem"></Column>
                <Column field="email" header="Email" :sortable="true" style="min-width:10rem"></Column>
                <Column field="phoneNumber" header="Teléfono" :sortable="true" style="min-width:10rem"></Column>
                <Column field="role" header="Rol" :sortable="true" style="min-width:10rem"></Column>
                <Column :exportable="false" style="min-width:8rem">
                    <template #body="slotProps">
                        <Button icon="pi pi-pencil" class="p-button-rounded p-button-success p-mr-2" @click="editProduct(slotProps.data)" />
                        <Button icon="pi pi-trash" class="p-button-rounded p-button-warning" @click="confirmDeleteProduct(slotProps.data)" />
                    </template>
                </Column>
            </DataTable>
        </div>
        <!--Fin Tabla-->


        <!--- Formulario para crear un nuevo usuario -->
        <Dialog v-model:visible="userDialog" :style="{width: '450px'}" header="Creando un nuevo usuario" :modal="true" class="p-fluid">
            
            
            <div class="p-field">
                <label for="username">Usuario</label>
                <InputText id="name" v-model.trim="user.username" required="true" autofocus :class="{'p-invalid': submitted && !user.username}" />
                <small class="p-error" v-if="submitted && !user.username">Espacio requerido.</small>
            </div>

            <div class="p-field">
                <label for="password">Contraseña</label>
                <InputText id="name" v-model.trim="user.password" required="true" autofocus :class="{'p-invalid': submitted && !user.password}" />
                <small class="p-error" v-if="submitted && !user.password">Espacio requerido.</small>
            </div>

            <div class="p-field">
                <label for="name">Nombre</label>
                <InputText id="name" v-model.trim="user.name" required="true" autofocus :class="{'p-invalid': submitted && !user.name}" />
                <small class="p-error" v-if="submitted && !user.name">Espacio requerido.</small>
            </div>

            <div class="p-field">
                <label for="lastName">Apellidos</label>
                <InputText id="name" v-model.trim="user.lastName" required="true" autofocus :class="{'p-invalid': submitted && !user.lastName}" />
                <small class="p-error" v-if="submitted && !user.lastName">Espacio requerido.</small>
            </div>

            <div class="p-field">
                <label for="document">Número de identidad</label>
                <InputText id="name" v-model.trim="user.document" required="true" autofocus :class="{'p-invalid': submitted && !user.document}" />
                <small class="p-error" v-if="submitted && !user.document">Espacio requerido.</small>
            </div>

            <div class="p-field">
                <label for="email">email</label>
                <InputText id="name" v-model.trim="user.email" required="true" autofocus :class="{'p-invalid': submitted && !user.email}" />
                <small class="p-error" v-if="submitted && !user.email">Espacio requerido.</small>
            </div>
            
            <div class="p-field">
                <label for="phoneNumber">Teléfono</label>
                <InputText id="name" v-model.trim="user.phoneNumber" required="true" autofocus :class="{'p-invalid': submitted && !user.phoneNumber}" />
                <small class="p-error" v-if="submitted && !user.phoneNumber">Espacio requerido.</small>
            </div>

            <div class="p-field">
                <label class="p-mb-3">Rol</label>
                <div class="p-formgrid p-grid">
                    <div class="p-field-radiobutton p-col-6">
                        <RadioButton id="category1" name="category" value="administrador" v-model="user.rol" />
                        <label for="role">Administrador</label>
                    </div>
                    <div class="p-field-radiobutton p-col-6">
                        <RadioButton id="category2" name="category" value="auxiliar" v-model="user.rol" />
                        <label for="role">Auxiliar</label>
                    </div>
                    <div class="p-field-radiobutton p-col-6">
                        <RadioButton id="category3" name="category" value="vendedor" v-model="user.rol" />
                        <label for="role">Vendedor</label>
                    </div>
                </div>
            </div>

        </Dialog>
        <!--Fin Formulario-->

        <!-- Eliminar un solo Registro -->
        <Dialog v-model:visible="deleteProductDialog" :style="{width: '450px'}" header="Confirm" :modal="true">
            <div class="confirmation-content">
                <i class="pi pi-exclamation-triangle p-mr-3" style="font-size: 2rem" />
                <span v-if="product">   ¿Está seguro que quiere eliminar este registro? <b>{{product.name}}</b>?</span>
            </div>
            <template #footer>
                <Button label="No" icon="pi pi-times" class="p-button-text" @click="deleteProductDialog = false"/>
                <Button label="Sí" icon="pi pi-check" class="p-button-text" @click="deleteProduct" />
            </template>
        </Dialog>

        <!-- Eliminar varios Registros -->
        <Dialog v-model:visible="deleteProductsDialog" :style="{width: '450px'}" header="Confirm" :modal="true">
            <div class="confirmation-content">
                <i class="pi pi-exclamation-triangle p-mr-3" style="font-size: 2rem" />
                <span v-if="product">  ¿Está seguro que quiere eliminar los registros seleccionados? </span>
            </div>
            <template #footer>
                <Button label="No" icon="pi pi-times" class="p-button-text" @click="deleteProductsDialog = false"/>
                <Button label="Sí" icon="pi pi-check" class="p-button-text" @click="deleteSelectedProducts" />
            </template>
        </Dialog>

	</div>
</template>

<script>
import { FilterMatchMode } from 'primevue/api';
import ProductService from './service/ProductService';

export default {
    data() {
        return {
            users: [],
            userDialog: false,
            deleteUseroDialog: false,
            deleteUserDialog: false,
            user: {},
            selectedUser: null,
            filters: {},
            submitted: false,
        }
    },
    userService: null,

 
    created() {
        this.userService = new UserService();
        this.initFilters();
    },
    mounted() {
        this.userService.getUsers().then(data => this.users = data);
    },

    //Metodos
    methods: {
        openNew() {
            this.user = {};
            this.submitted = false;
            this.userDialog = true;
        },
        hideDialog() {
            this.userDialog = false;
            this.submitted = false;
        },
        //Guardar un solo Registro
        saveProduct() {
            this.submitted = true;
//
			if (this.user.name.trim()) {
                if (this.user.id) {
                    this.user.inventoryStatus = this.product.inventoryStatus.value ? this.product.inventoryStatus.value: this.product.inventoryStatus;
                    this.users[this.findIndexById(this.product.id)] = this.product;
                    this.$toast.add({severity:'success', summary: 'Successful', detail: 'Product Updated', life: 3000});
                }
                else {
                    this.product.id = this.createId();
                    this.product.code = this.createId();
                    this.product.image = 'product-placeholder.svg';
                    this.product.inventoryStatus = this.product.inventoryStatus ? this.product.inventoryStatus.value : 'INSTOCK';
                    this.products.push(this.product);
                    this.$toast.add({severity:'success', summary: 'Successful', detail: 'Product Created', life: 3000});
                }

                this.productDialog = false;
                this.product = {};
            }
        },//

        //editar un solo Registro
        editProduct(user) {
            this.user = {...user};
            this.userDialog = true;
        },
        //Confirmar Eliminar Registro
        confirmDeleteProduct(user) {
            this.user = user;
            this.deleteUserDialog = true;
        },
        //Eliminar un solo Registro
        deleteUser() {
            this.users = this.users.filter(val => val.id !== this.user.id);
            this.deleteUserDialog = false;
            this.user = {};
            this.$toast.add({severity:'success', summary: 'Successful', detail: 'User Deleted', life: 3000});
        },
        findIndexById(id) {
            let index = -1;
            for (let i = 0; i < this.users.length; i++) {
                if (this.users[i].id === id) {
                    index = i;
                    break;
                }
            }

            return index;
        },
        confirmDeleteSelected() {
            this.deleteUsersDialog = true;
        },
        deleteSelectedProducts() {
            this.products = this.products.filter(val => !this.selectedProducts.includes(val));
            this.deleteProductsDialog = false;
            this.selectedProducts = null;
            this.$toast.add({severity:'success', summary: 'Successful', detail: 'Products Deleted', life: 3000});
        },
        initFilters() {
            this.filters = {
                'global': {value: null, matchMode: FilterMatchMode.CONTAINS},
            }
        }
    }
}
</script>

<style lang="scss" scoped>
.table-header {
    display: flex;
    align-items: center;
    justify-content: space-between;

    @media screen and (max-width: 960px) {
        align-items: start;
	}
}

.product-image {
    width: 50px;
    box-shadow: 0 3px 6px rgba(0, 0, 0, 0.16), 0 3px 6px rgba(0, 0, 0, 0.23);
}

.p-dialog .product-image {
    width: 50px;
    margin: 0 auto 2rem auto;
    display: block;
}

.confirmation-content {
    display: flex;
    align-items: center;
    justify-content: center;
}
@media screen and (max-width: 960px) {
	::v-deep(.p-toolbar) {
		flex-wrap: wrap;
        
		.p-button {
            margin-bottom: 0.25rem;
        }
	}
}
</style>