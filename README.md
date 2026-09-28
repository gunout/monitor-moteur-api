# 📡 Monitor — Méta-moteur data.gouv.fr

> Dashboard de surveillance en temps réel du [méta-moteur data.gouv.fr](https://github.com/gunout/moteur-api).

[![Python](https://img.shields.io/badge/Python-3.12+-3776AB?style=flat-square&logo=python&logoColor=white)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.141-009688?style=flat-square&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![DSFR](https://img.shields.io/badge/DSFR-1.11-000091?style=flat-square)](https://www.systeme-de-design.gouv.fr/)
[![Licence](https://img.shields.io/badge/Licence-etalab--2.0-blue?style=flat-square)](https://www.etalab.gouv.fr/licence-ouverte-open-licence/)
[![Statut](https://img.shields.io/badge/Statut-actif-success?style=flat-square)]()
[![Dernier commit](https://img.shields.io/github/last-commit/gunout/monitor-moteur-api?style=flat-square&color=000091)](https://github.com/gunout/monitor-moteur-api/commits/main)
[![Issues](https://img.shields.io/github/issues/gunout/monitor-moteur-api?style=flat-square&color=E1000F)](https://github.com/gunout/monitor-moteur-api/issues)
[![Stars](https://img.shields.io/github/stars/gunout/monitor-moteur-api?style=flat-square&color=f1c40f)](https://github.com/gunout/monitor-moteur-api/stargazers)
[![Taille](https://img.shields.io/github/repo-size/gunout/monitor-moteur-api?style=flat-square&color=1212ff)](https://github.com/gunout/monitor-moteur-api)

---

## 📖 Présentation

**Monitor — Méta-moteur data.gouv.fr** est un dashboard de surveillance qui se connecte au [méta-moteur data.gouv.fr](https://github.com/gunout/moteur-api) et affiche en temps réel :

- L'**état du backend** (en ligne / hors ligne, latence, uptime)
- Les **performances** de chaque recherche (temps de réponse, nombre de résultats)
- La **santé des sources** interrogées (v1, v2, dataservices, tabulaire)
- Un **historique** des dernières recherches
- Un **panneau de contrôle** pour lancer des recherches sans quitter le monitor

L'interface suit la **charte Marianne** (DSFR) avec bandeau tricolore et bloc-marque officiel.

---

## 📸 Captures d'écran

### Dashboard principal

<img width="1644" height="829" alt="Screenshot 2026-09-28 at 15-46-32 🇫🇷 Meta Monitor — Méta-moteur data gouv fr" src="https://github.com/user-attachments/assets/97d94b3e-1aa3-4f99-a727-251945cde99a" />


*Vue d'ensemble avec recherche intégrée, statistiques en direct et historique d'activité.*

### Panneau d'intelligence

<img width="1644" height="829" alt="Screenshot 2026-09-28 at 15-50-50 🇫🇷 Meta Monitor — Méta-moteur data gouv fr" src="https://github.com/user-attachments/assets/b7a6760c-4357-4712-9282-543ce380688a" />


*État du backend, santé des sources et configuration en temps réel.*

---

## ✨ Fonctionnalités

- 🏓 **Ping automatique** du backend toutes les 30 secondes
- 📊 **Statistiques en direct** : latence, uptime, nombre de résultats
- 📡 **Santé des sources** : état des 4 API interrogées par le méta-moteur
- 🔍 **Recherche intégrée** : lancez des requêtes sans quitter le monitor
- 📥 **Export CSV / JSON** directement depuis le monitor
- 🕐 **Historique** : 10 dernières recherches avec heure et nombre de résultats
- 🏛️ **Liste des ministères** intégrés au méta-moteur
- 📄 **Vue JSON brut** des résultats pour le débogage
- ⏱️ **Vue timings** : temps de réponse détaillé
- 🎨 **Design Marianne** (DSFR) avec bandeau tricolore et bloc-marque
- 📱 **Responsive** : s'adapte mobile, tablette, desktop

---

## 🏗️ Architecture

```
monitor-moteur-api/
├── main.py              # Backend FastAPI (proxy de surveillance)
├── index.html           # Interface du monitor (design Marianne)
├── requirements.txt     # Dépendances Python
├── LICENSE              # Licence etalab-2.0
└── README.md
```

### Flux de données

```
┌──────────────┐
│   Monitor    │  (index.html · DSFR)
│   Navigateur │
└──────┬───────┘
       │
       │ 1. Ping régulier toutes les 30s
       │    GET /
       │
       │ 2. Recherches à la demande
       │    GET /search?q=...
       │
       ▼
┌──────────────┐
│ Méta-moteur  │  (port 8001)
│  Backend     │
└──────┬───────┘
       │
       │ Interroge en parallèle
       ├──────────────► API Catalogue v1
       ├──────────────► API Catalogue v2
       ├──────────────► API Dataservices
       └──────────────► API Organizations
       │
       ▼
┌──────────────┐
│  Résultats   │
│  fusionnés   │
└──────────────┘
```

---

## 🚀 Installation

### Prérequis

- **Python 3.12+**
- **pip** et **venv**
- Le **méta-moteur data.gouv.fr** doit tourner sur `http://localhost:8001`

### Étapes

```bash
# 1. Cloner le dépôt
git clone https://github.com/gunout/monitor-moteur-api.git
cd monitor-moteur-api

# 2. Créer et activer un environnement virtuel
python3 -m venv Mapi
source Mapi/bin/activate

# 3. Installer les dépendances
pip install -r requirements.txt

# 4. Lancer le serveur du monitor
uvicorn main:app --reload --port 8002
```

Le monitor démarre sur **`http://127.0.0.1:8002`**.

> ⚠️ **Important** : le monitor se connecte par défaut au méta-moteur sur `http://localhost:8001`. Assurez-vous que ce dernier tourne bien avant de lancer le monitor.

---

## 🖥️ Utilisation

### Interface web

Ouvrez dans votre navigateur :

```
http://127.0.0.1:8002/
```

Ou, si le montage `StaticFiles` n'est pas activé, ouvrez directement `index.html`.

### Configuration du backend cible

Si votre méta-moteur tourne sur un autre port, vous pouvez le changer dans la console du navigateur :

```javascript
localStorage.setItem('meta_api_base', 'http://localhost:8003');
location.reload();
```

Ou modifiez la constante dans `index.html` :

```javascript
const API_BASE = localStorage.getItem('meta_api_base') || 'http://localhost:8001';
```

---

## 🎛️ Panneaux du monitor

| Panneau | Contenu |
| :--- | :--- |
| **💚 État du backend** | Endpoint, statut, latence, dernier ping, uptime |
| **📊 Dernière recherche** | Requête, résultats, temps total, répartition datasets/APIs |
| **📡 Santé des sources** | État des 4 API (v1, v2, dataservices, tabulaire) |
| **🕐 Activité récente** | Historique des 10 dernières recherches |
| **⚙️ Configuration** | Backend cible, user agent, mode, cache |

---

## 🔍 Recherche intégrée

Le monitor permet de lancer des recherches sans quitter le dashboard :

1. Tapez un mot-clé dans la barre
2. Choisissez le type (`Tout`, `Datasets`, `APIs`)
3. Choisissez le tri (`Pertinence`, `Popularité`, `Récence`)
4. Cliquez sur **▶ LANCER**

Les résultats s'affichent dans la colonne centrale avec :
- Badges **API** ou **DATASET**
- Source (v1, v2, dataservices)
- Score de popularité (si disponible)
- Organisation productrice
- Tags

---

## 📊 Vues disponibles

| Onglet | Description |
| :--- | :--- |
| **📋 Résultats** | Affichage formaté des cartes de résultats |
| **📄 JSON brut** | JSON des 5 premiers résultats (utile pour déboguer) |
| **⏱️ Timings** | Temps de réponse et nombre de résultats de la dernière requête |

---

## 🛠️ Stack technique

| Composant | Technologie |
| :--- | :--- |
| **Backend** | Python 3.12, FastAPI |
| **Frontend** | HTML5, CSS3, JavaScript vanilla |
| **Design** | Système de Design de l'État (DSFR 1.11) |
| **Monitoring** | Fetch API + setInterval (ping 30s) |
| **Cache** | localStorage |

---

## 📁 Structure du projet

```
.
├── main.py              # Serveur FastAPI du monitor
├── index.html           # Interface de surveillance
├── requirements.txt     # Dépendances Python
├── LICENSE              # Licence etalab-2.0
├── README.md            # Ce fichier
└── Mapi/                # Environnement virtuel (non versionné)
```

---

## 🔗 Dépôt lié

Ce monitor est conçu pour surveiller le projet principal :

- **[gunout/moteur-api](https://github.com/gunout/moteur-api)** — Méta-moteur data.gouv.fr

---

## 🤝 Contribution

Les contributions sont bienvenues. Pour proposer une amélioration :

1. Forkez le projet
2. Créez une branche (`git checkout -b feature/amelioration`)
3. Committez vos changements (`git commit -m 'Ajout de…'`)
4. Poussez la branche (`git push origin feature/amelioration`)
5. Ouvrez une Pull Request

---

## 📜 Licence

Ce projet est distribué sous licence **etalab-2.0**, conformément à la politique d'ouverture des données publiques françaises.

Les données interrogées restent la propriété de leurs producteurs respectifs.

---

## 🔗 Ressources

- [data.gouv.fr](https://www.data.gouv.fr)
- [API data.gouv.fr — documentation](https://doc.data.gouv.fr/api/intro/)
- [Système de Design de l'État (DSFR)](https://www.systeme-de-design.gouv.fr/)
- [Licence etalab-2.0](https://www.etalab.gouv.fr/licence-ouverte-open-licence/)

---

<p align="center">
  <strong>République Française</strong><br>
  <em>Liberté · Égalité · Fraternité</em>
</p>

---

<div align="center">

### 🇫🇷 Gunout · 2026

![Made in France](https://img.shields.io/badge/Made_in-France-002395?style=flat-square&labelColor=FFFFFF&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA5MDAgNjAwIj48cmVjdCB3aWR0aD0iOTAwIiBoZWlnaHQ9IjYwMCIgZmlsbD0iIzAwMjM5NSIvPjxyZWN0IHdpZHRoPSI5MDAiIGhlaWdodD0iNDAwIiB5PSIxMDAiIGZpbGw9IiNmZmYiLz48cmVjdCB3aWR0aD0iOTAwIiBoZWlnaHQ9IjIwMCIgeT0iNDAwIiBmaWxsPSIjZWQyOTM5Ii8+PC9zdmc+)
![GitHub](https://img.shields.io/badge/GitHub-gunout-181717?style=flat-square&logo=github&logoColor=white)
![Year](https://img.shields.io/badge/2026-ED2939?style=flat-square&labelColor=FFFFFF)

<sub>© 2026 <strong>Gunout</strong> — Tous droits réservés.</sub>

</div>
