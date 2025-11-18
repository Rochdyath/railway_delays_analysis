# Note méthodologique - Analyse exploratoire

---

## Problème choisi

### Présentation du client  
Le client est un acteur ferroviaire (cas fictif basé sur les données publiques d’Infrabel) souhaitant comprendre les dynamiques d’incidents sur son réseau.

### Problématique évoquée  
> "Nous voulons identifier les causes majeures de perturbation du trafic et comprendre si certains lieux ou saisons sont plus critiques"

### Problème reformulé  
> **Comment analyser les incidents ferroviaires pour identifier les lieux, causes et périodes critiques ?**

---

## Dataset retenu

### Source  
Dataset public fourni par **Infrabel** ([Lien](https://opendata.infrabel.be/explore/dataset/belangrijkste-incidenten/information/?flg=fr-fr&disjunctive.lijn&disjunctive.description_de_l_incident&disjunctive.lieu&sort=date&dataChart=eyJxdWVyaWVzIjpbeyJjaGFydHMiOlt7InR5cGUiOiJjb2x1bW4iLCJmdW5jIjoiQVZHIiwieUF4aXMiOiJtaW5fZGVsYXkiLCJzY2llbnRpZmljRGlzcGxheSI6dHJ1ZSwiY29sb3IiOiIjMEQ5RkZGIn1dLCJ4QXhpcyI6ImFhcmRfdmFuX2luY2lkZW50IiwibWF4cG9pbnRzIjoiIiwidGltZXNjYWxlIjoiIiwic29ydCI6IiIsInNlcmllc0JyZWFrZG93blRpbWVzY2FsZSI6IiIsImNvbmZpZyI6eyJkYXRhc2V0IjoiYmVsYW5ncmlqa3N0ZS1pbmNpZGVudGVuIiwib3B0aW9ucyI6eyJmbGciOiJmci1mciIsImRpc2p1bmN0aXZlLmxpam4iOnRydWUsImRpc2p1bmN0aXZlLmRlc2NyaXB0aW9uX2RlX2xfaW5jaWRlbnQiOnRydWUsImRpc2p1bmN0aXZlLmxpZXUiOnRydWUsInNvcnQiOiJkYXRlIn19fV0sImRpc3BsYXlMZWdlbmQiOnRydWUsImFsaWduTW9udGgiOnRydWUsInRpbWVzY2FsZSI6IiJ9&calendarview=month)).

### Structure  
Chaque ligne = un incident, avec informations sur :  
- Date  
- Lieu  
- Ligne  
- Cause  
- Minutes de retard  
- Trains supprimés  

### Pertinence  
Le dataset permet :  
- l’analyse spatiale (lieux, lignes),  
- l’analyse temporelle (mois, saisons),  
- l’analyse causale.

---

## Technologies utilisées

### Stack technique
- **Python** pour le traitement et l’analyse  
- **Pandas** pour la manipulation des données  
- **Matplotlib**, **Seaborn** pour la visualisation  
- **SciPy** pour les tests statistiques  
- **Tableau** pour le dashboard  
- Notebook Jupyter pour la reproductibilité

---

## Hypothèses et tests

### Hypothèse 1 : Les lignes influencent les retards  
- Test : **ANOVA** puis **Kruskal–Wallis**  
- Résultat : **p-value > 0.05**  
- Coclusion : Aucune différence significative entre les lignes.


### Hypothèse 2 : Certains lieux concentrent les incidents  
- Test : **ANOVA** puis **Kruskal–Wallis**  
- Résultat :  **p-value > 0.05**
- Coclusion : Aucune différence significative entre les stations.


### Hypothèse 3 : Les saisons influencent les retards  
- Test : **ANOVA** puis **Kruskal–Wallis**  
- Résultat : dans les données présentes, **pas de saison significativement plus perturbée** mais **par mois si**


### Hypothèse 4 : Certaines causes génèrent plus de retards  
- Analyse : top causes selon fréquence, minutes et suppressions  
- Résultat : **certaines causes rares provoquent énormément de perturbations**.

---

## Synthèse des résultats

### Causes les plus impactantes  
Trois vues complémentaires :  
- Top 10 causes par fréquence  
- Top 10 par minutes de retard  
- Top 10 par trains supprimés  


### Saisonnalité et mois  
- Barplot incidents/mois  
- Barplot incidents/saisons  
Pas de saison particulièrement problématique, mais certains mois concentrent plus d’incidents.

### Rail Traffic Score (RTS)  
Score construit à partir de :  
- minutes perdues (poids 0.33)  
- suppressions (poids 0.33)  
- incidents (poids 0.33)

Normalisation (min-max) + pondération = score entre **0 et 100**.  

Interprétation :  
- **80–100** : critique  
- **60–80** : mauvais  
- **40–60** : fragile  
- **20–40** : bon  
- **0–20** : excellent

Permet de suivre l’état du réseau quotidiennement.

---

## Recommandations au client

### Utiliser le RTS comme indicateur de pilotage  
Un score unique, simple, compréhensible par tous les services.

### Cibler les causes à fort impact  
Certaines causes rares génèrent la majorité des minutes de retard → interventions prioritaires.

### Ne pas baser les actions sur les lignes ou les saisons  
Les tests montrent **aucune différence statistique significative**. Cela ne veut pas dire qu'il faut les négliger.

---

## Limites & améliorations

###  Limites
- Manque de données sur le trafic réel (nombre de trains/jour)  
- Absence des conditions météo  
- Causes parfois trop génériques  

### Améliorations possibles  
- Ajouter un module prédictif basé sur le RTS  
- Normaliser les retards par train-km pour une mesure plus juste  
- Ajouter des données météo pour expliquer les variations  
- Créer un clustering géographique des zones sensibles  

---

