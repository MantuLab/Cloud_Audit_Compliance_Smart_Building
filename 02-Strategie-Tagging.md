# Stratégie de Tagging - Analyse et Recommandations

## Résumé Exécutif

L'analyse de la stratégie de tagging actuelle révèle une approche incohérente et incomplète. Seulement quelques ressources sont taguées, avec des conventions variables et des informations manquantes critiques pour la gouvernance, la gestion des coûts et la sécurité.

## 1. Analyse de l'Existant

### 1.1 Valeurs de Tags Observées

#### Tag "team"
- `infra` (Infrastructure)
- `datascience` (Data Science)
- `infra/datascience` (Mixte - problématique)

#### Tag "branch"
- `iot` (IoT)
- `data` (Données)
- `security` (Sécurité)
- `alerts` (Alertes)
- `testdata` (Test)
- `iot/data` (Mixte - problématique)

### 1.2 Problèmes Identifiés

1. **Absence de Tags Critiques**
   - ❌ Pas de tag d'environnement (dev/test/prod)
   - ❌ Pas de tag de cost center
   - ❌ Pas de tag de propriétaire/contact
   - ❌ Pas de tag de projet
   - ❌ Pas de tag de criticité
   - ❌ Pas de tag de date de création/expiration

2. **Incohérence des Conventions**
   - Utilisation de "/" dans les valeurs (problématique pour filtrage)
   - Espaces dans les clés ("real time" vs "realtime")
   - Casse variable (pas de standard)

### 1.3 Impacts de l'Absence de Tags

1. **Gestion des coûts** : Impossible d'attribuer les coûts par projet, environnement ou propriétaire
2. **Sécurité** : Difficile d'identifier les ressources critiques ou sensibles
3. **Opérations** : Complexité accrue pour la maintenance et le dépannage
4. **Conformité** : Non-respect des bonnes pratiques de gouvernance Azure
5. **Automatisation** : Impossible d'utiliser les tags pour l'automatisation et les politiques

## 2. Catégories de tags
Il est important de choisir une convention de nommage et de s'y conformer. Les types de conventions classiques possibles:
- camelCase
- snake_case
- PascalCase
- kebab-case <- convention selectionnée pour la suite.
- UPPER_CASE

### Tags Obligatoires (Niveau 1 - Critique)

Ces tags sont **REQUIS** sur toutes les ressources sans exception.

| Tag | Description | Valeurs autorisées | Exemple |
|-----|-------------|-------------------|---------|
| `environment` | Environnement de déploiement | `prod`, `test`, `dev`, `staging` | `environment:prod` |
| `project` | Nom du projet | `smart-building` | `project:smart-building` |
| `team` | Équipe propriétaire | `infra`, `datascience`, `security`, `devops` | `team:infra` |
| `owner` | Responsable de la ressource | Format email valide | `owner:admin@mantu.com` |
| `cost-center` | Centre de coût | Format CC-XXXX | `cost-center:CC-1234` |
| `created-date` | Date de création | Format YYYY-MM-DD | `created-date:2025-06-24` |

### Tags Standards (Niveau 2 - Recommandé)

Tags fortement recommandés pour une gouvernance optimale.

| Tag | Description | Valeurs autorisées | Exemple |
|-----|-------------|-------------------|---------|
| `branch` | Branche fonctionnelle | `iot`, `data`, `security`, `alerts`, `ml`, `storage` | `branch:iot` |
| `data-classification` | Classification des données | `public`, `internal`, `confidential`, `restricted` | `data-classification:internal` |
| `backup-required` | Sauvegarde nécessaire | `true`, `false` | `backup-required:true` |
| `monitoring-level` | Niveau de monitoring | `basic`, `standard`, `premium` | `monitoring-level:standard` |
| `availability-requirement` | Exigence de disponibilité | `low`, `medium`, `high`, `critical` | `availability-requirement:high` |
| `compliance` | Exigences de conformité | `gdpr`, `iso27001`, `none` | `compliance:gdpr` |

### Tags Opérationnels (Niveau 3 - Optionnel)

Tags pour l'automatisation et les opérations.

| Tag | Description | Valeurs autorisées | Exemple |
|-----|-------------|-------------------|---------|
| `expiry-date` | Date d'expiration (dev/test) | Format YYYY-MM-DD | `expiry-date:2025-12-31` |
| `auto-shutdown` | Arrêt automatique autorisé | `true`, `false` | `auto-shutdown:true` |
| `patch-group` | Groupe de patching | `group-a`, `group-b`, `group-c` | `patch-group:group-a` |
| `business-unit` | Unité métier | `smart-cities`, `iot-solutions` | `business-unit:smart-cities` |
| `application` | Application spécifique | Nom de l'application | `application:sensor-processing` |
| `version` | Version de l'application | Format semver | `version:1.2.3` |

### Tags Spécifiques IoT (Si possible)
Tags pour l'indentification des sondes.

| Nom du Tag | Description | Valeurs Possibles | Exemple |
|------------|-------------|-------------------|---------|
| `device-type` | Type de capteur/device | `temperature`, `humidity`, `sound`, `presence`, `consumption` | `temperature` |
| `building` | Bâtiment concerné | [Nom/ID du bâtiment] | `building-a` |
| `floor` | Étage | [Numéro d'étage] | `floor-3` |
| `zone` | Zone du bâtiment | [Nom de la zone] | `openspace-north` |

## 3. Schéma par type d'environnement

### Environnement Production

```json
{
  "mandatory_tags": {
    "environment": "prod",
    "project": "smart-building",
    "team": "infra|datascience|security",
    "owner": "email@mantu.com",
    "cost-center": "CC-XXXX",
    "created-date": "YYYY-MM-DD"
  },
  "recommended_tags": {
    "branch": "iot|data|security|alerts",
    "data-classification": "internal|confidential",
    "backup-required": "true",
    "monitoring-level": "standard|premium",
    "availability-requirement": "high|critical",
    "compliance": "gdpr|iso27001"
  }
}
```

### Environnement Test

```json
{
  "mandatory_tags": {
    "environment": "test",
    "project": "smart-building",
    "team": "infra|datascience|security",
    "owner": "email@mantu.com",
    "cost-center": "CC-XXXX",
    "created-date": "YYYY-MM-DD"
  },
  "recommended_tags": {
    "branch": "iot|data|security|alerts",
    "expiry-date": "YYYY-MM-DD",
    "auto-shutdown": "true",
    "backup-required": "false"
  }
}
```

### Environnement Développement

```json
{
  "mandatory_tags": {
    "environment": "dev",
    "project": "smart-building",
    "team": "datascience|infra",
    "owner": "email@mantu.com",
    "cost-center": "CC-XXXX",
    "created-date": "YYYY-MM-DD"
  },
  "recommended_tags": {
    "expiry-date": "YYYY-MM-DD",
    "auto-shutdown": "true",
    "backup-required": "false",
    "monitoring-level": "basic"
  }
}
```

## 4. Schéma par type de service Azure

### Services IoT

```yaml
IoT Hub:
  mandatory: [environment, project, team, owner, cost-center, created-date]
  recommended: [branch:iot, data-classification, monitoring-level:premium]
  
Event Hub:
  mandatory: [environment, project, team, owner, cost-center, created-date]
  recommended: [branch:iot, monitoring-level:standard, backup-required:false]

Stream Analytics:
  mandatory: [environment, project, team, owner, cost-center, created-date]
  recommended: [branch:data, application, version]
```

### Services de données

```yaml
Cosmos DB:
  mandatory: [environment, project, team, owner, cost-center, created-date]
  recommended: [branch:data, data-classification:confidential, backup-required:true, compliance:gdpr]

Storage Account:
  mandatory: [environment, project, team, owner, cost-center, created-date]
  recommended: [branch, data-classification, backup-required, compliance]

Machine Learning:
  mandatory: [environment, project, team:datascience, owner, cost-center, created-date]
  recommended: [branch:ml, data-classification:internal, monitoring-level:standard]
```

### Services de calcul

```yaml
App Service Plan:
  mandatory: [environment, project, team, owner, cost-center, created-date]
  recommended: [auto-shutdown, availability-requirement]

Function App:
  mandatory: [environment, project, team, owner, cost-center, created-date]
  recommended: [application, version, branch, monitoring-level]

Container Registry:
  mandatory: [environment, project, team, owner, cost-center, created-date]
  recommended: [branch:data, data-classification:internal]
```

### Services réseau et sécurité

```yaml
Virtual Network:
  mandatory: [environment, project, team:infra, owner, cost-center, created-date]
  recommended: [branch:security, compliance]

Key Vault:
  mandatory: [environment, project, team, owner, cost-center, created-date]
  recommended: [branch:security, data-classification:restricted, compliance:gdpr]

Network Security Group:
  mandatory: [environment, project, team:infra, owner, cost-center, created-date]
  recommended: [branch:security]
```

## 4. Azure Policies pour l'application

Les Azures Policies ne peuvent être mise en place que si des corrections ont été apportées sur les ressources déjà existantes.

### Policy 1: Tags obligatoires (exemple)

```json
{
  "policyRule": {
    "if": {
      "allOf": [
        {
          "field": "type",
          "notIn": [
            "Microsoft.Resources/resourceGroups",
            "Microsoft.Resources/subscriptions"
          ]
        },
        {
          "anyOf": [
            {
              "field": "tags['environment']",
              "exists": "false"
            },
            {
              "field": "tags['project']",
              "exists": "false"
            },
            {
              "field": "tags['team']",
              "exists": "false"
            },
            {
              "field": "tags['owner']",
              "exists": "false"
            }
          ]
        }
      ]
    },
    "then": {
      "effect": "deny"
    }
  }
}
```

### Policy 2: Validation des valeurs (exemple)

```json
{
  "policyRule": {
    "if": {
      "allOf": [
        {
          "field": "tags['environment']",
          "exists": "true"
        },
        {
          "field": "tags['environment']",
          "notIn": ["prod", "test", "dev", "staging"]
        }
      ]
    },
    "then": {
      "effect": "deny"
    }
  }
}
```
#### Initiatives de Policy
1. **Deny-Without-Mandatory-Tags**: Bloquer création sans tags obligatoires
2. **Inherit-RG-Tags**: Hériter automatiquement certains tags
3. **Audit-Tag-Compliance**: Audit mensuel de conformité

---

*Fin du rapport d'analyse de la stratégie de tagging*