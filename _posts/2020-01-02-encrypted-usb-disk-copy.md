---
title: Copier une clé USB chiffrée
layout: post
---

Pour copier une clé USB chiffrée deux approches sont possibles :
1. cloner la clé avec la commande `dd` ou mieux `pv`
2. copier le contenu de la clé

## Cloner la clé

L'avantage de cette solution est que la copie de la clé peut être fait dans un environnement non sécurisé, la clé à 
copier n'étant pas montée sur le système.  
Par contre l'inconvénient est que toute la clé va être dupliquée, même si il n'y a que quelques fichiers à copier, 
rendant l'opération plus longue.

La commande `dd` peut être utilisée (par exemple `sudo dd if=/dev/sdb of=/dev/sdc bs=64k`) mais présente l'inconvénient 
d'être assez lente.

La commande [`pv`](https://www.ivarch.com/programs/pv.shtml) (_pipe viewer_) est plus rapide et permet en plus de voir la progression de la copie :
1. Commencez par brancher la clé source et vérifiez son identifiant
```bash
ls -l /dev/sd?
```
2. Branchez ensuite la clé cible et vérifiez également son identifant
3. Lancez la copie de la clé source vers la clé cible
```bash
sudo sh -c 'pv </dev/sdX >/dev/sdY'
```
## Copier le contenu de la clé

Cette opération demande plus de manipulations mais permet d'éviter de copier l'intégralité de la clé.  
De plus si les deux clés n'ont pas la même capacité cela permet de gérer soit même le partitionnement de la clé cible.
D'ailleurs si la clé cible a une capacité plus faible que la clé source cela sera la seule solution pour copier la clé.

Par contre si le contenu de la clé ne devrait pas être exposé il peut être recommandé d'effectuer les opérations en 
utilisant un système "fiable" comme [Tails](https://tails.boum.org/).

1. (_optionnel_) Démarrez Tails en activant l'accès **administrateur** au login

2. Installez `cryptsetup` (pour utiliser le chiffrement [LUKS](https://fr.wikipedia.org/wiki/LUKS))
```bash
sudo apt update && sudo apt install cryptsetup
```

3. Créez un répertoire temporaire afin d'y copier le contenu de la clé source
```bash
export BACKUP_DIR=$(mktemp -d)
```
4. Branchez la clé source et vérifiez son identifiant
```bash
ls -l /dev/sd?
```

5. Ouvrez la partition chiffrée de la clé source
```bash
sudo cryptsetup luksOpen /dev/sdX1 usb
```

6. Montez la partition
```bash
sudo mount /dev/mapper/usb /mnt
```

7. Copiez le contenu de la clé vers le répertoire temporaire
```bash
sudo cp -avi /mnt/ ${BACKUP_DIR}
```

8. Démontez la partition
```bash
sudo umount /mnt
```

9. Fermez la partition chiffrée
```bash
sudo cryptsetup luksClose usb
```

10. Branchez la clé cible et récupérez son identifiant
```bash
ls -l /dev/sd?
```

11. Créez une partition pour y copier les fichiers
```bash
fdisk /dev/sdY
```

12. Chiffrez la partition
```bash
sudo cryptsetup luksFormat /dev/sdY1
```

13. Ouvrez la partition chiffrée
```bash
sudo luksOpen /dev/sdY1 usb
```

14. Formattez la partition en ext4
```bash
sudo mkfs.ext4 /dev/mapper/usb -L usb
```

15. Montez la partition
```bash
sudo mount /dev/mapper/usb /mnt
```

16. Copiez les fichiers sur la clé cible
```bash
sudo cp -avi ${BACKUP_DIR}/* /mnt/
```

17. Démontez la partition
```bash
sudo umount /mnt
```
18. Et enfin fermez la partition chiffrée !
```bash
sudo cryptsetup luksClose usb
```
