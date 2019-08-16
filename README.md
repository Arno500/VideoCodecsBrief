# Brief des codecs

J'ai inclus une liste de codecs vidéo, audio, et conteneurs.
**Ne pas confondre les codecs avec les conteneurs !**  
En effet une piste vidéo est toujours encapsulée dans un conteneur, le plus souvent avec une piste audio. Le "format" MP4 n'existe pas, cela n'a pas de sens. On peut dire :
> "Le format HEVC, dans un conteneur MP4"

par exemple. Ou à la limite, on peut partir du principe que notre lecteur lira n'importe quel conteneur, donc simplement préciser le codec. 

## Liste des formats du plus ancien au plus récent :

- ### MPEG 2 :
     Format des DVD, pour une qualité acceptable il faut un débit ÉNORME (un DVD fait 4.7Go pour 1h30, ça donne une idée) -> obsolète aujourd'hui,

- ### DivX/XviD :
    Une implémentation de la norme MPEG 4. Bien plus efficace, on a une diminution de taille d'environ 50% pour une qualité équivalente (ie 2Go pour 1h30, mais les pirates avaient l'habitude de viser 700Mo pour que ça rentre sur un CD 😋) -> obsolète aussi,

- ### H264 (ou MPEG 4 - AVC, ou AVC) :
    Un codec standard, compatible tous navigateurs, toutes plateformes (**ATTENTION** : la vidéo n'est pas accélérée sur Chrome sur Linux, à cause de la flemmardise des dev de Google), accéléré sur pratiquement tous les périphériques, supporté à partir d'IE 9,

- ### VP8 :
    Le format de Google, Open-source, efficacité équivalente ou supérieur au H264, accéléré sur la plupart des périphériques (mais un poil moins que le H264), seulement supporté à partir de MacOS 10.14.4 et iOS 12.2 (mais pas de problèmes ailleurs),

- ### HEVC :
    Un format complètement fermé (les constructeurs payent des royalties pour les intégrer à leurs appareils, les navigateurs aussi, les décodeurs ne sont pas publiques), environ 50% plus efficace que le H264 pour une qualité équivalente, de plus en plus efficace au fur et à mesure que la résolution augmente. Ie : Pour la 4K, c'est top.  
    Particularité : contrairement au H264 10bits assez peu répandu (et pas accéléré dans 99% des cas), le HEVC supporte le 10 et 12 bits, qui permettent d'avoir une précision des couleurs supérieur. Ça paraît idiot comme ça, mais les dégradés sont beaucoup plus smooth, et on peut appliquer un filtre de dithering (Handbrake gère tout seul), ce qui fait finalement gagner encore de l'espace (on parle de 1 à 20%). Ce format doit être privilégié pour la production vidéo, à la fin de la chaîne. C'est aussi le format qu'utilise Netflix pour les appareils compatibles (et Netflix ils sont super calés à ce niveau là).
    Seulement compatible avec IE 11 et Edge 18 (pas sur les prochaines version sous Chromium) sur les appareils qui le supportent via le matériel, et Safari sur High Sierra+ ou iOS 11+. 

- ### VP9 :  
    Equivalent du HEVC, en version open-source, il est un petit poil plus efficace, et pratiquement aussi compatible. Pas supporté du tout sur les appareils Apple (merci Apple encore).

- ### AV1 :
    Format totalement open-source, formé pour contrer le MPEG venant d'un consortium privé. Il vient de l'Alliance for Open Media, fondée par Amazon, Cisco, Google, Intel, Microsoft, Mozilla et Netflix, VideoLan (association derrière VLC), Apple et Samsung (notamment, la liste est longue). Le codec AV1 est censé est rapide, pratique, peu consommateur, et le plus complet possible. Il n'est accéléré sur à peu près aucun appareil. Donc il est décodé via logiciel, ce qui demande de grosses performances, vide la batterie sur un appareil portable. Inutilisable aujourd'hui, mais à surveiller pour dans quelques années.


## Les formats audio :
- ### MP3 : 
    C'est vieux, c'est relativement efficace, mais c'est obsolète. On est sur du 10Mo environ pour une chanson de 3min30 en qualité acceptable. Le brevet a récemment expiré, mais ce format est globalement mort.
- ### OGG Vorbis :
    Une qualité légèrement meilleure pour une taille légèrement inférieur, ce codec est open-source. Semi-obsolète.
- ### AAC :
    Le successeur officiel du MP3. Bien plus efficace que les deux précédent, mais format "fermé". On peut attendre environ 6Mo pour une chanson en qualité acceptable. Supporté partout à partir d'IE 9.
- ### Opus :
    Format ouvert qui détruit tous les autres. Il est utilisé par les plateforme de discussion de jeu pour sa qualité et sa faible latence comme Discord, Mumble ou TeamSpeak. Il utilise un algo maison orienté musique, et l'algo de Skype piur la voix, en switchant automatiquement entre les deux. Il supporte le switch de débit en temps réel. On a une musique en qualité équivalente à ceux du dessus pour une taile d'environ 3.5 Mo. Démo : https://opus-codec.org/examples/  
    Supporté sur tous les navigateurs, sauf IE, et Safari avant High Sierra et iOS 11 (+ nécéssite le conteneur .caf). Encore merci Apple !

## Les conteneurs :

- ### MKV (Matroska) :
    J'en parle parce que c'est cool, mais c'est pas supporté par les navigateurs. Il supporte un nombre théoriquement illimité de pistes vidéo, audio, sous-titres, et des métadonnées, souvent utilisées pour les chapitres. On peut intégrer n'importe quel format de vidéo/son/sous-titre dedans. Il est totalement open-source.
- ### WebM :
    Là c'est intéressant ! Il découle du MKV, donc il a à peu près les mêmes caractéristiques. Il supporte seulement le VP8-9 en vidéo (et l'AV1 du coup), + l'OGG Vorbis et l'Opus en audio. Il peut intégrer les sous-titres directement dans le format (facilitant la distribution).
- ### MP4 :
    Standard, supporté partout à partir d'IE 9. Supporte le HEVC, le H264, le AAC et le MP3 (les formats fermés, tiens).  
  
Quelque soit le conteneur, les sous-titres sont au format WebVTT (facilement compatible depuis l'ultra-standard SRT), à intégrer dans les Tracks HTML5.

# Résumé

Pour viser de la haute qualité avec des navigateurs modernes : VP9 + Opus en WebM, avec un fallback en H264 et son en MP3.  
Et l'habit de fait pas le moine, puisqu'un MP4 peut contenir tellement de choses (pareil pour le WebM, MKV, AVI, et d'autres) ! 

# Note d'encodage

Il faut privilégier l'encodage avec un débit variable : on peut spécifier une qualité cible, et cela permet d'avoir une qualité constante, avec un débit plus faible pour les scènes légères. Je recommande une qualité d'environ 23 en H264 et HEVC, et je ne sais plus pour le VP9, faut tâtonner.  
Il faut aussi utiliser les flags pour FFMPEG en MP4 (Handbrake a une case Optimiser pour le Web) en utilisant : `-movflags faststart`. En fait, ça dit à l'encapsuleur de mettre les données dans un certain ordre, permettant au navigateur de télécharger les données qu'il faut plus rapidement (un peu comme le JPEG progressif).
