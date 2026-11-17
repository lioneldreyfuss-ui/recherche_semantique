# Recherche sémantique locale

Retrouver ses documents par leur sens, dans un conteneur chiffré, sans que rien ne quitte l'ordinateur.

Ce dépôt ne contient pas de logiciel. Il contient une **recette** : un texte, le fichier [PROMPT.md](PROMPT.md), que vous donnez à un assistant d'IA (Claude, ChatGPT, Mistral…) pour qu'il vous fabrique l'outil. Vous obtenez un fichier HTML unique, que vous ouvrez dans votre navigateur.

## À quoi sert l'outil

Vous tapez une phrase, par exemple « courrier de relance au client pour un retard de paiement » ou « attestation d'assurance du local ». L'outil vous propose les cinq documents dont le contenu s'en rapproche le plus, même si vous ne vous souvenez ni de leur nom, ni de l'endroit où vous les avez rangés. Un fichier nommé `scan_0047.pdf` ou `template_v3_final.docx` peut ainsi être retrouvé. Un clic sur un résultat l'ouvre dans son application.

L'outil lit les documents Word (.docx), Excel (.xlsx), PDF, texte et e-mails (.eml). Les autres fichiers sont retrouvés par leur nom et leur dossier.

## Pour qui

Pour les avocats et, plus largement, les professionnels soumis au secret qui rangent leurs dossiers sur un support amovible chiffré, par exemple un conteneur VeraCrypt sur un disque externe. Aucune compétence technique n'est nécessaire pour l'utiliser ; il faut seulement savoir copier un texte dans un assistant d'IA.

## Ce qui le distingue

- **Il est conçu pour les supports amovibles chiffrés.** L'index, c'est-à-dire ce que l'outil retient de vos documents pour les retrouver, est enregistré **dans le conteneur lui-même**. Quand vous démontez le conteneur, l'index redevient chiffré avec le reste.
- **Aucun nom de fichier ne reste sur l'ordinateur.** L'outil n'écrit rien sur le disque de la machine, ni dans la mémoire du navigateur, à une exception près, décrite plus bas.
- **La lettre du lecteur n'a pas d'importance.** L'index ne contient que des chemins relatifs au conteneur : que vous le montiez sur G: ou sur K:, il est retrouvé tel quel, sans rien refaire.
- **Seuls les changements sont retraités.** La première indexation est longue ; les suivantes ne traitent que les fichiers nouveaux ou modifiés.
- **Rien à installer.** Un seul fichier HTML, ouvert dans Chrome ou Edge, sous Windows, Mac ou Linux.

## Mode d'emploi

1. **Copiez le prompt.** Dépliez la section [« Le prompt à copier »](#le-prompt-à-copier), plus bas sur cette page, puis cliquez sur l'icône de copie, en haut à droite de la zone grisée. Le même texte, mis en forme pour la lecture, se trouve dans [PROMPT.md](PROMPT.md).
2. **Donnez-le à un assistant d'IA.** Collez-le dans une nouvelle conversation et envoyez-le. Le prompt ne contient aucune donnée vous concernant : vous ne transmettez rien de confidentiel à cette étape.
3. **Enregistrez le fichier obtenu.** L'assistant vous rend un fichier `recherche-semantique.html`. Enregistrez-le, de préférence dans votre conteneur chiffré.
4. **Ouvrez-le dans Chrome ou Edge.** Un double-clic suffit, si l'un de ces navigateurs est votre navigateur par défaut.
5. **Choisissez le volume.** Montez votre conteneur, puis faites glisser son lecteur, depuis « Ce PC » dans l'Explorateur, sur le cadre de la page. Autorisez l'enregistrement quand le navigateur le demande.
6. **Laissez indexer, puis cherchez.** Le compteur indique l'avancement. La recherche fonctionne déjà pendant l'indexation, sur les fichiers déjà traités.

**La première indexation est longue, et c'est normal.** Pour un conteneur entier, comptez environ deux heures : c'est la durée mesurée sur un volume de près de 19 000 fichiers, avec un ordinateur ancien. Elle varie selon le nombre de fichiers, la vitesse du disque et la puissance de la machine. L'outil lit chaque document et le fait analyser par le modèle, sur un seul cœur du processeur. Ce temps ne se paie qu'une fois : les ouvertures suivantes ne traitent que les fichiers nouveaux ou modifiés, et ne prennent que quelques minutes au plus. Pendant la première indexation, laissez l'onglet ouvert et désactivez la mise en veille de l'ordinateur.

Les fois suivantes, il suffit de monter le conteneur, d'ouvrir le fichier HTML et de redéposer le lecteur.

## Le prompt à copier

Cliquez sur la ligne ci-dessous pour déplier le prompt, puis sur l'icône de copie, en haut à droite de la zone grisée. Collez-le tel quel dans une nouvelle conversation avec un assistant d'IA.

<details>
<summary><strong>Afficher le prompt</strong></summary>

````text
# Recherche sémantique locale — prompt de fabrication

Je suis avocat. Fabrique-moi une application, « Recherche sémantique », sous la forme d'**un seul fichier HTML** que j'ouvre d'un double-clic dans Chrome ou Edge, sur Windows, Mac ou Linux. Je ne veux rien installer : ni Python, ni serveur, ni extension.

L'application retrouve mes documents par leur sens, et non par des mots exacts. Je tape « courrier de relance au client pour un retard de paiement » et elle me propose les cinq fichiers les plus proches, même si le bon s'appelle `scan_0047.pdf` ou `template_v3_final.docx`. Un clic sur un résultat ouvre le fichier dans son application (Word, Excel, le lecteur PDF…).

Mes dossiers professionnels sont rangés dans des conteneurs chiffrés VeraCrypt, sur un disque externe. Je monte le conteneur sur une lettre de lecteur qui change d'une fois à l'autre. L'application doit donc fonctionner avec un volume amovible et chiffré, dont la lettre n'est jamais la même, et ne rien laisser sur le disque de l'ordinateur.

Mes documents sont confidentiels : aucun fichier, aucun extrait, aucun nom de fichier ne doit jamais quitter mon ordinateur. Tout le traitement se fait dans le navigateur. Seuls les outils (trois bibliothèques et un petit modèle de recherche) sont téléchargés depuis Internet.

Les choix techniques ci-dessous ont déjà été testés sur un volume réel de près de 19 000 fichiers : applique-les tels quels, sans les discuter ni proposer d'alternative.

---

## 1. Contraintes générales

- **Un seul fichier HTML**, ouvert en local (adresse `file:///…`). Pas de serveur, pas de backend, pas de fichier annexe.
- **Navigateurs** : Chrome et Edge, qui disposent de l'API File System Access. Si `window.showDirectoryPicker` n'existe pas (Firefox, Safari, Brave), la page affiche : « Ce navigateur ne gère pas l'accès aux dossiers. Ouvrez ce fichier dans Chrome ou Edge. »
- **Bibliothèques**, chargées depuis un CDN, en versions fixes :
  - JSZip 3.10.1 : `https://cdnjs.cloudflare.com/ajax/libs/jszip/3.10.1/jszip.min.js` (balise `<script>` classique) ;
  - Transformers.js 3.5.1 : `https://cdn.jsdelivr.net/npm/@huggingface/transformers@3.5.1` (import dynamique) ;
  - pdf.js 4.10.38 : `https://cdn.jsdelivr.net/npm/pdfjs-dist@4.10.38/build/pdf.min.mjs` et son worker `pdf.worker.min.mjs` (import dynamique, au premier PDF rencontré).
- **Aucun autre appel réseau** : pas de statistiques, pas de police externe, pas de feuille de style externe, aucun envoi de données.
- **Interface en français**, sobre : fond blanc, police système, colonne centrée d'environ 760 px, utilisable sur un petit écran.
- **Code commenté en français.**
- **L'application ne modifie, ne déplace, ne renomme et ne supprime jamais un fichier du volume.** Elle n'écrit que dans son propre dossier d'index (voir section 3).

## 2. Choisir le volume

Au lancement, la page affiche un statut (« Choisissez le volume à indexer. ») et une grande zone de dépôt :

> **Glissez ici le lecteur de votre conteneur**
> Depuis « Ce PC » dans l'Explorateur, faites glisser le lecteur (ex. G:) sur ce cadre.
> [Ou choisir un dossier…]

Pendant que l'utilisateur choisit, le modèle commence à se charger en arrière-plan.

**Glisser-déposer (méthode principale).** C'est le seul moyen d'obtenir la racine d'un lecteur dans Chrome : le sélecteur de dossier refuse les racines de lecteurs.
- Dans l'événement `drop`, appeler `e.dataTransfer.items[0].getAsFileSystemHandle()` **immédiatement, avant tout `await`** : sinon les données du glisser-déposer ne sont plus accessibles.
- Sous Windows, quand on dépose un lecteur, le nom de fichier transmis (`e.dataTransfer.files[0].name`) est de la forme `G_drive`. En extraire la lettre : c'est ainsi que l'application connaît l'emplacement du volume sans rien demander.
- Si l'objet déposé est un fichier et non un dossier : « Déposez un lecteur ou un dossier, pas un fichier. »

**Bouton « Ou choisir un dossier… »** : `showDirectoryPicker({ mode: "readwrite", id: "recherche-semantique" })`. Si l'utilisateur annule, ne rien afficher ; pour toute autre erreur, afficher « Dossier refusé : » suivi du message.

**Dossiers refusés par Chrome.** Par sécurité, Chrome refuse certains dossiers avec un message indiquant qu'ils « contiennent des fichiers système », même quand c'est faux : la racine des lecteurs, le dossier personnel, Documents, Téléchargements, Bureau et, sur Mac, la Bibliothèque. Il n'existe aucun réglage pour lever ce blocage. L'application ne doit pas chercher à le contourner ; le mode d'emploi à l'écran indique d'utiliser le glisser-déposer ou de choisir un sous-dossier.

**Droit d'écriture.** Après le choix, vérifier `queryPermission({ mode: "readwrite" })` et, puisqu'on est dans un geste de l'utilisateur, demander `requestPermission` si besoin. Si l'écriture est refusée (ou si le volume est monté en lecture seule) :
- l'index est construit et utilisable, mais seulement en mémoire, pour la session ;
- **rien n'est écrit ailleurs**, en particulier pas dans le stockage du navigateur ;
- le statut le signale (« index non enregistré : autorisation d'écriture refusée ») et un bouton « Autoriser l'enregistrement de l'index dans le volume » permet de réessayer.

**Après le choix**, la zone de dépôt disparaît au profit d'une ligne :
- « Volume : G: » (ou le nom du dossier) ;
- un champ « Emplacement », prérempli avec `G:\` quand la lettre est connue, sinon vide, avec l'exemple « G: ou /Volumes/… ». Sur Mac et Linux, la lettre n'existe pas : l'utilisateur tape une fois l'emplacement (`/Volumes/veracrypt1`, `/media/…`). Le navigateur le mémorise dans `localStorage`, sous une clé construite avec le nom du dossier choisi. **C'est la seule chose mémorisée par le navigateur** ;
- un lien « Changer de volume », qui recharge la page (avec confirmation si une indexation est en cours).

## 3. L'index

**Emplacement.** L'index est enregistré **dans le volume indexé lui-même**, dans un dossier `.recherche-semantique` créé à sa racine. Ainsi, il reste chiffré quand le conteneur est démonté, il suit le conteneur d'une machine à l'autre, et aucun nom de fichier professionnel n'est écrit sur le disque de l'ordinateur.

**Interdit** : stocker les chemins, les vecteurs ou le moindre texte des documents dans `localStorage`, IndexedDB, le système de fichiers privé du navigateur (OPFS) ou tout autre endroit hors du volume.

**Chemins relatifs.** Chaque fichier est identifié par son chemin relatif à la racine choisie, avec `/` comme séparateur (`Clients/Dupont/devis.docx`). Jamais de lettre de lecteur ni de chemin absolu dans l'index : c'est ce qui permet de monter le conteneur sur G: un jour et sur K: le lendemain sans rien réindexer. La lettre n'est ajoutée qu'au moment d'afficher les résultats.

**Contenu : deux fichiers.**
- `index.json` : `{ version, extractorVersion, dim, model, timestamp, paths, mtimes }`. `paths` est la liste des chemins relatifs, `mtimes` la date de modification de chaque fichier (`File.lastModified`) au moment de son indexation.
- `vectors.bin` : tous les vecteurs bout à bout, en `Float32`, 384 valeurs par fichier, dans le même ordre que `paths`.
- L'index ne contient **aucun texte extrait** des documents : seulement les chemins, les dates et les vecteurs.

**Écriture.** Écrire `vectors.bin` puis `index.json`, chacun avec `createWritable()` puis `close()` : Chrome écrit dans un fichier temporaire et ne remplace l'ancien qu'à la fermeture, ce qui évite un fichier à moitié écrit.

**Lecture et contrôle de cohérence.** Au chargement, vérifier que `version`, `extractorVersion` et `dim` correspondent aux constantes du code, et que le nombre de vecteurs est égal au nombre de chemins et de dates. Si quelque chose ne correspond pas (index absent, illisible, interrompu entre l'écriture des deux fichiers, ancien format), considérer qu'il n'y a pas d'index et tout réindexer, sans message d'erreur.

**Versions.** `INDEX_VERSION = 1`, `EXTRACTOR_VERSION = 1`. Augmenter `EXTRACTOR_VERSION` quand on change la façon d'extraire le texte force une réindexation complète.

## 4. Mise à jour différentielle

À chaque ouverture d'un volume :

1. Charger l'index existant. La recherche est utilisable dès ce moment, sur l'ancien index.
2. Parcourir tout le volume (voir section 5). Afficher « Analyse du volume… N fichiers trouvés », mis à jour tous les 500 fichiers.
3. Pour chaque fichier déjà indexé : s'il existe toujours et que sa date de modification est **identique** à celle enregistrée, garder son vecteur. Sinon, il sera réencodé. La comparaison se fait fichier par fichier, et non par rapport à la date globale de l'index : un fichier restauré dans une ancienne version est ainsi détecté.
4. Encoder les fichiers nouveaux ou modifiés. Retirer de l'index les fichiers disparus.
5. Si rien n'a changé, réenregistrer simplement l'index (nouvel horodatage) et afficher « Index chargé — N fichiers indexés ».

**Pendant l'encodage :**
- traiter les fichiers par lots de 32 ;
- après chaque lot, mettre à jour l'index en mémoire, pour que la recherche porte déjà sur les fichiers traités ;
- afficher « Indexation en cours… 736 / 18 956 fichiers (≈ 203 / min) », nombres au format français ;
- **sauvegarde intermédiaire tous les 1 500 fichiers encodés**, puis une sauvegarde finale. Une interruption (onglet fermé, mise en veille, conteneur démonté) ne fait perdre que les fichiers traités depuis la dernière sauvegarde ; la fois suivante, la mise à jour reprend là où elle s'était arrêtée ;
- tant qu'une indexation est en cours, la fermeture de l'onglet déclenche l'avertissement standard du navigateur (`beforeunload`).

**Bouton « Réindexer tout »**, discret, sous le champ de recherche : après confirmation (« Relire tous les fichiers du volume ? Cela peut prendre un moment. »), il ignore l'index existant et réencode tout. Il est désactivé pendant une indexation. Une seule indexation à la fois.

**Volume démonté pendant l'indexation.** Les lectures et écritures échouent. Arrêter proprement l'indexation et afficher un message clair, par exemple « Le volume n'est plus accessible. L'index enregistré à la dernière sauvegarde reste valable. ». Ne jamais tenter d'écrire l'index ailleurs.

## 5. Parcours du volume

- Parcourir récursivement le dossier choisi.
- Ignorer les fichiers et dossiers dont le nom commence par `.` (ce qui exclut aussi `.recherche-semantique`) et les fichiers temporaires d'Office, qui commencent par `~$`.
- Ignorer ces dossiers (comparaison sans tenir compte des majuscules) : `$recycle.bin`, `system volume information`, `windows`, `program files`, `program files (x86)`, `programdata`, `appdata`, `node_modules`, `__pycache__`, `.git`, `.svn`, `.venv`, `venv`, `.cache`.
- Ignorer ces extensions : `.tmp`, `.temp`, `.lnk`, `.ini`, `.dll`, `.sys`, `.exe`, `.msi`, `.pyc`, `.pyo`, `.lock`, `.ds_store`, `.db`, `.dat`, `.crswap` (fichiers temporaires de Chrome).
- Tous les autres fichiers sont indexés, au minimum par leur nom et leur dossier.
- Un fichier ou un dossier illisible (droits insuffisants, fichier verrouillé, dossier système) est ignoré **silencieusement** : il ne doit jamais interrompre le parcours.

## 6. Extraction du texte

Pour chaque fichier, on extrait **au plus 500 caractères** de contenu, espaces normalisés. **Règle absolue : toute erreur d'extraction (fichier corrompu, protégé par mot de passe, format inattendu) donne un contenu vide, sans message, et le fichier est quand même indexé par son nom et son chemin.**

| Format | Méthode | Contenu extrait |
|---|---|---|
| `.docx` | JSZip, lecture de `word/document.xml` ; fin de paragraphe et tabulation remplacées par un espace, balises retirées, entités XML décodées | Début du corps, tableaux compris |
| `.xlsx` | JSZip : noms des feuilles dans `xl/workbook.xml`, chaînes partagées dans `xl/sharedStrings.xml`, cellules de `xl/worksheets/sheet1.xml` (sinon la première feuille trouvée), y compris les chaînes « inline » | `Feuilles : Budget, Notes \| ` suivi des 500 premiers caractères de la première feuille |
| `.pdf` | pdf.js, texte de la **page 1** uniquement, avec `isEvalSupported: false` | Début de la page 1 ; un PDF scanné donne un contenu vide (pas d'OCR) |
| `.txt`, `.md`, `.csv`, `.log` | Lecture des 2 000 premiers octets ; décodage UTF-8 strict, sinon Windows-1252 | 500 premiers caractères |
| `.eml` | Analyse du message (voir ci-dessous) | En-têtes + début du corps |
| Tous les autres | Aucune lecture | Nom et chemin seulement |

- Au-delà de **100 Mo**, les `.docx`, `.xlsx` et `.pdf` ne sont pas ouverts : nom et chemin seulement.
- **pdf.js depuis une page locale** : le navigateur refuse de lancer un worker hébergé sur un autre domaine. Télécharger le code du worker avec `fetch`, en faire un `Blob`, et passer son URL (`URL.createObjectURL`) à `GlobalWorkerOptions.workerSrc`. Fermer chaque document avec `destroy()`.

**E-mails `.eml`** (sans bibliothèque) :
- en-têtes `Subject`, `From`, `To`, `Date`, avec les lignes de continuation et les mots encodés (`=?charset?B?…?=` et `=?charset?Q?…?=`, y compris plusieurs mots encodés consécutifs) ; n'afficher que les en-têtes présents, sous la forme `Objet : … | De : … | À : … | Date : …`, limités à 400 caractères ;
- corps : parcourir les parties `multipart/*` (profondeur maximale 8), ignorer les pièces jointes (`Content-Disposition: attachment`), décoder `base64` et `quoted-printable` selon le `charset` déclaré ; prendre la partie `text/plain`, sinon la partie `text/html` débarrassée de `script`, `style`, `head` et des balises ;
- au-delà de **25 Mo** (grosses pièces jointes), ne lire que les 64 premiers Ko et seulement les en-têtes.

**Texte encodé pour chaque fichier** (une seule ligne) :

```
passage: <nom lisible> (<extension>) | <dossier relatif> | <contenu extrait>
```

- Le nom lisible est le nom du fichier sans extension, où `_`, `-` et `.` sont remplacés par des espaces : `template_v3_final` devient `template v3 final`. Cela aide beaucoup le modèle.
- Le dossier relatif est affiché avec des `\`.
- Le préfixe `passage: ` est **obligatoire** : le modèle E5 a été entraîné ainsi.

## 7. Modèle et recherche

**Modèle** : `Xenova/multilingual-e5-small` (version ONNX de `intfloat/multilingual-e5-small`), 384 dimensions, multilingue, qui comprend nativement le français.
- Le charger avec Transformers.js : `pipeline("feature-extraction", MODEL, …)`, avec `env.allowLocalModels = false`.
- Si `navigator.gpu` existe, essayer d'abord la carte graphique (`device: "webgpu"`, `dtype: "fp32"`) et vérifier qu'un encodage d'essai fonctionne ; en cas d'échec, utiliser le processeur avec `dtype: "q8"` (version compressée, environ 120 Mo).
- Encodage : `{ pooling: "mean", normalize: true }`. Les vecteurs étant normalisés, le produit scalaire égale la similarité cosinus.
- Un seul chargement du modèle, même si plusieurs parties de la page le demandent en même temps.
- **Un seul encodage à la fois** (file d'attente) : l'indexation et la recherche partagent le modèle.

**Recherche** :
- la requête est préfixée par `query: ` (**obligatoire**, comme `passage: `) ;
- comparaison exhaustive avec tous les vecteurs de l'index, par produit scalaire, en gardant les 5 meilleurs. Pas besoin de bibliothèque d'index vectoriel : pour quelques dizaines de milliers de fichiers, c'est l'affaire de quelques millisecondes ;
- si le modèle n'est pas encore chargé : « Le modèle est encore en cours de chargement. » ;
- si aucun résultat : « Aucun fichier trouvé. Vérifiez que l'indexation est terminée ou reformulez la requête. »

## 8. Résultats et ouverture des fichiers

Chaque résultat est une carte avec :
- le **nom du fichier**, cliquable ;
- le **score** en pourcentage (arrondi), à droite ;
- le **chemin complet** en petit et en gris : emplacement de la session + chemin relatif ;
- un bouton « Copier le chemin » (qui affiche brièvement « Copié »).

Tout texte issu des fichiers ou de leurs noms est **échappé** avant d'être inséré dans la page.

**Chemin complet.**
- Windows : emplacement `G`, `G:` ou `G:\` donne `G:\` ; un emplacement `G:\Dossiers` est accepté ; les `/` du chemin relatif deviennent des `\`.
- Mac et Linux : un emplacement commençant par `/` est utilisé tel quel, avec des `/`.

**Liens.** Un navigateur ne peut pas lancer une application lui-même ; deux mécanismes le permettent :
- **Fichiers Office** (`.doc`, `.docx`, `.xls`, `.xlsx`, `.ppt`, `.pptx`) : protocoles Office, qui ouvrent le **fichier d'origine** (et non une copie téléchargée) après une confirmation, par exemple `ms-word:ofe|u|file:///G:/Clients/Dupont/devis%20final.docx`. Utiliser `ms-word`, `ms-excel` ou `ms-powerpoint` selon l'extension.
- **Autres fichiers** : lien `file:///G:/…` (Windows) ou `file:///Volumes/…` (Mac, Linux), ouvert dans un nouvel onglet. Selon le type de fichier, le navigateur l'affiche (PDF) ou le télécharge.
- Encoder chaque segment du chemin avec `encodeURIComponent`, sauf la lettre de lecteur (`G:`).
- Si l'emplacement est vide, les noms ne sont pas cliquables et un message invite à le renseigner (« ex. G: ou /Volumes/veracrypt1 »). Le bouton « Copier le chemin » reste la solution de secours dans tous les cas.

## 9. Interface

De haut en bas :
- le titre « Recherche sémantique » ;
- une ligne de statut avec une pastille : verte (prêt), orange clignotante (travail en cours, sans animation si l'utilisateur a demandé à réduire les animations), rouge (erreur), grise (en attente). Messages : « Choisissez le volume à indexer. », « Chargement du modèle (téléchargement au premier usage)… », « Lecture de l'index… », « Analyse du volume… », « Indexation en cours… », « Index chargé — 18 956 fichiers indexés » ;
- la zone de dépôt, puis, une fois le volume choisi, la ligne du volume, un grand champ de recherche centré (exemple : « Ex. : courrier de relance au client Dupont ») avec un bouton « Rechercher », le lien « Réindexer tout », et les résultats.

## 10. Durée à annoncer honnêtement

- Une page ouverte en local ne peut pas utiliser plusieurs cœurs du processeur pour le modèle (le navigateur réserve ce mode aux pages servies avec des en-têtes de sécurité particuliers). L'indexation tourne donc sur un seul cœur, sauf si la carte graphique prend le relais.
- Mesure réelle sur un ordinateur ancien, sans carte graphique utilisée : environ **200 fichiers par minute**, soit **environ 1 h 30 pour 19 000 fichiers** à la première indexation.
- Les ouvertures suivantes ne réencodent que les fichiers nouveaux ou modifiés ; le parcours du volume prend de quelques secondes à quelques minutes sur un disque externe.
- La recherche elle-même prend moins de 2 secondes.
- Ne promets dans l'interface aucune durée d'indexation ; affiche seulement le débit mesuré.

## 11. Ce qu'il ne faut pas faire

- Pas de serveur local, pas de Python, pas d'extension de navigateur.
- Pas d'OCR, pas de grand modèle de langage, pas d'API en ligne.
- Pas d'index stocké dans le navigateur, pas de chemin absolu dans l'index.
- Pas de tentative de contourner les dossiers bloqués par Chrome.
- Pas d'écriture dans le volume en dehors de `.recherche-semantique/`.
- Ne pas casser l'indexation entière à cause d'un seul fichier.

## 12. Livrable

Donne-moi **le fichier complet**, `recherche-semantique.html`, d'un seul tenant, sans troncature ni « … reste du code inchangé ». Puis, en quelques lignes, la liste des vérifications à faire :

1. Ouvrir le fichier dans Chrome : le statut indique « Choisissez le volume à indexer. ».
2. Glisser un **petit dossier de fichiers fictifs** (une dizaine de `.docx`, `.pdf`, `.xlsx`, `.txt` et `.eml`), dont un fichier volontairement corrompu : l'indexation va au bout.
3. Vérifier qu'un dossier `.recherche-semantique` contenant `index.json` et `vectors.bin` est apparu dans le dossier choisi.
4. Chercher « modèle de courrier » : un fichier nommé par exemple `lettre_type.docx` doit sortir en tête.
5. Cliquer sur un résultat Word : Word ouvre le fichier d'origine (vérifier son chemin dans Fichier > Informations).
6. Fermer l'onglet, rouvrir le fichier HTML, redéposer le dossier : l'index se recharge et rien n'est réencodé.
7. Modifier un fichier, en supprimer un autre, rouvrir : un seul fichier est réencodé, et le fichier supprimé n'apparaît plus.
````

</details>

## Sécurité et secret professionnel

**Rien n'est envoyé.** Vos documents sont lus et analysés dans le navigateur, sur votre ordinateur. Aucun fichier, aucun extrait, aucun nom de fichier, aucune recherche n'est transmis à un service en ligne.

**Ce qui est téléchargé, et d'où.** L'outil télécharge uniquement des outils, jamais vos données :

| Quoi | D'où | Quand |
|---|---|---|
| Le modèle de recherche multilingual-e5-small (environ 120 Mo ; environ 470 Mo si votre carte graphique est utilisée) | huggingface.co, dépôt `Xenova/multilingual-e5-small` | Au premier usage, puis gardé dans le cache du navigateur |
| Transformers.js, qui fait fonctionner le modèle | cdn.jsdelivr.net | À l'ouverture de la page, ou depuis le cache |
| pdf.js, qui lit les PDF | cdn.jsdelivr.net | Au premier PDF rencontré, ou depuis le cache |
| JSZip, qui lit les fichiers Word et Excel | cdnjs.cloudflare.com | À l'ouverture de la page, ou depuis le cache |

Ces téléchargements révèlent à ces serveurs, comme toute visite d'une page web, votre adresse IP et le fait que vous utilisez ces outils. Ils ne révèlent rien de vos documents. Si le navigateur vide son cache, le modèle est simplement retéléchargé.

**Ce qui est écrit, et où.**
- Dans le conteneur : un dossier caché `.recherche-semantique`, qui contient l'index (`index.json` et `vectors.bin`). Il contient la liste des chemins de vos fichiers, leurs dates de modification et des vecteurs numériques, mais aucun texte extrait de vos documents. Traitez-le comme vos documents eux-mêmes : les chemins de fichiers peuvent déjà en dire long, et des travaux de recherche montrent qu'on peut parfois reconstituer une partie d'un texte à partir de ses vecteurs.
- Dans le navigateur : seulement l'emplacement que vous tapez, si vous en tapez un (par exemple `/Volumes/veracrypt1` sur Mac), pour ne pas avoir à le ressaisir.
- **Vos fichiers ne sont jamais modifiés, déplacés ni supprimés.** L'outil les lit, sans plus. Seule l'application que vous ouvrez d'un clic (Word, par exemple) peut les modifier, comme si vous les aviez ouverts depuis l'Explorateur.

## Réserve importante : le disque C: et les dossiers système

L'outil ne peut pas indexer le disque C: en entier. Pour protéger le système, Chrome et Edge refusent d'ouvrir les dossiers qui contiennent, ou peuvent contenir, des fichiers système : la racine de C:, mais aussi votre dossier personnel (`C:\Users\VotreNom`), Documents, Téléchargements et Bureau. Ce blocage vient du navigateur, pas de l'outil, et aucun réglage ne permet de le lever.

En revanche, un dossier ordinaire placé à l'intérieur de ces emplacements est accepté. Si vous voulez retrouver des documents rangés sur C:, regroupez-les donc d'abord dans un dossier dédié, puis indexez ce dossier :

- créez par exemple `C:\Dossiers` ou `Documents\Dossiers clients` ;
- déplacez-y les dossiers de travail que vous voulez retrouver ;
- choisissez ce dossier dans l'outil, avec le bouton « Ou choisir un dossier… ».

Ne déplacez pas les dossiers du système ni ceux des logiciels (`Windows`, `Program Files`, `AppData`…) : ils ne contiennent pas vos documents, et les déplacer empêcherait l'ordinateur ou vos logiciels de fonctionner.

Attention : dans ce cas, l'index est enregistré dans ce dossier, sur le disque de l'ordinateur, et la liste de vos noms de fichiers s'y trouve donc en clair. Pour des dossiers couverts par le secret, préférez un conteneur chiffré, pour lequel l'outil a été conçu.

## Limites connues

- **Navigateurs.** L'outil a été testé avec Chrome sous Windows. Il nécessite une fonction que seuls Chrome, Edge et les navigateurs de la même famille proposent : il ne fonctionne pas avec Firefox ni Safari, et Brave la bloque par défaut.
- **Dossiers refusés par Chrome.** Par sécurité, Chrome refuse d'ouvrir certains dossiers avec le bouton « Choisir un dossier », en indiquant qu'ils contiennent des fichiers système, même quand c'est faux : la racine des lecteurs, votre dossier personnel, Documents, Téléchargements, Bureau. Aucun réglage ne permet de lever ce blocage. Faites glisser le lecteur sur la page, ou choisissez un sous-dossier.
- **Sur Mac et Linux,** l'outil ne connaît pas l'emplacement du volume : vous le tapez une fois (par exemple `/Volumes/veracrypt1`) pour que les résultats soient cliquables. Chrome sur Mac refuse davantage de dossiers que sous Windows.
- **Durée de la première indexation.** Dans un navigateur, l'outil n'utilise qu'un cœur du processeur. Sur un ordinateur ancien, comptez environ 200 fichiers par minute, soit environ deux heures pour un conteneur de 19 000 fichiers. Désactivez la mise en veille pendant ce temps. Une interruption ne fait pas tout perdre : l'index est enregistré tous les 1 500 fichiers.
- **Ne démontez pas le conteneur pendant l'indexation.** Si cela arrive, l'index enregistré à la dernière sauvegarde reste valable, et l'indexation reprend à l'ouverture suivante.
- **Conteneur en lecture seule.** L'outil fonctionne, mais l'index ne peut pas être enregistré : il est refait à chaque ouverture.
- **Ouverture des fichiers.** Les fichiers Word, Excel et PowerPoint s'ouvrent directement dans leur application, après une confirmation. Les autres, comme les PDF, s'ouvrent dans le navigateur ou sont téléchargés, selon votre configuration. Le bouton « Copier le chemin » permet toujours de les retrouver dans l'Explorateur.
- **Ce que l'outil lit.** Seuls les 500 premiers caractères de chaque document sont pris en compte (la première page d'un PDF). Les PDF scannés ne sont pas lus, faute de reconnaissance de caractères : ils sont retrouvés par leur nom seulement.
- **Recherche par le sens, pas par le mot exact.** Pour un numéro de dossier, une référence ou un nom propre rare, la recherche classique de votre système reste plus fiable.

## Précautions

- **Testez d'abord sur des fichiers fictifs**, dans un dossier de test, avant de l'utiliser sur un conteneur réel.
- **Relisez le code produit,** ou faites-le relire par une personne compétente, avant de l'utiliser sur des dossiers clients. C'est un fichier texte : vous pouvez notamment vérifier qu'il ne contacte pas d'autres adresses que celles du tableau ci-dessus.
- **L'IA peut se tromper.** Chaque assistant fabrique sa propre version de l'outil à partir de la recette. Le résultat peut varier d'un assistant à l'autre et d'une fois à l'autre, et contenir des erreurs.
- **Gardez une sauvegarde** de votre conteneur, indépendamment de cet outil.
- **L'outil est une recette, pas un logiciel garanti.** Il est fourni tel quel, sans aucune garantie de fonctionnement, de résultat ou de sécurité.

## Vos retours

Un bug, une amélioration, un piège rencontré ? Ouvrez une « Issue » dans l'onglet du même nom, en haut de cette page. Chaque piège signalé enrichit la recette pour tout le monde.

## Licence

Ce texte et la recette sont publiés sous [licence MIT](LICENSE) : vous pouvez les utiliser, les modifier et les redistribuer librement, à condition de conserver la mention de copyright et le texte de la licence.
