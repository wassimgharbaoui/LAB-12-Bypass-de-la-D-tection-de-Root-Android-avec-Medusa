# Contournement des vérifications d'intégrité sous Android – Atelier pratique

## Contexte
Ce document décrit une approche pratique pour neutraliser les alertes de sécurité déclenchées par des modifications système sur un appareil Android virtuel. La méthode repose sur l'injection de code dynamique pour tromper une application qui détecte un environnement altéré.

---

## Étape 0 : Vérification des prérequis

S'assurer que la machine hôte dispose des outils en ligne de commande nécessaires : interpréteur Python, client de débogage (ADB) et un agent d'instrumentation dynamique.

<img width="986" height="305" alt="Screenshot 2026-05-11 184128" src="https://github.com/user-attachments/assets/ed4e161a-d98f-42ac-a70b-87ed41e06a40" />


---

## Étape 1 : Préparation du périphérique

### 1.1 Établissement de la communication
Utiliser le client de débogage pour lister les appareils connectés. Récupérer également l'architecture CPU de la cible – cela déterminera la variante de l'agent d'instrumentation à déployer.

<img width="808" height="207" alt="Screenshot 2026-05-11 184135" src="https://github.com/user-attachments/assets/628c7ff1-4063-4669-b0c5-4b0c74cd6ee1" />


### 1.2 Déploiement de l'agent d'instrumentation
Transférer le binaire de l'agent vers un emplacement accessible en écriture sur l'appareil, attribuer les droits d'exécution, puis le lancer en arrière-plan.

<img width="48" height="46" alt="Screenshot 2026-05-11 184143" src="https://github.com/user-attachments/assets/0d482efc-b25b-4254-96de-afe2e95959e0" />


Pour confirmer le bon fonctionnement de l'agent, demander la liste des processus actifs.

<img width="975" height="741" alt="Screenshot 2026-05-11 184155" src="https://github.com/user-attachments/assets/71e28a27-a04a-4800-89c9-fc1e321e5e51" />


---

## Étape 2 : Obtention du framework d'instrumentation

Télécharger le code source du framework depuis son dépôt en ligne et installer les dépendances Python nécessaires.

<img width="330" height="646" alt="Screenshot 2026-05-11 184443" src="https://github.com/user-attachments/assets/ced6864c-661e-4fec-a43c-194b8a7fe577" />
<img width="491" height="35" alt="Screenshot 2026-05-11 184223" src="https://github.com/user-attachments/assets/8c259ee0-d016-4f79-a9f3-abe12a2ba165" />

---

## Étape 3 : Inspection de l'application cible

### 3.1 Comportement par défaut
L'application de test de sécurité (qui vérifie l'intégrité système) signale initialement que l'appareil est dans un état modifié.

<img width="330" height="646" alt="Screenshot 2026-05-11 184443" src="https://github.com/user-attachments/assets/07071946-2f0a-486f-91c2-e2d7f15c3167" />


### 3.2 Attachement du framework
Lancer le script principal du framework, sélectionner l'appareil connecté, puis parcourir la liste des paquets installés. Le nom du paquet cible est noté pour une utilisation ultérieure.

<img width="523" height="772" alt="Screenshot 2026-05-11 184511" src="https://github.com/user-attachments/assets/0c8c3519-8d29-4fd5-bd6c-6152c66168ac" />

---

## Étape 4 : Technique de contournement

### 4.1 Recherche d'un script adapté
Parcourir la collection de scripts intégrés au framework qui ciblent les vérifications d'intégrité.

<img width="592" height="132" alt="Screenshot 2026-05-11 184517" src="https://github.com/user-attachments/assets/bb94ce24-a88d-4503-a969-8866524b1f12" />


### 4.2 Activation du script
Activer le script spécialement conçu pour la logique de protection de l'application de test.

<img width="762" height="95" alt="Screenshot 2026-05-11 184521" src="https://github.com/user-attachments/assets/f106aa39-6932-47a1-9845-64de0abd9a64" />


### 4.3 Injection du script
Lancer l'application cible via le framework. La sortie console confirme que plusieurs méthodes de vérification ont été interceptées (recherche de binaires, validations de chemins, etc.).

<img width="712" height="808" alt="Screenshot 2026-05-11 184540" src="https://github.com/user-attachments/assets/5e4f00f4-a596-4a38-ab4d-baeeae8da268" />

---

## Étape 5 : Confirmation du résultat

Après l'injection du script, l'application rafraîchit son statut et indique désormais que l'appareil semble non modifié. Le contournement de la vérification d'intégrité est réussi.

<img width="327" height="601" alt="Screenshot 2026-05-11 184604" src="https://github.com/user-attachments/assets/a9370e36-83aa-4140-a86d-2a2baeb597b4" />


---


