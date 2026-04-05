# MEMORY.md — Journal des évolutions EV Clinic Paris

> **Règle obligatoire :** Toute évolution, amélioration, décision technique ou changement d'architecture doit être consigné ici. Ce fichier est la source de vérité sur l'historique du projet.

---

## Format des entrées

```
### [YYYY-MM-DD] — Titre court
**Type :** Feature | Amélioration | Fix | Architecture | Décision | Refactor
**Scope :** evclinic-web | evclinic-api | evclinic-back | Transverse
**Description :** Ce qui a été fait et pourquoi.
**Impact :** Ce que ça change concrètement.
```

---

## Historique

### [2026-04-05] — Initialisation du projet EV Clinic Paris
**Type :** Architecture
**Scope :** Transverse
**Description :** Définition de l'architecture globale du projet :
- 3 repos séparés : `evclinic-web` (Astro), `evclinic-api` (FastAPI), `evclinic-back` (React+TS)
- Hébergement Railway, facturation Pennylane (NF525)
- Pratiques craft : TDD, SOLID, couverture ≥ 80%, architecture hexagonale
- CI/CD GitHub Actions
- Auth email/password + MFA pour le back-office
- Sécurité : JWT, RBAC, AES-256, audit logs, RGPD
**Impact :** Cadre technique et méthodologique posé pour l'ensemble du projet.

### [2026-04-05] — Enrichissement contenu avec données franchise
**Type :** Amélioration
**Scope :** evclinic-web
**Description :** Réécriture complète du contenu de toutes les pages. Récupération du contenu des sites franchise (evclinic.eu, evclinic-berlin.de) et intégration des éléments de confiance :
- À propos : Vanja Katic ex-Rimac Automobili (9 ans, lead BMS), équipe TU Berlin, partenariat SKF, outils diagnostiques propriétaires (EVC Explorer), philosophie "NO-MATTER-WHAT"
- Batteries : FAQ (5 questions), engagement réparation-ou-gratuit, spectroscopie d'impédance
- Moteurs : témoignage Gemmingen (1.8M km Tesla, 450K km après réparation EVC), explication technique joint amélioré
- Toutes pages : ton simple et rassurant, orienté problème/solution, parlant aux particuliers ET professionnels
**Impact :** Contenu crédible et engageant, basé sur les données réelles du réseau EV Clinic.

### [2026-04-05] — Fix routing + déploiement Railway
**Type :** Fix
**Scope :** evclinic-web
**Description :** Correction du flag `serve -s` (mode SPA) qui renvoyait index.html pour toutes les URLs. Passage de Nixpacks à Dockerfile (node:22-alpine) pour compatibilité Astro v6.
**Impact :** Toutes les pages affichent maintenant leur contenu propre. Déploiement Railway fonctionnel.

### [2026-04-05] — Phase 1 : Création du site vitrine FR
**Type :** Feature
**Scope :** evclinic-web
**Description :** Initialisation du site vitrine EV Clinic Paris avec Astro + Tailwind CSS.
Adaptation du design fourni (template HTML Berlin) pour le marché français.
Pages prévues : Accueil, Services, Batteries, Moteurs, Onduleurs, À propos, Contact, Réservation (placeholder), Mentions légales.
Contenu traduit et adapté : adresse Antony, email contact@evclinic.fr, tarifs identiques Berlin.
**Impact :** Première présence web de EV Clinic Paris.

---

<!-- Les nouvelles entrées s'ajoutent ici, en haut de la section Historique -->
