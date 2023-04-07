<script setup>
import {ref, onMounted} from "vue";

const data = ref('');
const libelle = ref('');
const rue = ref('');
const codePostal = ref('');
const ville = ref('');
const id = ref('');

// Avec adresse
async function ajouter() {
    const dataPost = JSON.stringify({
        libelle: libelle.value,
        adresse: {
            rue: rue.value,
            codepostal: codePostal.value,
            ville: ville.value,
        },
    })
    if (id.value !== '') {
        await fetch(`https://localhost:8000/api/fournisseurs/${id.value}`, {
            method: 'PUT',
            headers: {
                'Content-Type': 'application/json',
            },
            body: dataPost,
        }).then(async () => {
            getFournisseurs()
        })
    } else {
        await fetch('https://localhost:8000/api/fournisseurs', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
            },
            body: dataPost,
        }).then(async () => {
            getFournisseurs()
        })
    }
    getFournisseurs();
}

const getFournisseurs = async () => {
    const response = await fetch('https://localhost:8000/api/fournisseurs?page=1');
    data.value = await response.json();
}

onMounted(async() => {
    getFournisseurs();
})

async function supprimer(id) {
    await fetch(`https://localhost:8000${id}`, {
        method: 'DELETE',
        headers: {
            'Content-Type': 'application/json',
        },
    })
  getFournisseurs();
}

async function modifier(_id) {
    await fetch(`
    https://localhost:8000${_id}`).then((response) => {
            response.json().then((fournisseur) => {
                id.value = fournisseur.id
                libelle.value = fournisseur.libelle
                rue.value = fournisseur.adresse.rue
                codePostal.value = fournisseur.adresse.codepostal
                ville.value = fournisseur.adresse.ville
            })
        },
    )
}
</script>

<template>
    <main>
        <h1>Fournisseurs</h1>

        <h2>Ajouter un fournisseur</h2>
        <label for="libelle">libellé</label>
        <input type="text" id="libelle" v-model="libelle">
                    <p>Adresse</p>
                    <label for="rue">rue</label>
                    <input type="text" id="rue" v-model="rue">
                    <label for="codePostal">code postal</label>
                    <input type="text" id="codePostal" v-model="codePostal">
                    <label for="ville">ville</label>
                    <input type="text" id="ville" v-model="ville">
        <button @click="ajouter">Ajouter</button>


        <div v-if="data">
            <h2>Il y a : {{ data['hydra:totalItems'] }} fournisseurs</h2>
            <div v-for="fournisseur in data['hydra:member']">
                <ul>
                    <li>
                        {{ fournisseur.libelle }}
                    </li>
                    <li v-if="fournisseur.adresse">
                        {{ fournisseur.adresse.rue }}
                        {{ fournisseur.adresse.codepostal }}
                        {{ fournisseur.adresse.ville }}
                    </li>
                </ul>
                <button @click="supprimer(fournisseur['@id'])">Supprimer</button>
                <button @click="modifier(fournisseur['@id'])">Modifier</button>
            </div>
        </div>

        <pre>
                {{ data }}
            </pre>

    </main>
</template>
