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
