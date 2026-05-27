# 📊 Sales Dashboard — Power BI

> Atelier de bout en bout : de la donnée brute au rapport interactif multi-onglets avec modèle en étoile, mesures DAX, filtres, tooltips et sécurité.

---

## 📁 Données source

| Fichier | Description |
|---|---|
| `sales_2.csv` | Données de ventes transactionnelles — 13 colonnes brutes |

---

## ⚙️ Power Query — Nettoyage & Transformation

- Analyse de qualité : **qualité des colonnes**, **distribution** et **profil**
- Vérification et correction des types de données
- Renommage des colonnes :

| Colonne initiale | Nouveau nom |
|---|---|
| OrderID | Id commande |
| CustomerID | Id client |
| CompanyName | Nom client |
| ProductID | Id produit |
| ProductName | Nom produit |
| Category | Catégorie produit |
| RegionID | Id région |
| RegionName | Nom région |
| OrderDate | Date commande |
| Quantity | Quantité |
| UnitPrice | Prix unitaire |
| TotalPrice | Prix total |
| OrderStatus | Statut commande |

- Normalisation en 4 tables via l'option **Référence** :
  - `Clients` — regroupement par Id client & Nom client
  - `Produits` — regroupement par Id produit, Nom produit & Catégorie
  - `Régions` — regroupement par Id région & Nom région
  - `Ventes` — table de faits (colonnes redondantes supprimées)

---

## 🗂️ Modèle de données — Schéma en étoile

```
         Clients
            |
Régions — Ventes — Produits
```

- Relations **1 à plusieurs** entre la table de faits `Ventes` et chaque dimension
- Relations vérifiées automatiquement puis recréées manuellement pour validation

---

## 📐 Mesures DAX

Toutes les mesures sont regroupées dans une table dédiée `Mesures`.

### Onglet Suivi des ventes

```dax
Total ventes = SUM(Ventes[Prix total])
Nombre de commandes = DISTINCTCOUNT(Ventes[Id commande])
Quantité vendue = SUM(Ventes[Quantité])
Commande moyenne = DIVIDE([Total ventes], [Nombre de commandes])
```

### Onglet Commandes annulées

```dax
Total commandes annulées =
    CALCULATE(COUNT(Ventes[Id commande]), Ventes[Statut commande] = "Cancelled")

Montant commandes annulées =
    CALCULATE(SUM(Ventes[Prix total]), Ventes[Statut commande] = "Cancelled")

Pourcentage commandes annulées =
    DIVIDE([Total commandes annulées], [Nombre de commandes])
```

---

## 📊 Rapport — Structure des onglets

### Onglet 1 — Suivi des ventes

| Visuel | Détail |
|---|---|
| 4 KPI | Total ventes · Nb commandes · Quantité vendue · Commande moyenne |
| Courbe / Aires | Évolution du CA et des volumes dans le temps |
| Barres horizontales | Répartition du CA par région |
| Donut | Répartition du CA par catégorie de produit |
| Tableau | Détail des commandes (id, client, produit…) |
| Filtres | Plage de dates · Statut commande · Région |

### Onglet 2 — Commandes annulées

| Visuel | Détail |
|---|---|
| 3 KPI | Total annulées · Montant annulé · % annulations |
| Courbe | Évolution du % et du nombre de commandes annulées |
| Histogramme vertical | Part des annulations par région |
| Treemap | CA annulé par catégorie et par produit |
| Ruban (Ribbon) | Évolution trimestrielle des annulations par produit |

### Navigation & Tooltips

- Menu latéral avec icônes — navigation entre les onglets
- **Tooltip 1** (histogramme région) → courbe d'évolution des ventes
- **Tooltip 2** (donut catégorie) → ruban d'évolution par produit

---

## 🎨 Thème & Style

| Paramètre | Valeur |
|---|---|
| Thème | JSON personnalisé importé |
| Fond de page | `#1E2D38` |
| Couleur des briques | `#232448` |
| Hauteur de page | 2000 px |

---

## ⭐ Fonctionnalités avancées (Bonus)

### Signets (Bookmarks)
- **Mobiles** — filtre sur Smartphone, Headphone, SmartWatch
- **Bureautique** — filtre sur Tablet, Monitor, Laptop, Keyboard, Mouse
- **Reset** — réinitialisation de tous les filtres

### Sécurité (Row-Level Security)
- **Rôle 1** — accès uniquement aux sociétés `AI Systems` et `TechCorp`
- **Rôle 2** — accès uniquement à la région `South`

### Vue mobile
- Premier onglet adapté à la mise en page mobile
- Filtres conservés (date, statut, région)
- Tableau détaillé omis

### KPI Donut personnalisé
- Visualisation **PayPal KPI Donut Chart**
- Affiche la part du CA filtré autour du KPI "Total ventes"

### Publication
- Rapport publié sur **Power BI Service** via le fichier `.pbix`

---

## 📂 Fichiers du projet

```
📦 projet-powerbi-sales
 ┣ 📄 sales_2.csv          ← données source
 ┣ 📄 dashboard.pbix       ← fichier Power BI
 ┗ 📄 README.md
```
