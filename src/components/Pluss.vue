<template>
    <div class="text-center">
        <h1 class="p-5">
            Plüssök
        </h1>
    <table class="m-auto p-5">
        <thead>
            <tr class="f-4">
                <th>Azonosító</th>
                <th>Név</th>
            </tr>
        </thead>
        <tbody>
                <tr v-for="p in plussok" :key="p.id">
                    <td>{{ p.id }}</td>
                    <td>{{ p.nev }}</td>
                    <td>
                        <button class="btn btn-danger w-100" @click="deletePluss(p.id)">Törlés</button>
                        <br>
                        <button class="btn btn-primary w-100"  @click="editPluss(p.id)">Szerkesztés</button>
                    </td>
                </tr>
                <tr>
                    <td></td>
                    <td>
                        <input type="text" v-model="pluss.nev">
                    </td>
                    <td>
                        <button @click="newPluss" :disabled="saving" v-if="!add_new">Hozzáadás</button>
                        <button v-if="add_new" @click="savePluss">Mentés</button>
                        <br>
                        <button v-if="add_new" @click="cancelPluss">Mégse</button>
                    </td>
                </tr>
        </tbody>
    </table>
    </div>
</template>

<script>
import axios from "axios"

export default {
    nev: 'Pluss',

    data() {
        return {
            plussok: [],
            pluss: {
                nev: "",
            },
            add_new: false,
            saving: false
            }
    },

    methods: {
        async loadData() {
            await axios
                .get('http://127.0.0.1:8000/api/pluss')
                .then(response => (this.plussok = response.data))
                .catch(error => console.log(error))
        },
    
        async newPluss() {
            this.saving = true
            await axios
                .post('http://127.0.0.1:8000/api/pluss', this.pluss)
                .catch(error => console.log(error))
            await this.loadData()
            
            this.saving = false
            this.resetForm()
        },

        async deletePluss(id) {
            await axios
                .delete(`http://127.0.0.1:8000/api/pluss/${id}`)
                .catch(error => console.log(error))
            await this.loadData()
        },

        async editPluss(id) {
            this.add_new = true
            let response = await fetch(`http://127.0.0.1:8000/api/pluss/${id}`)
            this.pluss = await response.json()
        },

        async savePluss() {
            this.saving = 'disabled'
            await axios
                .patch(`http://127.0.0.1:8000/api/pluss/${this.pluss.id}`, this.pluss)
            await this.loadData()
            this.resetForm()
            this.saving = false
        },

        cancelPluss() {
            this.resetForm()
        },

        resetForm() {
            this.pluss = {
                nev: "",
            },
            this.add_new = false
        }
    },
    
    mounted() {
        this.loadData()
    }
}
</script>

<style scoped>
    table {
        background: #222629;
    }
</style>