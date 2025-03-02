<script setup>
import { ref } from "vue";

const url = "https://apipharmacie.pecatte.fr/api/1/medicaments";
const denomination = ref("");
const formepharmaceutique = ref("");
const qte = ref(1);
const photoBase64 = ref("");

// Gestion du fichier photo
function handleFileUpload(event) {
  const file = event.target.files[0];
  if (!file) return;

  const reader = new FileReader();
  reader.onload = () => {
    photoBase64.value = reader.result;
  };
  reader.readAsDataURL(file);
}

// ======== POST Médicament ===============
function addMedicament() {
  const medicament = {
    denomination: denomination.value,
    formepharmaceutique: formepharmaceutique.value,
    qte: qte.value,
    photo: photoBase64.value,
  };

  fetch(url, {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(medicament),
  })
    .then((response) => response.json())
    .then(() => {
      // Réinitialiser le formulaire
      denomination.value = "";
      formepharmaceutique.value = "";
      qte.value = 1;
      photoBase64.value = "";
    })
    .catch((error) => console.error("Erreur lors de l'ajout :", error));
}
</script>

<template>
  <h2>Ajouter un Médicament</h2>
  <form @submit.prevent="addMedicament">
    <input v-model="denomination" placeholder="Nom du médicament" required />
    <input v-model="formepharmaceutique" placeholder="Forme" required />
    <input v-model="qte" type="number" placeholder="Quantité" required />
    <input type="file" @change="handleFileUpload" />
    <button type="submit">Ajouter</button>
  </form>
</template>
