# Prototype interactif de permis biométrique pour la RDC

Ce document fournit une structure de maquette destinée à démontrer un système de gestion de permis biométrique. L'objectif est de disposer d'une démonstration visuelle (via Framer ou tout autre outil) à présenter aux ministères, investisseurs et équipes techniques.

## 1. Architecture générale

### Frontend (exemple avec React)

- **Page d'accueil** :
  - Logo et drapeau de la RDC (images à placer dans `src/assets/`).
  - Image de fond représentant la ville de Goma en arrière-plan.
  - Liens vers l'inscription citoyen et la connexion agent.

- **Interface citoyen** :
  - Formulaire d'enregistrement comportant : prénom, nom, sexe, date de naissance, groupe sanguin, adresse (ex. Goma, Nord-Kivu), téléversement de photo et de pièce d'identité.
  - Validation par code OTP envoyé par SMS (simulé).
  - Tableau de suivi du dossier avec 5 statuts : `En attente`, `En traitement`, `Accepté`, `Test pratique`, `Permis délivré`.
  - Affichage d'un QR Code après acceptation finale, avec option de télécharger un permis PDF simulé.

- **Interface agent** :
  - Écran de connexion sécurisé (mot de passe + code 2FA).
  - Liste des dossiers citoyens avec possibilité de valider/rejeter les pièces, planifier un rendez-vous pour le test pratique, approuver le dossier.
  - Génération d'un numéro de permis unique et export CSV/JSON.

### Backend (exemple Node.js/Express)

- API REST ou GraphQL pour gérer les opérations :
  - `/api/register` : enregistrement citoyen.
  - `/api/verify-otp` : validation OTP.
  - `/api/status/:id` : récupération du statut d'un dossier.
  - `/api/admin/login` : authentification agent.
  - `/api/admin/applications` : liste et gestion des dossiers (GET/PUT).

### Base de données (schéma simplifié)

```text
Citizens
- id (UUID)
- prenom
- nom
- sexe
- date_naissance
- groupe_sanguin
- adresse
- photo_path
- piece_identite_path
- statut (enum)
- numero_permis (optionnel)

Agents
- id (UUID)
- email
- mot_de_passe_hash
- role (admin / agent)
```

### Classes / Objets principaux (pseudo-code)

```ts
class Citizen {
  id: string;
  prenom: string;
  nom: string;
  sexe: 'M' | 'F';
  dateNaissance: Date;
  groupeSanguin: string;
  adresse: string;
  photoPath: string;
  identitePath: string;
  statut: 'En attente' | 'En traitement' | 'Accepté' | 'Test pratique' | 'Permis délivré';
  numeroPermis?: string;
}

class Agent {
  id: string;
  email: string;
  motDePasseHash: string;
  role: 'admin' | 'agent';
}
```

### Routes API simulées (Express)

```ts
// Inscription
app.post('/api/register', (req, res) => { /* création du dossier */ });

// Vérification OTP
app.post('/api/verify-otp', (req, res) => { /* validation du code */ });

// Récupération du statut
app.get('/api/status/:id', (req, res) => { /* renvoie le statut du dossier */ });

// Connexion agent
app.post('/api/admin/login', (req, res) => { /* authentification */ });

// Gestion des dossiers
app.get('/api/admin/applications', (req, res) => { /* liste */ });
app.put('/api/admin/applications/:id', (req, res) => { /* mise à jour */ });
```

## 2. Proposition d'interface utilisateur (simplifiée)

```html
<!-- Page d'accueil -->
<header class="banner">
  <img src="/assets/flag-rdc.png" alt="Drapeau RDC" class="flag" />
  <h1>Gestion du Permis de Conduire - RDC</h1>
</header>
<main>
  <a href="/register" class="btn">S'inscrire</a>
  <a href="/agent" class="btn-secondary">Espace Agent</a>
</main>
```

```css
body {
  font-family: Arial, sans-serif;
  background-image: url('/assets/goma-background.jpg');
  background-size: cover;
  color: #003366;
}
.flag {
  width: 50px;
}
.btn {
  background-color: #1F64FF;
  color: white;
  padding: 12px 24px;
  border-radius: 4px;
  text-decoration: none;
}
```

## 3. Utilisation dans Framer

1. Importer le logo, le drapeau de la RDC et l'image de Goma dans la bibliothèque Framer.
2. Créer des frames correspondant aux différentes pages (Accueil, Inscription, Suivi, Espace Agent).
3. Utiliser des composants interactifs pour simuler l'envoi d'OTP, la mise à jour des statuts et l'affichage du QR Code.
4. Publier la maquette sur Framer avec le lien `https://permis-rdcongo.framer.website` pour la démonstration.

---

Cette structure permet de présenter un prototype fonctionnel sans connexion réelle à une base de données, tout en illustrant les principales étapes du processus d'obtention d'un permis biométrique sécurisé en RDC.

