<template>
    <div class="text-center pb-5">
        <p class="fs-1 fw-bold text-uppercase text-decoration-underline p-5">Plüssök</p>
    <table class="m-auto w-75">
        <thead>
            <tr class="fs-2">
                <th>Azonosító</th>
                <th>Név</th>
            </tr>
        </thead>
        <tbody class="fs-4">
                <tr v-for="p in plussok" :key="p.id">
                    <td class="w-25">{{ p.id }}</td>
                    <td class="w-50">{{ p.nev }}</td>
                    <td class="w-25">
                        <button class="btn btn-danger w-75 fs-5" @click="deletePluss(p.id)">Törlés</button>
                        <br>
                        <button class="btn btn-primary w-75 fs-5" @click="editPluss(p.id)">Szerkesztés</button>
                    </td>
                </tr>
                <tr class="mt-5">
                    <td class="w-25"></td>
                    <td class="w-50">
                        <input class="w-100" type="text" v-model="pluss.nev">
                    </td>
                    <td class="p-5 w-25">
                        <button class="btn btn-success w-75 fs-5" @click="newPluss" :disabled="saving" v-if="!add_new">Hozzáadás</button>
                        <button class="btn btn-primary w-75 fs-5" v-if="add_new" @click="savePluss">Mentés</button>
                        <br>
                        <button class="btn btn-danger w-75 fs-5" v-if="add_new" @click="cancelPluss">Mégse</button>
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

            this.resetForm()
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