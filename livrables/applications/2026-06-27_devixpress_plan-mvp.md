# Plan — DevisExpress MVP

## Contexte

Bilal dirige Les Rois du Web, une agence avec ~900 clients artisans. Beaucoup de ces artisans font des devis peu professionnels ou trop longs. DevisExpress leur permet de créer un devis professionnel complet en moins d'une minute, par la voix ou via un formulaire simple. L'app sera d'abord utilisée en interne / proposée aux clients de l'agence.

---

## Ce qu'on construit

**DevisExpress** — App mobile React Native (iOS + Android via Expo)

Un artisan ouvre l'app, appuie sur un bouton, parle, et son devis est prêt en PDF en moins d'une minute.

---

## Stack technique

| Couche | Techno |
|---|---|
| Mobile | React Native + Expo |
| Auth + DB + Storage | Supabase |
| Voix → Texte | OpenAI Whisper API |
| Texte → Données structurées | Claude API (claude-sonnet-4-6) |
| PDF | react-native-html-to-pdf |
| Design | Claude Design (prompt fourni par Jarvis) |

---

## Fonctionnalités MVP

### 1. Auth
- Inscription email/mot de passe (Supabase Auth)
- Connexion
- Réinitialisation mot de passe

### 2. Profil artisan
- Nom de l'entreprise, prénom/nom
- SIRET, adresse, téléphone, email
- Logo (upload vers Supabase Storage)
- Ces infos apparaissent automatiquement sur chaque devis

### 3. Création de devis (le cœur)

**Mode Voix (différenciation principale)**
1. Bouton micro central — l'artisan appuie et parle
2. Exemple de dictée : "Devis pour M. Dupont au 12 rue de la Paix à Lyon, réfection de toiture, 3 jours de travail à 800 euros par jour, fournitures 1 500 euros"
3. Whisper transcrit → Claude extrait les données structurées
4. Le formulaire se remplit automatiquement
5. L'artisan vérifie, ajuste si besoin, valide

**Mode Formulaire**
- Formulaire étape par étape (stepper)
- Étape 1 : Infos client (nom, adresse, téléphone)
- Étape 2 : Lignes de prestation (description, qté, prix unitaire)
- Étape 3 : Récapitulatif + TVA (option) + remise (option)

### 4. Aperçu + Export
- Prévisualisation du devis mis en page (logo artisan, numéro auto, date, infos des deux parties, lignes, total HT/TVA/TTC)
- Export PDF
- Partage direct (WhatsApp, email, AirDrop)

### 5. Historique des devis
- Liste paginée (date, client, montant, statut : brouillon / envoyé / accepté / refusé)
- Filtres basiques
- Duplication d'un devis existant

---

## Schéma base de données (Supabase)

```
profiles          — infos artisan (1 par user)
  id, user_id, company_name, siret, address, phone, logo_url

clients           — carnet d'adresses de l'artisan
  id, artisan_id, name, address, phone, email

quotes            — en-tête du devis
  id, artisan_id, client_id, number, date, status,
  subtotal, tax_rate, tax_amount, total, notes

quote_lines       — lignes du devis
  id, quote_id, description, quantity, unit_price, total
```

---

## Écrans à designer (prompt Claude Design)

1. **Splash + Onboarding** (3 slides valeur)
2. **Auth** : Login / Signup
3. **Setup profil** : formulaire premier lancement
4. **Dashboard** : liste des devis + bouton "Nouveau devis" central
5. **Nouveau devis** : choix mode (Voix / Formulaire)
6. **Écran voix** : animation micro, transcription en temps réel
7. **Formulaire** : stepper 3 étapes
8. **Aperçu devis** : rendu PDF-like scrollable
9. **Profil** : édition infos artisan

---

## Ordre de développement

### Phase 1 — Fondations (Semaine 1)
- Setup Expo + Supabase
- Auth complet (signup, login, reset)
- Setup profil artisan
- Navigation principale

### Phase 2 — Cœur (Semaine 2)
- Création de devis formulaire
- Numérotation auto des devis
- Génération et export PDF
- Partage

### Phase 3 — Voix (Semaine 3)
- Intégration Whisper (transcription)
- Intégration Claude (parsing structuré)
- Auto-remplissage du formulaire depuis la voix

### Phase 4 — Finitions MVP (Semaine 4)
- Historique + filtres
- Duplication de devis
- Design final appliqué (depuis Claude Design)
- Tests sur iOS + Android

---

## Prompt Claude Design (à utiliser)

À lancer dans Claude Design pour générer tous les écrans :

> Crée le design complet d'une application mobile native (iOS/Android) appelée **DevisExpress**. C'est une app pour artisans (couvreurs, plombiers, élagueurs, etc.) qui leur permet de créer un devis professionnel en moins d'une minute par la voix ou via un formulaire.
>
> **Direction artistique :**
> - Moderne, simple, ultra-lisible. L'utilisateur type n'est pas tech-savvy.
> - Couleur principale : bleu foncé professionnel (#1E3A5F) avec accent or/ambre (#F59E0B)
> - Fond blanc ou gris très clair, pas de dark mode pour le MVP
> - Typo : Inter ou SF Pro. Boutons grands et faciles à taper
> - Style "application pro de gestion" — référence : Facture Simple, Invoice Ninja, mais bien plus épuré
>
> **Écrans à générer (mobile 375x812) :**
> 1. Splash screen + logo DevisExpress
> 2. Onboarding 3 slides (bénéfices : rapide / professionnel / partageable)
> 3. Login + Signup
> 4. Setup profil (premier lancement) — formulaire avec upload logo
> 5. Dashboard — liste des devis avec FAB "+" central pour nouveau devis
> 6. Choix du mode — 2 grandes cartes : "🎤 Par la voix" et "✏️ Manuellement"
> 7. Écran voix — grand bouton micro animé au centre, zone de transcription en temps réel
> 8. Formulaire devis — stepper 3 étapes (client / prestations / récap)
> 9. Aperçu devis — rendu du devis mis en page, prêt à exporter
> 10. Profil artisan — édition des infos entreprise
>
> Génère des composants cohérents, réutilisables, avec des vraies données d'exemple (pas de lorem ipsum — utilise des noms d'artisans français, des vraies prestations de toiture/plomberie).

---

## Vérification MVP

- [ ] Un artisan peut s'inscrire, remplir son profil, créer un devis par la voix en moins de 60 secondes et envoyer le PDF par WhatsApp
- [ ] Les devis sont sauvegardés et consultables dans l'historique
- [ ] L'app fonctionne sur iOS et Android (testée via Expo Go)
