<template>
<v-container>
    <div class="main-div shadow mt-5">
        <div class="row">
            <div class="col-lg-5">
                <v-text-field v-model="search" append-icon="mdi-magnify" label="Search" single-line hide-details></v-text-field>
                <div class="d-flex align-items-center mt-5">
                    <v-text-field v-model="name" label="Name"  dense outlined hide-details required></v-text-field>
                    <v-btn color="primary" class=" ml-5" @click="addUser">
                        Add
                    </v-btn>
                </div>
                <v-data-table :headers="headers" :items="users" class="mt-5" :search="search">
                    <template v-slot:item="{ item }">
                        <tr @click="viewHobbyList(item.id) ">
                            <td>{{item.id}}</td>
                            <td>{{item.name}}</td>

                        </tr>
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
                    <template v-slot:item="{ item }">
                        <tr v-if="item.status">
                            <td>{{item.passion}}</td>
                            <td>{{item.hobby}}</td>
                            <td>{{item.year}}</td>
                            <td>
                                <v-btn color="error" @click="deleteHobby(item.id)">
                                    Delete
                                </v-btn>
                            </td>
                        </tr>
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

            ],
            search: '',
            passion: ['High', 'Medium', 'Low'],
            searchHobby: '',
            users: [],
            hobby: [],
            userid: '',
            name: "",
            year: "",
            hobbyName: "",
            selectedPassion: "",
        }
    },
    methods: {
        viewHobbyList(id) {
            this.userid = id;
            this.hobby.forEach(hobby => {
                hobby.status = false;
                if (hobby.user_id == id) {
                    hobby.status = true;
                }
            })

        },
        addUser() {
            this.userid = this.users.length + 1;
            this.users.push({
                id: this.userid,
                name: this.name,
                action: 'view'
            });
        },
        addHobby() {
            if (this.userid) {
                this.hobby.push({
                    id: this.hobby.length + 1,
                    passion: this.selectedPassion,
                    hobby: this.hobbyName,
                    year: this.year,
                    status: true,
                    user_id: this.userid,
                    action: 'delete'

                });
            } else {
                swal({
                    title: "User Not Selected",
                    text: "Please select user first!",
                    icon: "info",
                    buttons: true,
                    dangerMode: false
                })
            }

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
                        this.hobby = this.hobby.filter((item) => item.id != id);
                        // this.hobby.splice(this.hobby.indexOf(id), 1);
                        swal("Your Hobby Book has been deleted!", {
                            icon: "success",
                        });
                    } else {
                        swal("Your  file is safe!");
                    }
                });

        },

    },

    mounted() {
        this.hobby = hobbyData;
        this.users = usersData;

    },

}
</script>

<style scoped>
.main-div {
    padding: 15px;
    background: var(--vs-theme-layout);
    border-radius: 20px;
}

.shadow {
    box-shadow: 0 0.5rem 1rem rgba(0, 0, 0, 0.15) !important;
}


</style>
