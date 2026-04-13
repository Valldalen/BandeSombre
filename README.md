[README.md](https://github.com/user-attachments/files/26544700/README.md)
<div align="center">

# 🌑 Bande Sombre

**Journal d'humeur quotidien pour Android**

*Suivez, analysez et comprenez vos états émotionnels au fil du temps*

</div>

---

## Aperçu

Bande Sombre est une application Android minimaliste à thème sombre permettant de noter son humeur chaque jour sur une échelle de 1 à 10, d'y associer des tags contextuels et des descriptions libres, puis d'analyser ses données via des statistiques et des corrélations.

L'échelle chromatique va du rouge foncé (1 — Terrible) au vert foncé (10 — Excellent), en passant par le jaune-orangé pour les valeurs moyennes.

---

## Fonctionnalités

### 📝 Saisie d'humeur
- Échelle de **10 niveaux** avec emoji et label (Terrible → Excellent)
- **Tags positifs et négatifs** avec intensité : cochez un tag plusieurs fois pour indiquer qu'il a beaucoup aidé (`+2`) ou beaucoup pesé (`-2`)
- **Description libre** par entrée
- **Saisie rétrospective** : remplissez les jours passés sans restriction

### 📅 Vue calendrier
- Mois coloré selon l'humeur de chaque jour
- Navigation mois par mois
- Accès rapide à l'édition de chaque journée

### 📊 Statistiques & corrélations
- Périodes : 7 jours, 30 jours, trimestre, année, tout
- Graphique de tendance d'humeur
- Distribution des notes (1–10)
- Meilleur / pire jour de la semaine
- Tendance générale (en progression, stable, en baisse)
- **Corrélations tags → humeur** :
  - *Ce qui a aidé* (tags positifs) — classé par impact total
  - *Ce qui a pesé* (tags négatifs) — classé par impact total
  - L'impact total combine intensité et récurrence : trois occurrences de `-1` comptent plus qu'une occurrence de `-2`

### 🔍 Exploration & filtres
- Filtrage croisé par niveau d'humeur et par tags
- Vue liste de toutes les entrées
- Recherche dans l'historique

### 📤 Export
- **CSV** — compatible Google Sheets, encodé UTF-8 avec BOM
- **PDF** — rapport de synthèse thématique (6 pages) :
  - Page de garde avec KPI et résumé narratif
  - Évolution dans le temps (courbe quotidienne + barres mensuelles)
  - Comparaison des périodes (30j vs 30j précédents)
  - Patterns & rythmes (jours de la semaine, streak)
  - Corrélations tags (alignées avec les stats de l'app)
  - Synthèse avec points positifs / points de vigilance

### 🔔 Rappels
- Notification quotidienne à l'heure choisie
- Message personnalisable
- Replanification automatique après chaque déclenchement et après redémarrage
- Gestion correcte de la permission `POST_NOTIFICATIONS` (Android 13+)

### 🔒 Sécurité
- Verrouillage par **PIN**
- Verrouillage par **empreinte digitale** / reconnaissance faciale (biométrie forte)

### 🪟 Widget
- Widget écran d'accueil 2×2
- Affiche l'humeur de la journée

---

## Architecture

```
app/
├── data/
│   ├── db/               # Room — MoodDatabase, MoodEntryDao, entités, migrations
│   └── repository/       # MoodRepository, SettingsRepository, TagsRepository
├── di/                   # Modules Hilt (AppModule)
├── domain/
│   └── model/            # MoodEntry, MoodLevel, MoodStats, DEFAULT_TAGS
├── presentation/
│   ├── home/             # HomeScreen, AddEditMoodScreen, ViewModels
│   ├── calendar/         # CalendarScreen, CalendarViewModel
│   ├── stats/            # StatsScreen, StatsViewModel (corrélations)
│   ├── explore/          # ExploreScreen, ExploreViewModel
│   ├── settings/         # SettingsScreen, LockScreen, TagsManagerScreen
│   └── theme/            # BandeSombreTheme, couleurs, typographie
├── util/
│   └── ExportUtil.kt     # Export CSV et PDF (iText)
├── widget/               # MoodWidget (Glance)
└── worker/               # MoodReminderWorker, NotificationReceiver, BootReceiver
```

**Pattern** : MVVM + Clean Architecture  
**Navigation** : `NavHost` unique avec deep links entre les écrans  
**État** : `StateFlow` + `collectAsState()` dans chaque écran

---

## Stack technique

| Composant | Bibliothèque | Version |
|-----------|-------------|---------|
| Langage | Kotlin | 1.9.23 |
| UI | Jetpack Compose + Material 3 | BOM 2024.06 |
| Architecture | ViewModel + Hilt | Hilt 2.51.1 |
| Base de données | Room | 2.6.1 |
| Préférences | DataStore Preferences | — |
| Navigation | Navigation Compose | — |
| Notifications | AlarmManager (exact) | — |
| Widget | Glance AppWidget | 1.0.0 |
| Biométrie | BiometricManager | — |
| Export PDF | iText 5 | — |
| Coroutines | kotlinx.coroutines | — |

**Min SDK** : 31 (Android 12)  
**Target SDK** : 34 (Android 14)  
**Compile SDK** : 34

---

## Installation & développement

### Prérequis
- Android Studio Hedgehog (2023.1.1) ou plus récent
- JDK 17
- Android SDK 34


## Logique des corrélations

La corrélation d'un tag mesure son **impact total** sur la période sélectionnée :

```
impact_par_entrée = nb_occurrences_du_tag_ce_jour
totalImpact       = Σ(impact_par_entrée) sur toutes les entrées de la période
avgImpact         = totalImpact / nb_entrées
```

Exemples :
- Tag positif "Sport" noté `+1` trois jours → `totalImpact = +3`
- Tag négatif "Stress" noté `+2` un jour + `+1` un autre → `totalImpact = +3`
- Les deux ont le même poids total, mais "Sport" a plus de récurrence

Les tags sont triés par `totalImpact` décroissant. À impact égal, la récurrence (`count`) départage.


<div align="center">
<sub>Fait avec 🖤 — thème sombre uniquement</sub>
</div>







# BandeSombre ◈ Analyseur
 
> Analyseur local de journal d'humeur — propulsé par Ollama, zéro serveur, zéro cloud.
 
![Interface BandeSombre](https://img.shields.io/badge/version-1.0-c4703a?style=flat-square) ![HTML](https://img.shields.io/badge/stack-HTML%20%2F%20JS-e8e0d0?style=flat-square) ![Ollama](https://img.shields.io/badge/IA-Ollama%20local-6e9e6e?style=flat-square) ![Licence](https://img.shields.io/badge/licence-MIT-4a4538?style=flat-square)
 
---
 
## Présentation
 
**BandeSombre Analyseur** est une application web monopage (un seul fichier `.html`) pour analyser des données d'humeur exportées au format CSV. Elle tourne entièrement dans le navigateur et se connecte à une instance **Ollama locale** pour générer des résumés, détecter des patterns et dialoguer avec vos données — sans aucune donnée envoyée vers un serveur externe.
 
---
 
## Fonctionnalités
 
- **Import CSV** par glisser-déposer ou sélection de fichier
- **Filtre par plage de dates** avec raccourcis rapides (7j, 14j, 30j, tout)
- **Indicateur de tendance** comparé à la période précédente équivalente
- **Visualisations** : courbe temporelle, distribution des niveaux, tags fréquents, corrélations tag → humeur, radar émotionnel
- **Heatmap calendrier** — vue journalière type GitHub contributions
- **Comparaison de deux périodes** avec statistiques côte à côte
- **Analyse IA via Ollama** : résumé global, patterns & tendances, observations bienveillantes, corrélations détaillées
- **Dialogue contextuel** — chat libre avec vos données
- **Analyses sauvegardées** — chaque réponse IA est conservée, supprimable individuellement
- **Export PDF** — génère un rapport complet incluant graphiques, données brutes et analyses sauvegardées
 
---
 
## Format CSV attendu
 
```csv
Date,Niveau,Humeur,TagsPositifs,TagsNegatifs,Description
06/03/2026,4,Pas terrible,,Solitude|Stress,
07/03/2026,7,Bien,Loisirs|Sport,,Super journée dehors
```
 
| Colonne | Format | Exemple |
|---|---|---|
| `Date` | `DD/MM/YYYY` | `14/04/2026` |
| `Niveau` | Entier 1–10 | `6` |
| `Humeur` | Texte libre | `Moyen` |
| `TagsPositifs` | Tags séparés par `\|` | `Sport\|Loisirs` |
| `TagsNegatifs` | Tags séparés par `\|` | `Solitude\|Stress` |
| `Description` | Texte libre optionnel | `Bonne journée malgré...` |
 
---
 
## Prérequis
 
### 1. Installer Ollama
 
Téléchargez et installez Ollama depuis le site officiel :
 
**[https://ollama.com/download](https://ollama.com/download)**
 
Disponible sur macOS, Windows et Linux.
 
---
 
### 2. Télécharger le modèle recommandé — `gemma4`
 
Le modèle recommandé est **Gemma 4** de Google. Il offre un excellent équilibre entre qualité d'analyse et vitesse sur du matériel grand public.
 
```bash
ollama pull gemma4
```
 
> D'autres modèles compatibles : `llama3`, `mistral`, `phi3`, `qwen2`… Vous pouvez changer le modèle directement dans l'interface.
 
---
 
### 3. Lancer Ollama avec les origines CORS autorisées
 
> ⚠️ **Étape indispensable.** Sans cette commande, le navigateur bloquera les appels API vers Ollama.
 
#### Windows (PowerShell)
 
```powershell
$env:OLLAMA_ORIGINS="*"; ollama serve
```
 
#### macOS / Linux (Terminal)
 
```bash
OLLAMA_ORIGINS="*" ollama serve
```
 
Laissez ce terminal ouvert pendant toute la session d'utilisation.
 
---
 
## Utilisation
 
1. Lancez Ollama avec la commande ci-dessus
2. Ouvrez le fichier `bandesombre-analyseur.html` dans votre navigateur
3. Vérifiez la connexion via le bouton **Tester** dans la barre de configuration
4. Glissez votre fichier CSV dans la zone de dépôt
5. Explorez les visualisations, lancez des analyses IA, dialoguez avec vos données
6. Exportez en PDF via le bouton **Exporter PDF**
 
---
 
## Stack technique
 
- **HTML / CSS / JavaScript** pur — aucun framework, aucune dépendance npm
- **[Chart.js](https://www.chartjs.org/)** (CDN) — graphiques
- **[Ollama API](https://github.com/ollama/ollama/blob/main/docs/api.md)** — inférence locale en streaming
- **CSS `@media print`** — export PDF natif via le navigateur
 
---
 
## Vie privée
 
Toutes vos données restent sur votre machine. Aucune requête n'est envoyée vers un serveur externe — ni les données CSV, ni les réponses IA. Le seul trafic réseau est entre votre navigateur et `localhost:11434` (Ollama).
