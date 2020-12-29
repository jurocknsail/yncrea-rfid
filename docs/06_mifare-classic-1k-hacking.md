# TP - MIFARE Classic 1K Hacking

---  

## Prise en main (ACR122u)

Le ACR122u est un lecteur graveur de tag RFID, le seul respectant tous les standards actuellement en vigueur.  
Nous allons l'utiliser pour lire et écrire des blocks de données sur une carte dont **nous connaissons la Key A**.  

Ce lecteur vient avec son propre SDK et coute entre 25€ et 30€.

![acr122u](./assets/images/rfid/acr122u.png "acr122u")
  
### ACR122u Tool - UID

1. Clicker sur le bouton de connection, la led du lecteur est rouge
1. Poser la carte sur le lecteur, la led devient verte
1. Clicker sur Connecter
1. ``Reader Commands > Get Data`` permet de récupérer l'UID de la carte

### ACR122u Tool - ATR

Comme sur une smart card, il nous faut effectuer l'ATR pour communiquer avec la carte. Cela nous donnera l'accès aux commandes cartes.  

1. ``Reader Commands > Get ATR``
1. On peut maintenant Charger les Clés dans le lecteur : ``Load Authentication Keys > FF FF FF FF FF FF`` (Default Transport Configuration pour toute nouvelle carte)
1. Puis on peut ``Authenticate`` sur le block de notre choix : **Block 0**
1. On peut enfin ``Read`` le Block 0 en Hexa et retrouver notre UID !
1. On peut lire les Blocks 1 et 2, ils contiennent que des 0.
1. On peut aussi lire le Block 3 qui est le sector trailer du secteur 0.

### ACR122u Tool - Read / Write Data Block in Sector 1

1. Authenticate sur Block 4
1. Write & Read le Block 4 (Peut être décodé en ASCII)
1. Authenticate sur Block 5
1. Write & Read le Block 5 (Peut être décodé en ASCII)

!!! tip
    Le numéro du block doit être mis en Hexa. Si on veut le block 11, il faut mettre ``0x0B``.

### ACR122u Tool - Read / Write Sector Trailer

1. Authenticate sur Block 3
1. Essayer de Read le block 3.

!!! tip
    On remarque que la clé A ne peut pas être lue, on récupere ``00 00 00 00 00 00``  
    Et on ne pourra jamais la lire sur une Mifare, il faut obligatoirement que le lecteur la connaisse au préalable !
    
!!! tip
    Pour modifier les clés, il faut utiliser un autre outil "ACS MiFare Key Management Tool".
    
1. Utiliser le Key Management Tool pour changer la clé A
1. S'authentifier avec la Clé A : ``FF FF FF FF FF FF`` sur le **Sector 1**
1. Updater la Clé A en  : ``FF FF FF FF AA AA``, puis cliquer sur ``Format Sector``

!!! tip
    Le bloc **3** est updaté automatiquement car c'est le Sector Trailer !
    
1. S'authentifier en utilisant la nouvelle clé sur le secteur 1
1. Modifier la clé B en ; ``FF FF FF FF BB BB``

!!! tip
    On pourrais aussi modifier les Access Bits ...

1. Verifier ces changements en essayant de lire les blocks 5 ou 6 avec les anciennes clés. Cela devrait échouer. Block 8 devrait fonctionner car il appartient au secteur 2 que nous n'avons pas modifié.
1. Charger les nouvelles clés. Verifier que l'on peut maintenant lire les Blocks 4 & 5.

---

## Hack de Clés (Proxmark3)

TODO