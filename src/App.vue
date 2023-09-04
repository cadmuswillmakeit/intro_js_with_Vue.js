<script setup>
  import {ref} from "vue";

  const showModalInscription = ref(false);
  const showModalValidation = ref(false);

  const menuBurger = () => nav.classList.toggle("active"); 

  const liens = () => nav.classList.remove("active");

  const erreurBirthday = ref("");
  const erreurCondition = ref("");
  const erreurEmail = ref("");
  const erreurNbreTournoi = ref("");
  const erreurNom = ref("");
  const erreurPrenom = ref("");
  const erreurVille = ref("");

  const nom = ref("");
  const prenom = ref("");
  const email = ref("");
  const villes = ref("");
  const birthday = ref("");
  const nbreTournois = ref("");
  const conditions = ref(false);


  // Gestion des formulaires

  // script qui gère le formulaire

  // Les fonctions de vérification
  function verifierVille (ville) {
      if (ville.value !== "") {
          erreurVille.value = "";
          return true;
      }
      erreurVille.value = "Vous devez choisir une ville";
      return false;
  }

  function verifierEmail (e_mail) {
      let regex = new RegExp("[a-z0-9._-]+@[a-z0-9._-]+\.[a-z0-9._-]+");
      if (regex.test(e_mail)) {
          erreurEmail.value = "";
          return true;
      }
      erreurEmail.value = "Veuillez saisir un email valide";
      return false;
  }

  function verifierChaineDeCaracteres(chaine, type) {
      let regex = new RegExp("^[a-zA-Z]{3,}$");
      if (regex.test(chaine.trim())) {
          if (type === "prenom") {
              erreurPrenom.value = "";
          } else if (type === "nom") {
              erreurNom.value = "";
          }    
          return true;
      }
      if (type === "prenom") {
          erreurPrenom.value = "Veuillez saisir un prénom de 3 caractères et ne contenant pas de chiffre";
      } else if (type === "nom") {
          erreurNom.value = "Veuillez saisir un nom de 3 caractères et ne contenant pas de chiffre";
      }
      return false;
  }

  function verifierConditions (conditionApprouvees) {
      if (conditionApprouvees) {
          erreurCondition.value = "";
          return true;
      }
      erreurCondition.value = "Vous devez accepter les conditions d'utilisations";
      return false;
  }

  function nombreTournoisValide (nombre) {
      let regex = new RegExp("^[0-9]{1,3}$");
      if (regex.test(nombre)) {
          erreurNbreTournoi.value = "";
          return true;
      }
      erreurNbreTournoi.value = "Veuillez saisir un nombre de tournois valide";
      return false;
  }

  function differenceEnAnnees(dateParametre) {
      const dateActuelle = new Date();
      const dateFournie = new Date(dateParametre);
      const differenceEnMillisecondes = dateActuelle - dateFournie;
      const differenceEnAnnees = differenceEnMillisecondes / (1000 * 60 * 60 * 24 * 365);
      return Math.floor(differenceEnAnnees);
    }
    
  function validerDate (date) {
      const ageDuCandidat = differenceEnAnnees(date);
      if (ageDuCandidat < 0 || isNaN(ageDuCandidat)) {
          erreurBirthday.value = "Veuillez saisir une date valide";
          return false;  
      } else if(ageDuCandidat > 16) {
          erreurBirthday.value = "";
          return true;
      } else {
          erreurBirthday.value = "Vous devez avoir au minimum 16 ans";
          return false;
    
      }
  }

  const soumettreFormulaire = (event) => {
      event.preventDefault();
      const nomValide = verifierChaineDeCaracteres(nom.value, "nom");
      const prenomValide = verifierChaineDeCaracteres(prenom.value, "prenom");
      const emailValide = verifierEmail(email.value);
      const villeChoisie = verifierVille(villes);
      const conditionsAcceptees = verifierConditions(conditions.value)
      const nbreTournoisValide = nombreTournoisValide(nbreTournois.value);
      const birthdayValide = validerDate(birthday.value);
      const formulaireValide = nomValide && prenomValide && emailValide && villeChoisie && nbreTournoisValide && conditionsAcceptees && birthdayValide;

      if (formulaireValide) {
          prenom.value = '';
          nom.value = '';
          email.value = '';
          villes.value = '';
          birthday.value = '';
          nbreTournois.value = '';
          conditions.value = false;
          showModalInscription.value = false;
          showModalValidation.value = true;
      } 
  };    

</script>

<template>
  <header>
    <nav class="header-nav" id="nav">
        <div>Game On</div>
        <ul @click="liens">
            <li>
                <a href="#" class="red-bg">Détails de l'évènement</a>
            </li>
            <li>
                <a href="#">À propos</a>
            </li>
            <li>
                <a href="#">Contacts</a>
            </li>
            <li>
                <a href="#">Évènements passés</a>
            </li>
        </ul>
        <button id="menu_burger" @click="menuBurger"></button>
    </nav>       
  </header>
  <main>
      <section class="main-section">
          <div class="main-section__title">
              Marathon national <br> de jeux vidéos
          </div>
          <div class="main-section__secondchild">
              <p>
                  Vous aimez jouer ? Notre prochain évènement<br> gaming est ouvert aux réservations...<br> 
                  Places limitées !
              </p>
          </div>
          <div>
              <button title="s'inscrire" class="btnInscription" @click="showModalInscription = true"> Je m'inscris</button>
          </div>
      </section>
      <section class="sectionSecondaire">
          <div class="sectionSecondaire__title">
              Marathon national de jeux vidéos
          </div>    

          <div class="sectionSecondaire__secondchild">
              <p>
                  Vous aimez jouer ? Notre prochain évènement gaming est ouvert aux réservations...
              </p>
          </div>
          <div>
              <img src="/player-mobile.jpg" alt ="joueuse avec manette de jeu victorieuse">
          </div>
          <div>
              <button title="s'inscrire" class="btnInscriptionDeux" @click="showModalInscription = true"> Je m'inscris</button>
          </div>
      </section>    
      <section v-if="showModalInscription" class="modaleInscription">
          <form @submit.prevent="soumettreFormulaire">
              <div>
                  <button class="form__btnFermer" @click="showModalInscription = false">&#10005;</button>
              </div>
              <div>
                  <label for="prenom">Prenom</label><br><br>
                  <input v-model="prenom" type="text" name="prenom" id="prenom" placeholder="Votre prenom"><br>
                  <span v-if="erreurPrenom" class="erreur" id="erreurPrenom">{{ erreurPrenom }}</span>    
              </div>
              <div>
                  <label for="nom">Nom</label><br><br>
                  <input v-model="nom" type="text" name="nom" id="nom" placeholder="Votre nom">
                  <span v-if="erreurNom" class="erreur" id="erreurNom">{{ erreurNom }}</span>
              </div>
              <div>
                  <label for="email">E-mail</label><br><br>
                  <input v-model="email" type="email" name="email" id="email" placeholder="votremail@exemple.com"><br>
                  <span v-if="erreurEmail" class="erreur" id="erreurEmail">{{ erreurEmail }}</span>
              </div>
              <div>
                  <label for="birthday">Date de naissance</label><br><br>
                  <input v-model="birthday" type="date" name="birthday" id="birthday"><br>
                  <span v-if="erreurBirthday" class="erreur" id="erreurBirthday">{{ erreurBirthday }}</span>
              </div>
              <div>
                  <label for="nbreTournoi">A combien de tournois GameOn avez-vous déjà participé ?</label><br><br>
                  <input v-model="nbreTournois" type="number" name="nbreTournoi" id="nbreTournoi" placeholder="Tournois participés"><br>
                  <span v-if="erreurNbreTournoi" class="erreur" id="erreurNbreTournoi">{{ erreurNbreTournoi }}</span>
              </div>
              <div>
                  <label>A quel tournoi souhaitez-vous participer ?</label><br><br>
                  <input v-model="villes" type="radio" name="typeTournoi" id="newyork" value="New York">
                  <label for="newyork">New York</label>
                  <input v-model="villes" type="radio" name="typeTournoi" id="sanfrancisco" value="San Francisco">
                  <label for="sanfrancisco">San Francisco</label>
                  <input v-model="villes" type="radio" name="typeTournoi" id="seattle" value="Seattle">
                  <label for="seattle">Seattle</label>
                  <input v-model="villes" type="radio" name="typeTournoi" id="chicago" value="Chicago">
                  <label for="chicago">Chicago</label>
                  <input v-model="villes" type="radio" name="typeTournoi" id="boston" value="Boston">
                  <label for="boston">Boston</label>
                  <input v-model="villes" type="radio" name="typeTournoi" id="portland" value="Portland">
                  <label for="portland">Portland</label><br>
                  <span v-if="erreurVille" class="erreur" id="erreurVille">{{ erreurVille }}</span>
              </div>
              <div>
                  <input v-model="conditions" type="checkbox" name="conditions" id="conditions">
                  <label for="conditions">J'ai lu et accepté les conditions d'utilisation</label><br>
                  <span v-if="erreurCondition" class="erreur" id="erreurCondition">{{ erreurCondition }}</span>
                  <br>
                  <input type="checkbox" name="newsletter" id="newsletter">
                  <label for="newsletter">Je souhaite être prévenu des prochains évvènements</label>
              </div>
              <div class="btnDiv">
                  <input type="submit" name="envoyer" id="envoyer" value="C'est parti">
              </div>
          </form>
      </section>
      <section v-if="showModalValidation" class="modaleInscriptionValidee">
              <div class="modaleInscriptionEnregistree">
                  <div>
                      <button class="form__btnFermerDeux" @click="showModalValidation = false">&#10005;</button>
                  </div>
                  <div class="accueilMessage">
                      <p class="pUn">Merci pour</p>
                      <p class="pDeux">votre inscription</p>
                      <span class="span">Merci pour votre inscription</span>
                  </div>
                  <div class="btnDiv">
                      <button class="btnFermer" @click="showModalValidation = false">Fermer</button>
                  </div>            
              </div>
      </section>
  </main>     
  <footer>
      Copyright 2014-2022, GameOn Inc.
  </footer>

</template>

<style scoped>
@charset "UTF-8";
* {
  box-sizing: border-box;
}

body {
  background-color: white;
  font-family: roboto;
  font-size: 1em;
  margin: 0;
  padding: 0;
}
body ul {
  list-style: none;
}

a, main .main-section .btnInscription, header .header-nav .red-bg {
  text-decoration: none;
  color: black;
  padding: 10px;
}

main .main-section .btnInscription, header .header-nav .red-bg {
  background-color: red;
  color: white;
  border-radius: 6px;
}

header {
  padding: 0 10%;
  margin-block: 1rem;
  width: 100%;
  height: 100%;
}
header #menu_burger {
  cursor: pointer;
  display: none;
  color: red;
  font-weight: bold;
  border: 0;
  background-color: white;
}
header .header-nav > div {
  color: red;
  font-size: 3em;
  font-weight: bold;
}
header .header-nav {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
}
header .header-nav .red-bg {
  text-decoration: underline;
}

nav > ul {
  display: flex;
}

main {
  padding: 0 10%;
  display: flex;
  flex-direction: column;
  margin-block: 1rem;
}
main .sectionSecondaire {
  display: none;
}
main .main-section {
  padding: 35px 0 35px 4%;
  background: linear-gradient(to bottom, rgba(0, 0, 0, 0), rgba(0, 0, 0, 0.4)), center top/cover url("/player.png");
  color: white;
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 600px;
  border-radius: 12px;
  justify-content: space-around;
  gap: 20px;
}
main .main-section__title {
  width: 80%;
  font-size: 6rem;
  font-weight: 200;
}
main .main-section .btnInscription {
  border: 0;
  padding-left: 25px;
  padding-right: 25px;
  color: white;
  cursor: pointer;
}
main .modaleInscription, main .modaleInscriptionValidee {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  width: 100%;
  height: 100%;
  overflow: auto;
  background: rgba(51, 51, 51, 0.8274509804);
}
main form {
  top: 2%;
  left: 33.5%;
  max-width: 500px;
  min-width: 300px;
  background-color: #313131;
  color: white;
  position: relative;
  border-radius: 13px;
  padding: 12px 24px 16px 24px;
  display: flex;
  flex-direction: column;
  gap: 15px;
}
main form .form__btnFermer {
  cursor: pointer;
  position: absolute;
  top: 6px;
  right: 8px;
  align-items: flex-end;
  border: 0;
  background-color: #313131;
  color: white;
  justify-content: flex-end;
  font-weight: bold;
  font-size: 1.2em;
}
main form input[type=text], main form input[type=email], main form input[type=date], main form input[type=number] {
  width: 100%;
  height: 50px;
}
main form input[type=submit] {
  color: white;
  background-color: red;
  padding: 15px 35%;
  border: 0;
  border-radius: 6px;
}
main form .btnDiv {
  padding: 0 35%;
}
main form .erreur {
  font-size: 0.7em;
  color: red;
}
main form #envoyer {
  cursor: pointer;
}
main .modaleInscriptionEnregistree {
  top: 2%;
  left: 33.5%;
  max-width: 450px;
  min-width: 300px;
  font-family: "Times New Roman";
  background-color: #313131;
  color: white;
  height: 700px;
  position: relative;
  border-radius: 15px;
  padding: 12px 24px 16px 24px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
main .modaleInscriptionEnregistree .form__btnFermerDeux {
  cursor: pointer;
  position: absolute;
  top: 6px;
  right: 8px;
  align-items: flex-end;
  border: 0;
  background-color: #313131;
  color: white;
  justify-content: flex-end;
  font-weight: bold;
  font-size: 1.2em;
}
main .modaleInscriptionEnregistree .btnDiv {
  padding: 0 35%;
}
main .modaleInscriptionEnregistree .btnDiv .btnFermer {
  cursor: pointer;
  color: white;
  background-color: red;
  padding: 15px 37%;
  border: 0;
  border-radius: 6px;
}
main .modaleInscriptionEnregistree .accueilMessage {
  font-size: calc(1rem + 1vh);
}
main .modaleInscriptionEnregistree .accueilMessage .span {
  display: none;
}
main .modaleInscriptionEnregistree .accueilMessage .pUn {
  padding: 0 35.5%;
}
main .modaleInscriptionEnregistree .accueilMessage .pDeux {
  padding: 0 29%;
}



footer {
  padding: 1.5% 5%;
  color: red;
  font-size: 0.7em;
}

@media only screen and (max-width : 768px) {
  body {
    height: 100%;
  }
  header {
    padding: 0 5%;
    width: 100%;
    height: 100%;
  }
  header .header-nav > div {
    font-size: 1.5em;
  }
  header .header-nav .red-bg {
    background-color: skyblue;
    padding: 0;
    border: 0;
    color: red;
  }
  header #menu_burger {
    display: block;
  }
  header #menu_burger::before {
    content: "☰";
  }
  header .header-nav > ul {
    position: fixed;
    left: -100%;
    top: 5%;
    flex-direction: column;
    gap: 1em;
    width: 100%;
    height: 25%;
    text-align: center;
    transition: 0.25s;
    background-color: skyblue;
  }
  header .active #menu_burger::before {
    content: "✕";
  }
  header .active ul {
    left: 0;
  }
  main {
    padding: 0 5%;
  }
  main .main-section {
    display: none;
  }
  main .sectionSecondaire {
    display: flex;
    margin: 0;
    width: 100%;
    /*display: flex;*/
    flex-direction: column;
    color: black;
    background: white;
    height: auto;
    gap: 20px;
  }
  main .sectionSecondaire img {
    width: 100%;
    height: auto;
    border-radius: 6px;
  }
  main .sectionSecondaire__title {
    font-size: 1.5em;
  }
  main .sectionSecondaire .btnInscriptionDeux {
    border: 0;
    padding: 15px;
    display: inline-block;
    margin-left: 37%;
    cursor: pointer;
    background-color: red;
    color: white;
    border-radius: 6px;
  }
  main form, main .modaleInscriptionEnregistree {
    width: auto;
    left: 0;
  }
  main .modaleInscriptionEnregistree {
    min-height: 350px;
  }
  main .modaleInscriptionEnregistree .accueilMessage .pUn, main .modaleInscriptionEnregistree .accueilMessage .pDeux {
    display: none;
  }
  main .modaleInscriptionEnregistree .accueilMessage .span {
    display: block;
    padding: 0 25%;
    line-height: 1.4em;
  }
  footer {
    width: 100%;
    display: flex;
    flex-direction: row;
    justify-content: center;
  }
}

/*# sourceMappingURL=style.css.map */

</style>