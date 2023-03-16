<template>
  <b-form class="mt-3">
    <b-row>
      <b-col cols="6">
        <b-form-group id="first-name" label="First Name" label-for="first-name">
          <b-form-input id="first-name" type="text" placeholder="First Name"
            v-model="permission.firstnameEmployee"></b-form-input>
        </b-form-group>
      </b-col>
      <b-col cols="6">
        <b-form-group id="last-name" label="Last Name" label-for="last-name">
          <b-form-input id="last-name" type="text" placeholder="Last Name"
            v-model="permission.lastnameEmployee"></b-form-input>
        </b-form-group>
      </b-col>
    </b-row>
    <b-row class="mt-3">
      <b-col cols="6">
        <label for="example-datepicker">Fecha de Permiso</label>
        <b-form-datepicker id="example-datepicker" v-model="permission.datePermissions" class="mb-2"></b-form-datepicker>
      </b-col>
      <b-col cols="6">
        <label for="docente">Tipo de Permiso</label>
        <div class="card flex justify-content-center">
          <select v-model="typePermissionSelect" class="form-control" id="docente">
            <option v-for="item in typePermissions" :key="item.id">{{ item.description }}</option>
          </select>
        </div>
      </b-col>
    </b-row>
    <b-row class="mt-4">
      <b-col cols="3">
        <b-button variant="primary" class="px-5" @click="updatePermissions">Editar</b-button>
      </b-col>
      <b-col>
        <b-button variant="warning" @click="triggerClose">Cerrar</b-button>
      </b-col>
    </b-row>
  </b-form>
</template>
  
<script>
import axios from "axios";

export default {
  name: "EditPermissionsForm",
  props: {
    PermissionId: Number,
  },
  data() {
    return {
      permission: {},
      typePermissions: [],
      typePermissionSelect: {}
    };
  },
  mounted() {
    this.getPermissionByID();
    this.getTypePermissions();
  },
  methods: {
    triggerClose() {
      this.$emit("closeEditModal");
    },
    validateSelection() {

    },
    getTypePermissions() {
      axios
        .get(`${this.$apiUrlBase}TypePermissions/GetListTypePermissions`)
        .then((response) => {
          this.typePermissions = response.data;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    getPermissionByID() {
      axios
        .get(`${this.$apiUrlBase}Permissions/GetOne/${this.PermissionId}`)
        .then((response) => {
          this.permission = response.data.result;
          this.typePermissionSelect = this.permission.typePermissionsDto.description
        })
        .catch((error) => {
          console.log(error);
        });
    },
    updatePermissions() {
      this.permission.idtypePermission = this.typePermissions.find(c=> c.description == this.typePermissionSelect).id;
      axios
        .put(
          `${this.$apiUrlBase}Permissions/UpdatePermission`,
          this.permission
        )
        .then((response) => {
          console.log(response.data);
          this.$emit("closeEditModal");
          this.$emit("reloadDataTable");
          this.$emit("showSuccessAlert");
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>