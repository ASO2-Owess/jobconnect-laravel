**Étape 10 : README + push GitHub**

1. Crée le fichier `README.md` à la racine du projet :

```markdown
# JobConnect — API Demo (Job Completion Flow)

Démo technique illustrant l'implémentation d'un endpoint de finalisation de job sur une plateforme de mise en relation emploi-candidature, construite avec **Laravel 12**.

## Stack

- Laravel 12
- Laravel Sanctum (authentification API par token)
- MySQL

## Fonctionnalité démontrée

Endpoint permettant à un client de marquer un `JobPost` comme complété, avec :

- **Authentification** via Sanctum (Bearer Token)
- **Autorisation** via une Policy dédiée (`JobPostPolicy`) — seul le propriétaire du job peut le compléter, et uniquement si son statut est `in_progress`
- **Validation** via une Form Request (`CompleteJobRequest`)
- **Réponse formatée** via une API Resource (`JobPostResource`)

## Endpoint

```
PATCH /api/job-posts/{jobPost}/complete
Authorization: Bearer {token}
Content-Type: application/json

{
    "notes": "Travail terminé avec succès"
}
```

### Réponses

| Cas | Code |
|---|---|
| Succès (propriétaire, job `in_progress`) | `200 OK` |
| Job déjà complété / statut invalide | `403 Forbidden` |
| Utilisateur non propriétaire du job | `403 Forbidden` |
| Validation échouée | `422 Unprocessable Entity` |

## Structure du code

```
app/Http/Controllers/Api/JobPostCompletionController.php
app/Http/Requests/CompleteJobRequest.php
app/Http/Resources/JobPostResource.php
app/Policies/JobPostPolicy.php
app/Models/JobPost.php
app/Models/JobApplication.php
```

## Installation locale

```bash
composer install
cp .env.example .env
php artisan key:generate
php artisan migrate
php artisan serve
```
```

2. Ajoute un `.gitignore` correct (Laravel en génère un par défaut, vérifie juste qu'il exclut bien `.env` et `/vendor`) :

```powershell
type .gitignore
```

3. Commit et push :

```powershell
git add .
git commit -m "Add job completion API endpoint with Sanctum, Policy, Form Request and Resource"
git push origin main
```
