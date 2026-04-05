# CLAUDE.md — EV Clinic Paris

## Projet

**EV Clinic Paris** — Site vitrine et écosystème applicatif pour un atelier spécialisé dans la réparation de véhicules électriques (batteries HV, moteurs, onduleurs, convertisseurs DC/DC).

- **Société mère :** EV Clinic (Croatie, fondée par Vanja Katic)
- **Entité légale Berlin :** AddCycle GmbH
- **Branche française :** EV Clinic Paris
- **Adresse :** 3-7 rue Georges Besse, Bâtiment Lavoisier, 92160 Antony
- **Email :** contact@evclinic.fr
- **Téléphone :** en cours
- **Horaires :** Lun-Ven 9h-17h, sur rendez-vous uniquement

---

## Architecture globale (3 repos séparés)

```
┌─────────────────┐   ┌──────────────────┐   ┌─────────────────┐
│  evclinic-web   │   │  evclinic-api    │   │  evclinic-back  │
│  Astro+Tailwind │   │  FastAPI+PG      │   │  React+TS+TW    │
│  Site vitrine   │   │  API métier      │   │  Back-office    │
└────────┬────────┘   └───┬──────────┬───┘   └────────┬────────┘
         │                │          │                 │
         └────────────────┤          ├─────────────────┘
                     API calls    API calls
                          │          │
                     ┌────┴──────────┴────┐
                     │    PostgreSQL      │
                     │    (Railway)       │
                     └─────────┬─────────┘
                               │
                     ┌─────────┴─────────┐
                     │   Pennylane API   │
                     │   (Facturation)   │
                     └───────────────────┘
```

### Repo 1 : `evclinic-web` (ce repo)
- **Stack :** Astro + Tailwind CSS
- **Rôle :** Site vitrine public FR, SEO-first, pages statiques
- **Module futur :** Formulaire de prise de RDV (island React) connecté à l'API métier
- **Hébergement :** Vercel / Netlify / Railway (statique)

### Repo 2 : `evclinic-api` (à venir)
- **Stack :** Python, FastAPI, PostgreSQL, Alembic
- **Rôle :** API métier — cœur du système
- **Architecture :** Hexagonale (Ports & Adapters)
- **Hébergement :** Railway

### Repo 3 : `evclinic-back` (à venir)
- **Stack :** React, TypeScript, Tailwind CSS, Tanstack Query, Vitest
- **Rôle :** Back-office atelier (app interne sécurisée)
- **Auth :** Email/password + MFA
- **Hébergement :** Railway

---

## Pratiques de développement — OBLIGATOIRE

### Software Craftsmanship

- **TDD obligatoire** : Red → Green → Refactor sur toute logique métier
- **Couverture de tests ≥ 80%** (fail CI si en-dessous)
- **Principes SOLID** appliqués systématiquement :
  - **S** — Single Responsibility : une classe = une responsabilité
  - **O** — Open/Closed : extension par ajout, pas par modification
  - **L** — Liskov Substitution : les implémentations respectent le contrat de l'interface
  - **I** — Interface Segregation : interfaces fines et spécifiques
  - **D** — Dependency Inversion : le domain dépend d'abstractions (ports), jamais d'implémentations
- **Clean Code** : nommage explicite, fonctions courtes, pas de code mort
- **DRY** mais pas au détriment de la lisibilité (3 lignes dupliquées > abstraction prématurée)
- **YAGNI** : pas de feature spéculative, pas de sur-ingénierie

### Architecture Hexagonale (API métier)

```
domain/          ← Cœur métier pur, ZÉRO dépendance externe
  models/        ← Entités, Value Objects, Aggregates
  services/      ← Logique métier
  ports/         ← Interfaces ABC (contrats)
  exceptions.py

application/     ← Use Cases, CQRS Commands/Queries
infrastructure/  ← Adapters concrets (DB, API externes, auth)
tests/
  unit/          ← Domain (rapide, mocké)
  integration/   ← Adapters (Testcontainers + PostgreSQL réel)
  e2e/           ← Routes API complètes
```

### Outils qualité

| Outil | Rôle |
|-------|------|
| **Ruff** | Linter + formatter Python |
| **mypy** | Typage statique strict |
| **pytest + pytest-cov** | Tests + couverture |
| **Testcontainers** | Tests d'intégration avec vraie DB |
| **pre-commit hooks** | Ruff + mypy + tests avant chaque commit |
| **ESLint + Prettier** | Linter/formatter front (React/TS) |
| **Vitest + React Testing Library** | Tests front |

### CI/CD (GitHub Actions)

Pipeline pour chaque repo :
1. Lint (Ruff / ESLint)
2. Type check (mypy / TypeScript)
3. Tests unitaires
4. Tests d'intégration
5. Coverage check (≥ 80%, fail sinon)
6. Build
7. Deploy (Railway)

---

## Workflow métier — Parcours client EV Clinic

```
RDV en ligne → Réception véhicule → Diagnostic → Ordre de Réparation (OR)
    → Suivi intervention → Facturation (Pennylane, NF525) → Restitution
    → Suivi post-intervention
```

### Rôles RBAC
- **Admin** : accès complet
- **Réceptionnaire** : réception, création OR, facturation
- **Technicien** : diagnostic, suivi intervention, mise à jour statut

### Sécurité
- Auth : JWT + MFA (back-office)
- Chiffrement AES-256 des données sensibles en base
- Audit log de chaque action sur un OR
- RGPD : politique de rétention des données, droit à l'oubli
- Backups chiffrés quotidiens
- Séparation environnements : dev / staging / prod
- HTTPS enforced

---

## Services proposés (contenu site)

### Réparation batteries HV
- Réparation et reconditionnement de batteries haute tension
- Jusqu'à 70% moins cher qu'un remplacement complet
- Délai : 1 à 3 jours ouvrés
- Garantie : 1 an (standard) ou 2 ans sans limite de kilométrage (premium à 5 000€)

### Réparation moteurs électriques
- Réparation et reconditionnement de moteurs
- Jusqu'à 60% moins cher qu'un remplacement
- Small Drive Unit : 2 000€, 4-5 jours ouvrés
- Large Drive Unit : 4 200€, 5-8 jours ouvrés
- Entretien recommandé tous les 50 000 à 100 000 km

### Onduleurs, convertisseurs DC/DC, unités de contrôle
- Réparation d'onduleurs (conversion DC→AC)
- Réparation de convertisseurs DC/DC (haute tension → basse tension)
- Diagnostic et réparation des ECU

### Marques supportées
Tesla (Model S, 3, X, Y), BMW (i3, iX), Renault ZOE, VW, Audi, Hyundai (Ioniq 5), et plus.

### Services véhicules spécifiques
- Renault Zoe : roulement moteur, moteur échange standard
- Tesla : réparation moteur LDU
- Véhicules E-Hybrid

---

## Phases du projet

| Phase | Scope | Repo | Statut |
|-------|-------|------|--------|
| **1** | Site vitrine FR (Astro + Tailwind) | evclinic-web | 🔄 En cours |
| **2** | API métier — modèles + RDV | evclinic-api | ⏳ À venir |
| **3** | Back-office — réception + diagnostic + OR | evclinic-back | ⏳ À venir |
| **4** | Module réservation site (island React → API) | evclinic-web + api | ⏳ À venir |
| **5** | Intégration facturation Pennylane | evclinic-api | ⏳ À venir |
| **6** | Sécurité avancée, RGPD, audit logs | Transverse | ⏳ À venir |

---

## Conventions

- **Langue du code** : anglais (variables, fonctions, classes)
- **Langue du contenu** : français (textes du site, commentaires métier)
- **Commits** : messages concis en anglais, format conventionnel
- **Branches** : `feature/`, `fix/`, `chore/`
- **PR** : description en français, review obligatoire
