<template>
    <div>
      <b-row>
        <b-alert v-model="showSuccessAlert" variant="success" dismissible>
          {{ alertMessage }}
        </b-alert>
      </b-row>

      <b-row class="mt-3">
        <b-card>
          <b-row align-h="between">
            <b-col cols="6">
   
            </b-col>
            <b-col cols="2">
              <b-row>
                <b-col>
                  <b-button
                    variant="primary"
                    id="show-btn"
                    @click="showCreateModal"
                  >
                    <b-icon-plus class="text-white"></b-icon-plus>
                    <span class="h6 text-white">Nuevo Permiso</span>
                  </b-button>
                </b-col>
              </b-row>
            </b-col>
          </b-row>
          <b-row class="mt-3">
            <b-table
              striped
              hover
              :items="items"
              :fields="fields"
              class="text-center"
            >
              <template #cell(Acciones)="data">
                <b-row>
                  <b-col cols="7">
                    <b-icon-pencil-square
                      class="action-item"
                      variant="primary"
                      @click="getRowData(data.item.id)"
                    ></b-icon-pencil-square>
                  </b-col>
                  <b-col cols="1">
                    <b-icon-trash-fill
                      class="action-item"
                      variant="danger"
                      @click="showDeleteModal(data.item.id)"
                    ></b-icon-trash-fill>
                  </b-col>
                </b-row>
              </template>
            </b-table>
          </b-row>
        </b-card>
      </b-row>
  
      <!-- Modal for adding new customers -->
      <b-modal
        ref="create-permissions-modal"
        size="xl"
        hide-footer
        title="Nuevo Permiso"
      >
        <create-permissions-form
          @closeCreateModal="closeCreateModal"
          @reloadDataTable="getCustomerData"
          @showSuccessAlert="showAlertCreate"
        ></create-permissions-form>
      </b-modal>
  
      <!-- Modal for updating customers -->
      <b-modal
        ref="edit-permissions-modal"
        size="xl"
        hide-footer
        title="Editar Permiso"
      >
        <edit-permissions-form
          @closeEditModal="closeEditModal"
          @reloadDataTable="getCustomerData"
          @showSuccessAlert="showAlertUpdate"
          :PermissionId="PermissionId"
        ></edit-permissions-form>
      </b-modal>
  
      <!-- Delete Customer Modal -->
      <b-modal
        ref="delete-permissions-modal"
        size="md"
        hide-footer
        title="Confirmar"
      >
        <delete-permissions-modal
          @closeDeleteModal="closeDeleteModal"
          @reloadDataTable="getCustomerData"
          @showDeleteAlert="showDeleteSuccessModal"
          :PermissionId="PermissionId"
        ></delete-permissions-modal>
      </b-modal>
    </div>
  </template>
  
  <script>
  import axios from "axios";
  import CreatePermissionsForm from "@/components/CreatePermissionsForm.vue";
  import EditPermissionsForm from "@/components/EditPermissionsForm.vue";
  import DeletePermissionsModal from "@/components/DeletePermissionsModal.vue";
  
  export default {
    components: {
      CreatePermissionsForm,
      EditPermissionsForm,
      DeletePermissionsModal,
    },
    data() {
      return {
  
        fields: [
          {
            key: "id",
            label: "#",
            sortable: false,
          },
          {
            key: "firstnameEmployee",
            label: "Nombre",
            sortable: false,
          },
          {
            key: "lastnameEmployee",
            label: "Apellido",
            sortable: false,
          },
          {
            key: "datePermissions",
            label: "Fecha de Permiso",
            sortable: false,
          },
          {
            key: "typePermissionsDto.description",
            label: "Tipo de Permiso",
            sortable: false,
          },
          "Acciones",
        ],
        items: [],
        numberOfPermissions: 0,
        activePermissions: 0,
        activePermissionsData: [],
        PermissionId: 0,
        tableHeader: "",
        showSuccessAlert: false,
        alertMessage: "",
      };
    },
    mounted() {
      this.getCustomerData();
    },
    methods: {
      showCreateModal() {
        this.$refs["create-permissions-modal"].show();
      },
      closeCreateModal() {
        this.$refs["create-permissions-modal"].hide();
      },
      getCustomerData() {
        axios
          .get(`${this.$apiUrlBase}Permissions/GetPaged?PageNumber=1&PageSize=10`)
          .then((response) => {
            
            this.tableHeader = "Permisos";
            this.items = response.data.result.data;
            this.numberOfCustomers = response.data.result.data.length;
          })
          .catch((error) => {
            console.log(error);
          });
      },
      getRowData(id) {
        this.$refs["edit-permissions-modal"].show();
        this.PermissionId = id;
      },
      closeEditModal() {
        this.$refs["edit-permissions-modal"].hide();
      },
      showAlertCreate() {
        this.showSuccessAlert = true;
        this.alertMessage = "Permiso Creado!";
      },
      showAlertUpdate() {
        this.showSuccessAlert = true;
        this.alertMessage = "Permiso Editado";
      },
      showDeleteModal(id) {
        this.$refs["delete-permissions-modal"].show();
        this.PermissionId = id;
      },
      closeDeleteModal() {
        this.$refs["delete-permissions-modal"].hide();
      },
      showDeleteSuccessModal() {
        this.showSuccessAlert = true; 
        this.alertMessage = "Permiso Borrado !";
      },
    },
  };
  </script>
  
  <style>
  .action-item:hover {
    cursor: pointer;
  }
  </style>