<template>
<v-container>
    <div class="main-div shadow mt-5">
        <div class="row">
            <div class="col-lg-5">
                <v-text-field v-model="search" append-icon="mdi-magnify" label="Search" single-line hide-details></v-text-field>
                <div class="d-flex align-items-center mt-5">
                    <v-text-field v-model="name" label="Name" dense outlined hide-details></v-text-field>
                    <v-btn color="primary" class=" ml-5" @click="addUser">
                        Add
                    </v-btn>
                </div>
                <v-data-table :headers="headers" :items="users" class="mt-5" :search="search">
                    <template v-slot:[`item.view`]="{ item }">
                        <v-btn color="success" @click="handleClick(item.id)">
                            View
                        </v-btn>
                    </template>
                </v-data-table>
            </div>
            <div class="col-lg-7">

                <v-text-field v-model="searchHobby" append-icon="mdi-magnify" label="Search" single-line hide-details></v-text-field>
                <div class="row mt-2">
                    <div class="col-lg-3">
                        <v-select :items="passion" v-model="selectedPassion" label="Passion" dense outlined hide-details></v-select>
                    </div>
                    <div class="col-lg-4">
                        <v-text-field v-model="hobbyName" label="Hobby" dense outlined hide-details></v-text-field>
                    </div>
                    <div class="col-lg-3">
                        <v-text-field v-model="year" label="Year" dense outlined hide-details></v-text-field>
                    </div>
                    <div class="col-lg-2">
                        <v-btn color="success" @click="addHobby">
                            Add
                        </v-btn>
                    </div>
                </div>
                <v-data-table :headers="Hobbyheaders" :items="hobby" class="mt-5" :search="searchHobby">
                    <template v-slot:[`item.delete`]="{ item }">
                        <v-btn color="error" @click="deleteHobby(item.id)">
                            Delete
                        </v-btn>
                    </template>
                </v-data-table>
            </div>
        </div>
    </div>
</v-container>
</template>

<script>
// import axios from 'axios'
import usersData from '../assets/data.json';
import hobbyData from '../assets/hobby.json';
import swal from 'sweetalert';
export default {
    data() {
        return {
            Hobbyheaders: [{
                    text: "Sr.No",
                    value: "id",
                }, {
                    text: "Passion",
                    value: "passion",
                },
                {
                    text: "Hobby",
                    value: "hobby",
                },
                {
                    text: "Year",
                    value: "year",
                },
                {
                    text: "Action",
                    value: "delete",
                },
            ],
            headers: [{
                    text: "Sr.No",
                    value: "id",
                },
                {
                    text: "Name",
                    value: "name",
                },
                {
                    text: "Action",
                    value: "view",
                },

            ],
            search: '',
            passion: ['High', 'Medium', 'Low'],
            searchHobby: '',
            users: usersData,
            hobby: hobbyData,
            userid: '',
            name: "",
            year: "",
            hobbyName: "",
            selectedPassion: "",
        }
    },
    methods: {
        handleClick(id) {
            this.userid = id;
            this.hobby = hobbyData.filter((item) => item.id == this.userid);
        },
        addUser() {
            this.users.push({
                id: this.users.length + 1,
                name: this.name,
                action: 'view'
            });
        },
        addHobby() {
            this.hobby = hobbyData;
            this.hobby.push({
                id: this.hobby.length + 1,
                passion: this.selectedPassion,
                hobby: this.hobbyName,
                year: this.year,
                action: 'delete'

            });
        },
        deleteHobby(id) {
            swal({
                    title: "Are you sure?",
                    text: "Once Deleted it cannot be restored!",
                    icon: "warning",
                    buttons: true,
                    dangerMode: true,
                })
                .then((willDelete) => {
                    if (willDelete) {
                        this.hobby.splice(this.hobby.indexOf(id), 1);
                        swal("Your Hobby Book has been deleted!", {
                            icon: "success",
                        });
                    } else {
                        swal("Your  file is safe!");
                    }
                });

        },
        deleteDialog() {

        }
    },
    mounted() {
        this.hobby = [];

    }
}
</script>

<style scoped>
.main-div {
    padding: 15px;
    background: var(--vs-theme-layout);
    border-radius: 20px;
    /* background: rgba( 255, 255, 255, 0.4 );
    box-shadow: 0 8px 32px 0 rgba( 31, 38, 135, 0.37 );
    backdrop-filter: blur( 10px );
    -webkit-backdrop-filter: blur( 10px );
    border: 1px solid rgba( 255, 255, 255, 0.18 ); */
}

.shadow {
    box-shadow: 0 0.5rem 1rem rgba(0, 0, 0, 0.15) !important;
}
</style>
