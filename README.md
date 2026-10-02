# Relevé MT — Application Android de relevé des compteurs Moyenne Tension

Application **100 % hors ligne** (Kotlin, Jetpack Compose, Room). Aucune permission Internet, aucune donnée envoyée à un serveur.

## 1. Ouvrir le projet
1. Installer **Android Studio** (version récente, Koala ou plus).
2. `File > Open` puis choisir le dossier `ReleveMT`.
3. Attendre la fin de la « synchronisation Gradle » (connexion Internet nécessaire la 1ʳᵉ fois seulement, pour télécharger les bibliothèques).

## 2. Compiler et générer l'APK
**Avec Android Studio :** `Build > Build Bundle(s) / APK(s) > Build APK(s)`.
L'APK se trouve dans `app/build/outputs/apk/debug/app-debug.apk`.

**En ligne de commande :** `./gradlew assembleDebug` (Windows : `gradlew.bat assembleDebug`). Java 17 requis.

**Sans installer Android Studio (GitHub) :**
1. Créer un dépôt GitHub **privé** (les données clients ne font pas partie du projet, mais restez prudent).
2. Envoyer tout le contenu du dossier `ReleveMT` sur la branche `main`.
3. Onglet **Actions** > « Compiler l'APK » > attendre la fin > télécharger l'artefact **ReleveMT-apk**.

## 3. Installer l'APK sur le téléphone
1. Copier `app-debug.apk` sur le téléphone.
2. L'ouvrir ; autoriser « Installer des applications inconnues » si Android le demande.
3. Android 8.0 ou plus est requis.

## 4. Importer la base clients (fichier Excel)
1. Copier le fichier `.xlsx` sur le téléphone.
2. Accueil > **IMPORTER LA BASE CLIENTS** > choisir le fichier > **Confirmer**.
3. Un récapitulatif s'affiche : clients importés, mis à jour, doublons, erreurs.
- Clé d'identification : **numéro de contrat** (à défaut, numéro de compteur).
- Réimporter le même fichier ne crée **aucun doublon** : les clients existants sont mis à jour.
- L'importation ne supprime jamais de client ni de relevé.

## 5. Effectuer un relevé
1. Accueil : taper un numéro de contrat, de compteur ou un nom (recherche partielle).
2. Toucher le client > **Nouveau relevé**.
3. Saisir les 22 cadrans (les champs vides seront enregistrés à **0**).
4. **ENREGISTRER LE RELEVÉ**. La date et l'heure sont enregistrées automatiquement.
- Un même compteur peut être relevé autant de fois que nécessaire : **chaque relevé est conservé**.
- Un relevé peut être corrigé depuis l'historique (**Modifier ce relevé**) ; la date d'origine est conservée.

## 6. Exporter les relevés (CSV pour Excel)
1. Accueil > **EXPORTER LES RELEVÉS**.
2. Choisir l'année et le mois > **Exporter le mois**.
3. Au premier export, choisir le dossier (ex. `Documents/RELEVES_MT`).
- Fichier : `RELEVES_MT_AAAA-MM.csv` (ex. `RELEVES_MT_2026-09.csv`).
- « Exporter tous les relevés » : `RELEVES_MT_COMPLET_AAAA-MM-JJ.csv`.
- Format : UTF-8 avec BOM, séparateur `;`. **Une colonne par relevé**, une ligne par information (Nom client, Compteur, Contrat, Date, Heure, puis les 22 cadrans).
- Les numéros de compteur et de contrat sont écrits sous la forme `="00012345"` pour qu'Excel conserve les zéros initiaux et n'affiche pas de notation scientifique.
- Si l'export échoue, les relevés restent dans l'application : relancez l'export.

## 7. Sauvegarde / restauration
Paramètres > **Sauvegarder la base** (fichier `.db` dans le dossier choisi) ; **Restaurer la base** remplace toutes les données par celles de la sauvegarde (confirmation demandée).

## 8. Tests
- Tests unitaires (logique métier) : `./gradlew testDebugUnitTest`
- Tests Room sur appareil/émulateur : `./gradlew connectedDebugAndroidTest`

## Structure
`core/` règles métier (22 cadrans, CSV, formats) · `importer/` lecture XLSX · `data/` Room · `storage/` dossiers et sauvegarde · `ui/` écrans Compose.
