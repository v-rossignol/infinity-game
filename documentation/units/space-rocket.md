# Construction d'une Fusée : Extractions et Transformations de Ressources

```yaml
date: 2026-07-03
author: Roro LeSage
model: Mistral Medium 3.5
source: 
  - resources.md
```

---

## **1. Extraction des Ressources de Base**
Les ressources nécessaires pour construire une fusée sont réparties selon les terrains. Voici les **ressources prioritaires** et leur utilisation.
   **Terrain**   | **Ressource**       | **Utilisation dans la fusée**                          | **Quantité estimée** |
 |---------------|---------------------|-------------------------------------------------------|-----------------------|
 | Mountain      | Iron ore            | Structure, réservoirs, cadre                          | 500                   |
 | Mountain      | Copper ore          | Câblage électrique, alliages                          | 300                   |
 | Mountain      | Coal                | Combustible (fusion des métaux), énergie             | 400                   |
 | Mountain      | Stone               | Revêtement thermique, protection                      | 600                   |
 | Volcanic       | Sulfur              | Propergol (combustible de fusée), acide sulfurique     | 200                   |
 | Volcanic       | Basalt              | Boucliers thermiques, protection radiative           | 400                   |
 | Volcanic       | Obsidian            | Revêtement résistant à la chaleur                     | 300                   |
 | Desert        | Silica              | Verre (hublots, panneaux solaires)                    | 500                   |
 | Desert        | Oil*          | Carburant (raffinage en kérosène)                      | 800                   |
 | Desert        | Alkaline minerals   | Batteries (systèmes électriques)                      | 100                   |
 | Desert        | Lithium*      | Batteries lithium-ion (systèmes critiques)            | 50                    |
 | Ice            | Ice                 | Oxygène (électrolyse) + hydrogène (carburant)          | 1000                  |
 | Ice            | Cryogenic materials | Isolants thermiques, réservoirs cryogéniques          | 200                   |
 | Plain          | Food                | Nourriture pour équipage (si habité)                   | 50                    |
 | Plain          | Fresh water         | Oxygène (électrolyse), consommation                   | 100                   |

*Ressources occasionnelles à activer pour la construction de la fusée.

---

---

## **2. Transformations Intermédiaires**

---

### **2.1 Métallurgie (Forge)**
 | **Type**          | **Transformation**       | **Entrées**                     | **Sortie**            | **Quantité** | **Notes**                                      |
 |-------------------|--------------------------|---------------------------------|------------------------|--------------|------------------------------------------------|
 | Fondue            | Lingot de fer            | Iron ore (10) + Coal (5)         | Iron ingot            | 8            | Base pour la structure.                         |
 | Fondue            | Lingot de cuivre         | Copper ore (10) + Coal (3)       | Copper ingot          | 7            | Pour câblage et alliages.                       |
 | Alliage           | Acier                    | Iron ingot (8) + Coal (8)        | Steel ingot           | 6            | Structure principale de la fusée.              |
 | Alliage           | Acier inoxydable         | Steel ingot (6) + Chromium*      | Stainless steel       | 4            | Réservoirs de carburant, coque résistante.       |
 | Alliage           | Bronze                   | Copper ingot (7) + Tin*          | Bronze ingot          | 5            | Composants mécaniques (vannes, engrenages).     |
 | Alliage           | Allage léger             | Bauxite (10) + Énergie (20)      | Aluminum ingot        | 8            | Réduction de poids pour la coque.               |
 | Revêtement         | Bouclier thermique       | Basalt (20) + Steel ingot (5)    | Thermal shield        | 1            | Protection contre la rentrée atmosphérique.    |

*Ressources à ajouter : `Chromium` (Montagne), `Tin` (Montagne), `Bauxite` (Forêt, occasionnelle).

---

### **2.2 Raffinage (Carburants et Chimie)**
 | **Type**          | **Transformation**       | **Entrées**                     | **Sortie**            | **Quantité** | **Notes**                                      |
 |-------------------|--------------------------|---------------------------------|------------------------|--------------|------------------------------------------------|
 | Raffinage         | Kérosène                 | Oil (100)                 | Kerosene              | 60           | Carburant principal pour moteurs-fusées.        |
 | Électrolyse       | Oxygène liquide          | Ice (100) + Énergie (50)        | Liquid oxygen (LOX)   | 30           | Oxydant pour carburant (LOX/RP-1 ou LOX/LH2).   |
 | Électrolyse       | Hydrogène liquide        | Ice (100) + Énergie (50)        | Liquid hydrogen (LH2) | 20           | Carburant haute performance.                  |
 | Chimie            | Propergol solide         | Sulfur (10) + Coal (5) + Nitre (5) | Solid propellant    | 8            | Pour boosters ou petits moteurs.                |
 | Chimie            | Hydrazine*               | Alkaline minerals (20) + Nitre (10) | Hydrazine       | 5            | Carburant pour moteurs auxiliaires.            |
 | Chimie            | Peroxyde d’azote*        | Nitre (10) + Fresh water (5)    | Nitrogen tetroxide    | 4            | Oxydant pour propergols hypergoliques.          |

*Ressources à ajouter : `Hydrazine`, `Nitrogen tetroxide`.

---

### **2.3 Matériaux Avancés**
 | **Type**          | **Transformation**       | **Entrées**                     | **Sortie**               | **Quantité** | **Notes**                                      |
 |-------------------|--------------------------|---------------------------------|--------------------------|--------------|------------------------------------------------|
 | Verrerie          | Verre borosilicate       | Silica (50) + Boron* (5)        | Borosilicate glass      | 30           | Hublots résistants à la chaleur.               |
 | Composite         | Fibre de carbone         | Silica (100) + Coal (30)         | Carbon fiber             | 20           | Structure légère pour coque ou réservoirs.     |
 | Isolants          | Mousse isolante          | Cryogenic materials (20) + Plastic* (10) | Foam insulation | 15 | Isolation thermique pour réservoirs. |
 | Électronique      | Circuits imprimés        | Copper ingot (5) + Silica (10) + Gold* (1) | PCB | 1 | Pour systèmes de contrôle. |

*Ressources à ajouter : `Boron` (Désert), `Plastic` (dérivé du pétrole), `Gold` (Montagne, occasionnel).

---

---
---
## **3. Fabrication des Composants de la Fusée**
 | **Composant**          | **Ressources requises**                          | **Quantité** | **Bâtiment requis**       | **Notes**                                      |
 |------------------------|------------------------------------------------|--------------|----------------------------|------------------------------------------------|
 | **Structure**          | Steel ingot (200) + Aluminum ingot (100)        | 1            | Usine d’assemblage         | Coque principale.                              |
 | **Réservoirs**         | Stainless steel (150) + Thermal shield (10)     | 1            | Atelier de métallurgie     | Pour LOX, LH2, et kérosène.                    |
 | **Moteur principal**  | Steel ingot (50) + Copper ingot (30) + Kerosene (200) | 1 | Usine de moteurs | Moteur à combustion (type Merlin ou Raptor). |
 | **Moteur à propergol solide** | Solid propellant (50) + Steel ingot (20) | 1 | Usine de moteurs | Pour boosters. |
 | **Système électrique** | Copper ingot (50) + Gold* (5) + PCB (10)        | 1            | Atelier d’électronique   | Câblage, ordinateurs de bord.                 |
 | **Bouclier thermique** | Thermal shield (50) + Borosilicate glass (20) | 1 | Atelier de revêtement | Protection pour la rentrée. |
 | **Réservoirs cryogéniques** | Aluminum ingot (80) + Foam insulation (30) | 1 | Usine cryogénique | Pour LH2/LOX. |
 | **Panneaux solaires**  | Silica (100) + Copper ingot (20)               | 5            | Atelier d’énergie         | Alimentation électrique en orbite.          |
 | **Batteries**          | Lithium battery (20) + Alkaline minerals (50) | 10           | Atelier d’énergie         | Stockage d’énergie pour systèmes critiques.|
 | **Système de support vie** | Fresh water (50) + Food (20) + Oxygen (100) | 1 | Laboratoire | Pour équipage (si fusée habitée). |
 | **Hublots**            | Borosilicate glass (10)                        | 5            | Verrerie                   | Observation et éclairage.                    |
 | **Train d’atterrissage** | Steel ingot (40) + Rubber* (20)              | 1            | Atelier mécanique         | Pour atterrissage (si réutilisable).         |
 | **Système de navigation** | PCB (5) + Gold* (3) + Quartz* (2)          | 1 | Atelier d’électronique | Pour guidage. |

*Ressources à ajouter : `Rubber` (dérivé du pétrole + soufre), `Quartz` (Désert/Montagne).

---
---
---
## **4. Assemblage Final de la Fusée**
 | **Étape**              | **Composants requis**                          | **Durée estimée** | **Bâtiment requis**       | **Notes**                                      |
 |------------------------|-----------------------------------------------|-------------------|----------------------------|------------------------------------------------|
 | **Étape 1 : Coque**    | Structure (1) + Réservoirs (1) + Bouclier thermique (1) | 10 jours | Hangar spatial | Assemblage de la structure de base. |
 | **Étape 2 : Propulsion** | Moteur principal (1) + 2x Moteur à propergol solide | 5 jours | Hangar spatial | Installation des moteurs. |
 | **Étape 3 : Systèmes** | Système électrique (1) + Système de navigation (1) + Panneaux solaires (5) | 7 jours | Hangar spatial | Intégration des systèmes critiques. |
 | **Étape 4 : Finitions** | Hublots (5) + Train d’atterrissage (1) + Système de support vie (1) | 3 jours | Hangar spatial | Finalisation pour le lancement. |
 | **Étape 5 : Remplissage** | Kerosene (500) + LOX (300) + LH2 (200) | 1 jour | Station de ravitaillement | Carburant pour le vol. |

---
---
---
## **5. Ressources Manquantes à Implémenter**

### **5.1 Ressources**
 | **ID**            | **Nom**               | **Terrain**       | **Type**          | **Utilisation**                          |
 |-------------------|-----------------------|-------------------|-------------------|-----------------------------------------|
 | `chromium`        | Chromium              | Mountain          | Occasionnelle    | Acier inoxydable.                        |
 | `tin`             | Tin                   | Mountain          | Occasionnelle    | Alliage bronze.                          |
 | `bauxite`         | Bauxite               | Forest            | Occasionnelle    | Aluminium.                               |
 | `aluminum-ingot`  | Aluminum Ingot        | N/A               | Transformée      | Structure légère.                       |
 | `boron`           | Boron                 | Desert            | Occasionnelle    | Verre borosilicate.                      |
 | `plastic`         | Plastic               | N/A               | Transformée      | Isolants, composites.                    |
 | `gold`            | Gold                  | Mountain          | Occasionnelle    | Électronique.                            |
 | `quartz`          | Quartz                | Desert/Mountain   | Permanente       | Électronique (oscillateurs).             |
 | `rubber`          | Rubber                | N/A               | Transformée      | Train d’atterrissage, joints.            |
 | `hydrazine`       | Hydrazine             | N/A               | Transformée      | Propergol pour moteurs auxiliaires.      |
 | `nitrogen-tetroxide` | Nitrogen Tetroxide | N/A               | Transformée      | Oxydant pour propergols hypergoliques.   |
 | `kerosene`        | Kerosene              | N/A               | Transformée      | Carburant principal.                     |

---

### **5.2 Bâtiments**
 | **Nom**               | **Description**                          | **Ressources requises pour construction** | **Fonction**                          |
 |-----------------------|-----------------------------------------|---------------------------------------------|---------------------------------------|
 | **Fonderie avancée**  | Permet de fondre tous les métaux.        | Stone (200) + Iron ingot (50) + Coal (100)  | Production de lingots.                |
 | **Raffinerie**        | Raffinage du pétrole en carburants.     | Stone (150) + Steel ingot (30) + Copper ingot (20) | Production de kérosène, plastique. |
 | **Usine chimique**    | Production de propergols et produits chimiques. | Steel ingot (50) + Glass (20) + Alkaline minerals (30) | Hydrazine, acide sulfurique, etc. |
 | **Usine cryogénique** | Manipulation de liquides cryogéniques. | Stainless steel (40) + Cryogenic materials (20) | LOX, LH2. |
 | **Atelier d’électronique** | Fabrication de circuits et composants. | Copper ingot (30) + Silica (50) + Gold (5) | PCB, systèmes de contrôle. |
 | **Hangar spatial**    | Assemblage final de la fusée.           | Steel ingot (500) + Aluminum ingot (200) + Glass (100) | Assemblage des composants. |
 | **Station de ravitaillement** | Remplissage en carburant. | Steel ingot (100) + Concrete (50) | Stockage et distribution de carburant. |

---
---
---
## **6. Chaîne de Production Simplifiée (Exemple Concret)**
Voici un **exemple de chemin critique** pour construire une fusée basique (1 étage, non réutilisable) :

1. **Extraction** :
   - Mountain : Iron ore (500), Copper ore (300), Coal (400), Stone (600).
   - Volcanic : Sulfur (200), Basalt (400).
   - Desert : Silica (500), Oil (800).
   - Ice : Ice (1000).

2. **Transformations** :
   - Iron ore (500) + Coal (250) → Iron ingot (400).
   - Copper ore (300) + Coal (90) → Copper ingot (210).
   - Iron ingot (240) + Coal (320) → Steel ingot (180).
   - Steel ingot (100) + Chromium (20) → Stainless steel (80).
   - Oil (800) → Kerosene (480) + Plastic (100).
   - Ice (1000) + Énergie (500) → LOX (300) + LH2 (200).
   - Sulfur (200) + Coal (100) + Nitre (100) → Solid propellant (160).

3. **Fabrication des composants** :
   - Steel ingot (180) + Aluminum ingot (100) → Structure (1).
   - Stainless steel (80) + Thermal shield (10) → Réservoirs (1).
   - Steel ingot (50) + Copper ingot (30) + Kerosene (200) → Moteur principal (1).
   - Solid propellant (50) + Steel ingot (20) → Moteur à propergol solide (2).
   - Copper ingot (50) + Gold (5) + PCB (10) → Système électrique (1).

4. **Assemblage final** :
   - Structure + Réservoirs + Moteur principal + 2x Moteur à propergol solide + Système électrique + Bouclier thermique → **Fusée basique**.

---
---
---
## **7. Idées pour Étendre la Complexité**

### **7.1 Fusée Réutilisable**
- Ajouter des **tuiles de protection thermique** (à base de basalte et de carbone) pour la rentrée.
- Intégrer un **train d’atterrissage** (Steel + Rubber).
- Utiliser des **moteurs à propergol liquide réallumables** (LOX/LH2).

### **7.2 Fusée Habitée**
- Ajouter un **module de commande** (Aluminum + Borosilicate glass + PCB).
- Intégrer un **système de support vie** (Oxygène, eau, nourriture).
- Inclure des **sièges éjectables** (pour la sécurité).

### **7.3 Fusée Multi-Étages**
- **1er étage** : Moteurs à propergol solide (boosters).
- **2e étage** : Moteur à kérosène/LOX (principal).
- **3e étage** : Moteur à LH2/LOX (haute performance pour l’orbite).

### **7.4 Carburants Alternatifs**
- **Méthane liquide** (CH4) : Plus efficace que le kérosène, mais nécessite du `methane-ice` (Ice terrain).
- **Propergol hypergolique** : Hydrazine + Nitrogen Tetroxide (pour moteurs réallumables).

---
---
---
## **8. Équilibrage Suggéré**

- **Coûts** :
  - Une fusée basique devrait nécessiter **au moins 1000 unités de ressources transformées** (hors carburant).
  - Le carburant seul représente **50-60% du coût total** (ex: 500 LOX + 300 LH2 + 200 Kérosène).

- **Temps** :
  - Extraction : 1-2 jours (selon le nombre de joueurs/unités).
  - Transformations : 3-5 jours (dépend des bâtiments disponibles).
  - Assemblage : 2-3 jours.

- **Dépendances technologiques** :
  - Déverrouiller la **fonderie avancée** avant de pouvoir produire des lingots.
  - Déverrouiller la **raffinerie** pour le kérosène et le plastique.
  - Déverrouiller l’**usine chimique** pour les propergols avancés.