# JobConnect — Job Completion API (Laravel 12 + Sanctum)

Démo technique d'un endpoint RESTful sécurisé, conçue pour illustrer une implémentation propre du flow de finalisation de mission sur une plateforme de mise en relation emploi-candidature (type marketplace freelance).

## 🎯 Objectif du projet

Ce dépôt reproduit un cas d'usage réel : permettre à un client de marquer une mission (`JobPost`) comme complétée, en respectant les conventions Laravel modernes (API Resources, Form Requests, Policies) et une authentification par token via Sanctum.

## 🛠️ Stack technique

- **Laravel 12**
- **Laravel Sanctum** — authentification API par token
- **MySQL**
- PHP 8.2

## ✅ Fonctionnalités démontrées

- Authentification API par **Bearer Token** (Sanctum)
- **Autorisation fine** via Policy — seul le client propriétaire du job peut le compléter, uniquement si son statut est `in_progress`
- **Validation** des données entrantes via Form Request dédiée
- **Formatage de réponse** standardisé via API Resource (pas de retour de model brut)
- Gestion d'erreurs HTTP cohérente (`403`, `422`)

## 📡 Endpoint

```http
PATCH /api/job-posts/{jobPost}/complete
Authorization: Bearer {token}
Accept: application/json
Content-Type: application/json

{
    "notes": "Travail terminé avec succès"
}
```

### Réponses possibles

| Scénario | Code HTTP |
|---|---|
| Client propriétaire, job `in_progress` | `200 OK` |
| Job déjà `completed` (ou autre statut) | `403 Forbidden` |
| Utilisateur non propriétaire du job | `403 Forbidden` |
| Champ `notes` invalide | `422 Unprocessable Entity` |
| Token manquant ou invalide | `401 Unauthorized` |

### Exemple de réponse (`200 OK`)

```json
{
  "data": {
    "id": 1,
    "title": "Test Job",
    "description": "Job de test",
    "status": "completed",
    "completed_at": "2026-07-12T11:54:34+00:00",
    "client": {
      "id": 1,
      "name": "Client Test"
    },
    "created_at": "2026-07-12T11:43:46+00:00",
    "updated_at": "2026-07-12T11:54:34+00:00"
  }
}
```

## 📁 Structure du code

```
app/
├── Http/
│   ├── Controllers/Api/JobPostCompletionController.php
│   ├── Requests/CompleteJobRequest.php
│   └── Resources/JobPostResource.php
├── Models/
│   ├── JobPost.php
│   └── JobApplication.php
└── Policies/
    └── JobPostPolicy.php
```

## 🚀 Installation locale

```bash
composer install
cp .env.example .env
php artisan key:generate
php artisan migrate
php artisan serve
```

## 🧪 Tests manuels effectués

- ✅ Complétion réussie par le propriétaire du job
- ✅ Rejet (`403`) si l'utilisateur n'est pas le propriétaire
- ✅ Rejet (`403`) si le job n'est pas en statut `in_progress`

## 👤 Auteur

**Owess Akpa** — Développeur Full-Stack (Laravel, Flutter, React)
[GitHub](https://github.com/ASO2-Owess)

git commit -m "Fix README markdown formatting"
git push origin main
```
