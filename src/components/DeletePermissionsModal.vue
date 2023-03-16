<template>
    <div>
      <b-row class="mt-2 mb-3">
        <h6 class="text-secondary">
          ¿Está seguro de que desea eliminar este Permiso?
        </h6>
      </b-row>
      <b-row class="mt-2 mb-3">
        <p class="text-danger">
          Esta acción no es reversible y puede resultar en la pérdida si es importante
          datos.
        </p>
      </b-row>
      <b-row class="mt-4">
        <b-col>
          <b-button variant="danger" @click="removeCustomerFromData"
            >Borrar permiso</b-button
          >
        </b-col>
        <b-col>
          <b-button variant="warning" @click="triggerClose">Cerrar</b-button>
        </b-col>
      </b-row>
    </div>
  </template>
  
  <script>
  import axios from "axios";
  
  export default {
    name: "DeletePermissionModal",
    props: {
      PermissionId: Number,
    },
    methods: {
      triggerClose() {
        this.$emit("closeDeleteModal");
      },
      removeCustomerFromData() {
        axios
          .delete(`${this.$apiUrlBase}Permissions/DeletePermissions/${this.PermissionId}`)
          .then(() => {
            this.$emit("reloadDataTable");
            this.$emit("showDeleteAlert");
            this.$emit("closeDeleteModal");
          })
          .catch((error) => {
            console.log(error);
          });
      },
    },
  };
  </script>