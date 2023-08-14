<template>
    <div>
        <!-- Page Heading -->
        <div class="d-sm-flex align-items-center justify-content-between mb-4">
            <h1 class="h3 mb-0 text-gray-800">Gestion des employee</h1>
        </div>
        <div class="row">
            <div class="card border-success mx-auto">
                <div v-if="showMessage">
                    <div class="alert alert-success alert-dismissible fade show" role="alert">
                        <strong>{{ message }}</strong>
                        <button type="button" class="close" data-dismiss="alert" aria-label="Close">
                        <span aria-hidden="true">&times;</span>
                        </button>
                    </div>
                </div>
                <div class="card-header">
                    <div class="row">
                        <div class="col">
                            <form>
                                <div class="form-row align-items-center">
                                <div class="col">
                                    <input type="search" v-model.lazy="search" class="form-control mb-2" placeholder="Rechercher..." />
                                </div>
                                <div class="col">
                                    <select v-model="selectedDeprtment" name="city" class="form-control mb-2" aria-label="Default select example">
                                        <option v-for="department in departments" :key="department.id" :value="department.id">
                                        {{ department.name }}
                                        </option>
                                    </select>
                                </div>
                                <div class="col">
                                    <button type="submit" class="btn btn-primary mb-2">Recherche</button>
                                </div>
                                </div>
                            </form>
                        </div>
                        <div>
                            <router-link :to="{ name: 'EmployeesCreate' }" class="btn btn-primary mb-2">Ajouter</router-link>
                        </div>
                    </div>
                </div>
                <div class="card-body">
                    <table class="table">
                        <thead>
                            <tr>
                                <th scope="col">#Id</th>
                                <th scope="col">Prénom</th>
                                <th scope="col">Nom</th>
                                <th scope="col">Adresse</th>
                                <th scope="col">Department</th>
                                <th scope="col">Actions</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="employee in employees" :key="employee.id">
                                <th scope="row">#{{ employee.id }}</th>
                                <td>{{ employee.first_name }}</td>
                                <td>{{ employee.last_name }}</td>
                                <td>{{ employee.address }}</td>
                                <td>{{ employee.department.name }}</td>
                                <td>
                                    <router-link :to="{ name: 'EmployeesEdit', params: { id: employee.id } }" class="btn btn-success">Modifier</router-link>
                                    <button class="btn btn-danger" @click="deleteEmployee(employee.id)">Supprimer</button>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                employees: [],
                showMessage: false,
                message: "",
                search: null,
                selectedDeprtment: null,
                departments: []
            };
        },
        watch: {
            search() {
                this.getEmployees();
            },
            selectedDeprtment() {
                this.getEmployees();
            }
        },
        created() {
            this.getEmployees();
            this.getDepartments();
        },
        methods: {
            getEmployees() {
                axios.get("/api/employees", { params: {search: this.search, department_id: this.selectedDeprtment } })
                    .then(res => {this.employees = res.data.data;})
                    .catch(error => {console.log(error);});
            },
            getDepartments() {
                axios.get("/api/employees/departments")
                    .then(res => { this.departments = res.data; })
                    .catch(error => {console.log(console.error);});
            },
            deleteEmployee(id) {
                axios.delete("api/employees/" + id)
                .then(res => {
                    this.showMessage = true;
                    this.message = res.data;
                    this.getEmployees();
                });
            }
        }
    };
</script>

<style></style>
