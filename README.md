# lettrator
Détecteur de courrier automatique

# Description
Le but de ce dispositif est de détecter l'ouverture d'un boîte aux lettres et d'en informer le propriétaire.
Pour l'instant il est prévu deux modules : l'émetteur à installer dans la boîte et le récepteur dans l'habitation.
D'un point de vue technique les choix suivant ont été réalisés :
- utilisation de deux arduino nano (émetteur et base) : https://fr.aliexpress.com/item/1005001711279154.html
- détection de l'ouverture de la porte via un émetteur récepteur infrarouge : https://fr.aliexpress.com/item/1005001289042583.html
- alimentation de l'émetteur via un bloc de piles AAA : https://fr.aliexpress.com/item/4001075406472.html
- communication entre les deux éléments en 2,4 Ghz : https://fr.aliexpress.com/item/2042534967.html
- led ?

# Cas d'utilisation
## Cas métier
### CM01 : En tant qu'utilisateur je veux savoir si j'ai reçu du courrier
Le passage d'une lettre ou d'un petit colis devant le détecteur doit être détecté par l'émetteur soit la forme d'une modification du signal de courte durée (<100ms)

### CM02 : En tant qu'utilisateur je veux savoir si j'ai reçu un colis
Le dépôt d'un colis dans la boîte devant le détecteur doit être détecté par l'émetteur sous la forme d'une modifcation longue du signal de détection (>1 min)

### CM03 : En tant qu'utilisateur je veux savoir si la porte de ma boîte aux lettres a été ouverte
L'ouverture de la boîte doit être détectée par l'émetteur sous la forme d'une modification temporaire du signal (> lettre et < à colis)

### CM04 : En tant qu'utilisateur je dois pouvoir signaler que je suis allé relever ma boîte
Quand j'ai vérifier ma boîte, où en cas de faux positifs / dysfonctionnement il faut réinitaliser le système de détection c'est à dire :
- considérer l'état courant du détecteur comme référence au niveau de l'émetteur
- éteindre la ou les notifications sur la base
Si le bouton est sur l'émetteur => il faut laisser quelques secondes pour refermer la boîte
Si le bouton est sur le réception => il faut quand même prévoir de réinit l'émetteur 

## Cas techniques
### CT01 : En tant qu'utilisateur je veux savoir si ma base est alimentée
Une led verte alumée sur la base en cas d'alimentation

### CT02 : En tant qu'utilisateur je veux savoir si la liason entre la base et l'émetteur fonctionne correctement
Une led éteinte si jamais de réception de l'émetteur
Une led alummée sur la base si réception émetteur < 1h (heartbeat)
La led clignote si la réception est datée de plus.

### CT03 : En tant qu'utilisateur je veux savoir si les piles de l'émetteur sont à changer
=> la connexionb est faite (CT02) allummée
=> clignotement de la led d'alimentation

### CT04 : En tant qu'utilisateur je ne veux pas avoir à changer les piles trop souvent (3 mois ?)
Calculer la consommation de l'émetteur en fonction : 
- du cycle de détection 
- du cycle d'émission du heartbeat
- de la complexité des calculs côté émetteur

## Questions à traiter
### Positionnement de l'émetteur
Au fond de la boîte mais comme certaines boîtes s'ouvrent des deux cotés, prévoir de l'adhésif + un système de rails pour pouvoir changer les piles

### Nécessité d'un apparaige entre base et émetteur
Risque si plusieurs émetteurs en 2,4 Ghz à proximité (faux positifs)

### Qui connait l'état ?
#### Option 1 : L'émetteur
- : consommation supplémentaire pour faire les calculs
+ : si la base se déconnecte, pas besoin de réinit (procédure à définir)

#### Option 2 : La base
+ : il suffit de couper l'alim pour réinitialiser
+ : pas de calcul côté émetteur : on envoi simplement les infos, la base fait le reste
- : il faut attendre un ping de l'émetteur pour pouvoir initialiser (long ou si plsu court hausse de la consomation)
- : si la première mesure reçue est mauvaise (un colis, porte ouverte, ...) : tout sera faussé

### Réinit de l'émetteur
1 Enlever les piles => risque de ne pas être pratique car surement dans le boitier
2 Bouton reset sur le côté du boitier => risque sur l'étanchéité

### Risque sur l'alimentation
Les 4,5V ne devraient pas être suffisant :(  => à confirmer

### Risque sur l'humidité
Réduire au maximum les ouvertures et donc les boutons. Voir même si pas possible de faire la détection au travers de plastique transparent

# Conception électronique
## Références
https://randomnerdtutorials.com/nrf24l01-2-4ghz-rf-transceiver-module-with-arduino/
http://idehack.com/blog/alimenter-larduino-nano/
https://forum.arduino.cc/index.php?topic=15629.0
https://arduino.stackexchange.com/questions/50769/how-to-measure-the-supply-voltage-of-arduino-without-using-an-analog-pin

# Conception code
## Références
https://randomnerdtutorials.com/nrf24l01-2-4ghz-rf-transceiver-module-with-arduino/

# Conception 3D

