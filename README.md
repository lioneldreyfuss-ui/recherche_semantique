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

1. **Copiez le prompt.** Ouvrez [PROMPT.md](PROMPT.md), puis cliquez sur l'icône « Copy raw file », en haut à droite du texte.
2. **Donnez-le à un assistant d'IA.** Collez-le dans une nouvelle conversation et envoyez-le. Le prompt ne contient aucune donnée vous concernant : vous ne transmettez rien de confidentiel à cette étape.
3. **Enregistrez le fichier obtenu.** L'assistant vous rend un fichier `recherche-semantique.html`. Enregistrez-le, de préférence dans votre conteneur chiffré.
4. **Ouvrez-le dans Chrome ou Edge.** Un double-clic suffit, si l'un de ces navigateurs est votre navigateur par défaut.
5. **Choisissez le volume.** Montez votre conteneur, puis faites glisser son lecteur, depuis « Ce PC » dans l'Explorateur, sur le cadre de la page. Autorisez l'enregistrement quand le navigateur le demande.
6. **Laissez indexer, puis cherchez.** Le compteur indique l'avancement. La recherche fonctionne déjà pendant l'indexation, sur les fichiers déjà traités.

**La première indexation est longue, et c'est normal.** Pour un conteneur entier, comptez environ deux heures : c'est la durée mesurée sur un volume de près de 19 000 fichiers, avec un ordinateur ancien. Elle varie selon le nombre de fichiers, la vitesse du disque et la puissance de la machine. L'outil lit chaque document et le fait analyser par le modèle, sur un seul cœur du processeur. Ce temps ne se paie qu'une fois : les ouvertures suivantes ne traitent que les fichiers nouveaux ou modifiés, et ne prennent que quelques minutes au plus. Pendant la première indexation, laissez l'onglet ouvert et désactivez la mise en veille de l'ordinateur.

Les fois suivantes, il suffit de monter le conteneur, d'ouvrir le fichier HTML et de redéposer le lecteur.

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
