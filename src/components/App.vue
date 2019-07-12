<template>
    <div class="container">
        <div>
            <nav class="navbar navbar-dark bg-dark">
                <a class="navbar-brand text-white mb-0 h1">{{title}}</a>
            </nav>
        </div>
        <div>
            <table class="mt-5 table">
                <thead>
                    <tr>
                        <td>
                            <button class="btn btn-primary" @click="showModal = true">Adicionar Knight</button>
                        </td>
                    </tr>
                    <tr>
                        <th>Nome</th>
                        <th>Idade</th>
                        <th>Armas</th>
                        <th>Atributo</th>
                        <th>Ataque</th>
                        <th>Exp</th>
                        <th>Update</th>
                        <th>Delete</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="knight in knights">
                        <td>{{knight.name}}</td>
                        <td>{{calculatingAge(knight.birthday)}}</td>
                        <td>{{numberOfWeapons(knight)}}</td>
                        <td>{{knight.keyAttribute}}</td>
                        <td>{{calculateAttack(knight)}}</td>
                        <td>{{calculateExp(knight.birthday)}}</td>
                        <td>
                            <button class="btn btn-success" @click="showModal = true">Update Knight</button>
                        </td>
                        <td>
                            <button class="btn btn-danger">Delete Knight</button>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>

        <!---->
        <div v-if="showModal === true" class="modal fade" id="exampleModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
            <div class="modal-dialog" role="document">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="exampleModalLabel">Modal title</h5>
                        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                            <span aria-hidden="true">&times;</span>
                        </button>
                    </div>
                    <div class="modal-body">
                        ...
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                        <button type="button" class="btn btn-primary">Save changes</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import axios from 'axios'
    export default {
        name: "App",
        data: function() {
            return {
                title: "Petra Gold Frontend Challenge",
                showModal: false,
                knights: [
                    {
                        _id: 1,
                        name: "Sr. Morte",
                        nickname: "",
                        birthday: 20001022,
                        weapons: [
                            {
                                _id: 1,
                                name: "sword",
                                mod: 3,
                                attr: "strength",
                                equipped: false,
                                _v: 0
                            },
                            {
                                _id: 2,
                                name: "katana",
                                mod: 5,
                                attr: "strength",
                                equipped: true,
                                _v: 0
                            },
                            {
                                _id: 3,
                                name: "long composite bow",
                                mod: 3,
                                attr: "dexterity",
                                equipped: false,
                                _v: 0
                            },
                        ],
                        attributes: {
                            strength: 18,
                            dexterity: 12,
                            constitution: 15,
                            intelligence: 12,
                            wisdom: 10,
                            charisma: 10
                        },
                        keyAttribute: "strength",
                        _v: 0
                    }
                ]
            }
        },
        methods: {
            calculatingAge(birthday) {
                let today = new Date(),
                    knightBirthday = new Date(birthday)
                let age = today.getFullYear() - knightBirthday.getFullYear()
                if (today.getMonth() < knightBirthday.getMonth()) {
                    age -= 1
                }
                if (today.getMonth() == knightBirthday.getMonth() && today.getDay() < knightBirthday.getDay()) {
                    age -= 1
                }
                return age
            },
            numberOfWeapons(knight) {
                let numberOfWeapons = knight.weapons
                return numberOfWeapons.length
            },
            calculateAttack(knight) {
                let weapons = knight.weapons
                let attack = 0
                weapons.forEach((weapon) => {
                    if (weapon.equipped == true) {
                        attack += weapon.mod
                    }
                    return attack
                })
                 attack = 10 + attack + this.calculateMod(knight.attributes[knight.keyAttribute])
                return attack
            },
            calculateMod(attribute) {
                let mod = Math.ceil(Math.max(attribute, 0) / 2) - 6
                return mod
            },
            calculateExp(birthday) {
                let exp = Math.floor((this.calculatingAge(birthday) - 7) * Math.pow(22, 1.45))
                return exp
            },
            createKnight(knight) {
                const url = '127.0.0.1:5000/api/v1/controller/knights/'
                return axios.post(url, knight)
            },
            updateKnight(knight) {
                const url = `127.0.0.1:5000/api/v1/controller/knights/${knight.id}`
                return axios.put(url, knight)
            },
            deleteKnight(knight) {
                const url = `127.0.0.1:5000/api/v1/controller/knights/${knight.id}`
                return axios.delete(url)
            }
        },
        mounted() {
            axios.get('127.0.0.1:5000/api/v1/controller/knights')
                .then(res => {
                    this.knights = res.data
                })
                .catch(error => console.error(error))
        }
    }
</script>

<style scoped>

</style>
