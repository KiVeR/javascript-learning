<template>
    <div class="container">
        <div v-if="currentStep === availableStep['initialStep']"
             class="bg-inverse text-white box-padding">

            <h1>Que veux-tu faire ?</h1>

            <br>

            <button class="btn btn-primary" @click="updateStep(1)">Lister les contacts</button>
            <button class="btn btn-success" @click="updateStep(2)">Ajouter un contact</button>
            <a class="btn btn-warning" href="/">Quitter</a>
        </div>

        <table v-else-if="currentStep === availableStep['showTable']"
               class="table table-inverse">
            <thead>
            <tr>
                <th>#</th>
                <th>Prénom</th>
                <th>Nom</th>
            </tr>
            </thead>
            <tbody>
            <tr v-for="(contact, index) in contacts">
                <th scope="row">{{ index }}</th>
                <td>{{ contact.firstName }}</td>
                <td>{{ contact.lastName }}</td>
            </tr>
            </tbody>
        </table>


        <div v-else-if="currentStep === availableStep['addContact']"
             class="bg-inverse text-white">
            <form class="box-padding">
                <fieldset>
                    <legend>Ajouter un contact</legend>

                    <br>

                    <div class="form-group row">
                        <label for="firstName" class="col-2 col-form-label">Prénom</label>
                        <div class="col-10">
                            <input class="form-control"
                                   type="text"
                                   id="firstName"
                                   v-model="newContact.firstName">
                        </div>
                    </div>

                    <div class="form-group row">
                        <label for="lastName" class="col-2 col-form-label">Nom</label>
                        <div class="col-10">
                            <input class="form-control"
                                   type="text"
                                   id="lastName"
                                   v-model="newContact.lastName">
                        </div>
                    </div>

                    <br>

                    <button type="button" class="btn btn-success" @click="addContact(newContact)">Ajouter</button>

                    <button type="button" class="btn btn-danger" @click="resetStep()">Annuler</button>
                </fieldset>
            </form>
        </div>

        <div v-else-if="currentStep === availableStep['contactAdded']"
             class="bg-inverse text-white box-padding">
            <h1>Bravo</h1>

            <p>Tu as ajouté : <b>{{ newContact.firstName }} {{ newContact.lastName }}</b></p>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                /* STEP DATA */
                currentStep: null,

                availableStep: {
                    initialStep: 0,
                    showTable: 1,
                    addContact: 2,
                    contactAdded: 3,
                },

                /* CONTACT DATA */
                contacts: [
                    {firstName: 'Carole', lastName: 'Levisse'},
                    {firstName: 'Mélodie', lastName: 'Nelsonne'},
                ],

                newContact: null,
            }
        },

        /* Lancer quand le composant de charge */
        mounted() {
            console.log('--- STEP : initialStep ---');
            this.initStep();
            this.initNewContact();
        },

        methods: {
            initStep() {
                console.info('initStep()');

                this.currentStep = this.availableStep['initialStep'];
            },

            resetStep() {
                this.initStep()
            },

            updateStep(newStep) {
                console.info('updateStep()', newStep);

                this.currentStep = newStep;

                if (this.currentStep === this.availableStep['showTable']) {
                    console.log('--- STEP : showTable ---');

                    setTimeout(() => {
                        this.resetStep();
                    }, 2000);
                } else if (this.currentStep === this.availableStep['contactAdded']) {
                    console.log('--- STEP : contactAdded ---');

                    setTimeout(() => {
                        this.resetStep();
                        this.resetNewContact();
                    }, 2000);
                }
            },


            addContact(newContact) {
                console.info('addContact()');

                this.contacts.push(newContact);

                this.updateStep(this.availableStep['contactAdded']);
            },

            initNewContact() {
                console.info('initNewContact()');

                this.newContact = {
                    firstName: '',
                    lastName: '',
                }
            },

            resetNewContact() {
                this.initNewContact();
            },
        }
    }
</script>
