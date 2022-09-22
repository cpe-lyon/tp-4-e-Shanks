# **Exercice 1. Commandes de base**

1 - Pour mettre à jour le système, on va utiliser les commandes **sudo apt-get update** et **sudo apt-get upgrade**.

2 - A l'aide de nano on ouvre le fichier .bashrc puis dans la partie des alias, on ajoute la ligne suivante: **alias maj='sudo apt-get update && sudo apt-get upgrade'**. Une fois ajoutée, on quitte en sauvegardant le fichier. Enfin on exécute la commande **source .bashrc** pour prendre en compte les modifications sans avoir à redémarrer le serveur. Ce sont ces étapes qui permettent de sauvegarder l'alias pour pas qu'il ne soit perdu au prochain démarrage.

3 - Afin d'obtenir les 5 derniers paquets installés sur la machine, on tape la commande **cat /var/log/dpkg.log | tail -5**. Pour afficher les paquets installés, il est également possible d'utiliser la commande **dpkg -l**.

4 - Les paquets installés explicitement avec la commande **apt install** sont visibles avec **dpkg -L apt install**.

5 - **dpkg -l | wc -l** qui donne 623 et **apt list --installed | wc -l** qui donne 616.

6 - 

7 -

8 -
