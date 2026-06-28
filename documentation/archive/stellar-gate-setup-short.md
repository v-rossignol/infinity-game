# Infinity - StellarGate (Client Login) - Setup Projet

**Date** : 7 juin 2026  
**Modèle** : Vibe (Mistral Medium 3.5)  
**Auteur** : Roro LeSage  
**Type** : Documentation Technique - Setup Projet

---

## **1. Introduction**

**StellarGate** est le client dédié à l'**authentification** (login, création de compte, récupération de mot de passe) pour le jeu **Infinity**. Ce client est une **application web légère** qui communique avec le serveur pour gérer les comptes joueurs.

**Objectifs** :

- Permettre aux joueurs de **se connecter** à leur compte.
- Permettre la **création de compte** (inscription).
- Gérer la **récupération de mot de passe** (optionnel).
- Rediriger vers le **Client Galaxie** après une authentification réussie.

---

## **2. Stack Technologique**

### **A. Core Technologies**


| **Catégorie**        | **Technologie**                                                                                       | **Version Recommandée** | **Rôle**                                                                |
| -------------------- | ----------------------------------------------------------------------------------------------------- | ----------------------- | ----------------------------------------------------------------------- |
| **Framework UI**     | [React](https://react.dev/)                                                                           | v18.x                   | Construction de l'interface utilisateur (formulaires, boutons, etc.).   |
| **State Management** | [Zustand](https://github.com/pmndrs/zustand)                                                          | v4.x                    | Gestion d'état légère pour les formulaires et l'état de connexion.      |
| **Routing**          | [React Router](https://reactrouter.com/)                                                              | v6.x                    | Navigation entre les pages (login, inscription, etc.).                  |
| **Formulaires**      | [React Hook Form](https://react-hook-form.com/)                                                       | v7.x                    | Gestion des formulaires avec validation.                                |
| **Validation**       | [Zod](https://zod.dev/)                                                                               | v3.x                    | Validation des données des formulaires.                                 |
| **Communication**    | [Axios](https://axios-http.com/)                                                                      | v1.x                    | Requêtes HTTP vers le serveur pour l'authentification.                  |
| **Socket.IO**        | [Socket.IO Client](https://socket.io/docs/v4/client-api/)                                             | v4.x                    | Optionnel : pour une synchronisation en temps réel (ex: notifications). |
| **UI Components**    | [MUI](https://mui.com/) ou [Chakra UI](https://chakra-ui.com/)                                        | v5.x / v2.x             | Bibliothèques de composants prêts à l'emploi pour un design moderne.    |
| **Animations**       | [Framer Motion](https://www.framer.com/motion/)                                                       | v10.x                   | Animations fluides pour les transitions et effets visuels.              |
| **Bundler**          | [Vite](https://vitejs.dev/)                                                                           | v5.x                    | Build rapide et optimisé pour le développement web moderne.             |
| **Langage**          | TypeScript                                                                                            | v5.x                    | Typage statique pour une meilleure maintenabilité.                      |
| **Styles**           | [Tailwind CSS](https://tailwindcss.com/) ou [CSS Modules](https://github.com/css-modules/css-modules) | v3.x                    | Styling des composants.                                                 |
