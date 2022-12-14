# **Exercice 1. Commandes de base**

1 - Pour mettre à jour le système, on va utiliser les commandes **sudo apt-get update** et **sudo apt-get upgrade**.

2 - A l'aide de nano on ouvre le fichier .bashrc puis dans la partie des alias, on ajoute la ligne suivante: **alias maj='sudo apt-get update && sudo apt-get upgrade'**. Une fois ajoutée, on quitte en sauvegardant le fichier. Enfin on exécute la commande **source .bashrc** pour prendre en compte les modifications sans avoir à redémarrer le serveur. Ce sont ces étapes qui permettent de sauvegarder l'alias pour pas qu'il ne soit perdu au prochain démarrage.

3 - Afin d'obtenir les 5 derniers paquets installés sur la machine, on tape la commande **cat /var/log/dpkg.log | tail -5**. Pour afficher les paquets installés, il est également possible d'utiliser la commande **dpkg -l**.

4 - Les paquets installés explicitement avec la commande **apt install** sont visibles avec **dpkg -L apt install**.

5 - **dpkg -l | wc -l** qui donne 623 et **apt list --installed | wc -l** qui donne 616.

6 - Avec la commande **apt list | wc -l** on obtient un total de 68953 paquets disponibles en téléchargement.

7 -
* Le paquet glances: application en ligne de commande qui permet d'afficher l'état des principales ressources d'un système, de sa charge du fonctionnement des applications. **sudo apt-get install glances**
* Le paquet tldr: Les pages tldr sont un effort communautaire pour simplifier les pages de manuel bien-aimées avec des exemples pratiques. **sudo apt-get install -y tldr**
* Le paquet hollywood: Cet utilitaire divisera votre console en plusieurs volets d'authentiques technobabble, parfaitement adapté à tout mélodrame geek hollywoodien. Il est particulièrement adapté sur n'importe quel nombre de consoles informatiques dans le arrière-plan de tout excellent technothriller schlock. En d'autres termes, cela permet de simuler une fenêtre de hacking comme au cinéma. **sudo apt-get install hollywood**. (c'est stylé)

8 - Il existe plusieurs paquets pour pouvoir jouer au sudoku, tels que sudoku, gnome-sudoku, ksudoku, sudoku-solver.

# **Exercice 2.**

**dpkg -S ls** est une commande qui permet de trouver le paquet qui a installé la commande ls. On peut obtenir cette information en une seule commande: **dpkg -S $(which -a ls)** ou **which -a ls | xargs dpkg -S**.
* **cd script**
* **touch origine-commande**
* **nano**
```ruby
#!/bin/bash

find_package()
{
        dpkg -S $(which -a $1);
}

find_package $1;

echo $0;
```

# **Exercice 3.**

En utilisant la commande **dpkg** on peut spécifier -s pour trouver le status d'un paquet. Donc, via la commande **dpkg -s [paquet] | grep Status** récupère le status du paquet. On peut ensuite vérifier si ce status existe ou non. La commande **dpkg -s [paquet] | grep Status && echo "INSTALLÉ" || echo "NON INSTALLÉ"** va afficher installé si le paquet est installé ou non installé si le paquet n'est pas installé.
exemple:
![exemple](TP-4_exo3.png)

# **Exercice 4.**

Pour lister les programmes livrés avec coreutils, on peut utiliser la commande **dpkg -L coreutils**.
Celui qui se nomme [, représente la commande [ qui est une commande de test et permet d'évaluer l'expression conditionnelle. C'est en réalité un synonyme de la commande built-in **test**.

# **Exercice 5. aptitude**

Il faut d'abord installer aptitude avec **sudo apt-get install aptitude**. Ensuite, on le lance avec **aptitude**. Une fois dans l'interface, on peut installer les paquets emacs et lynx en faisant une recherche des paquets grâce à /. On peut chercher le paquet emacs avec n. Une fois trouvé, on presse +, ensuite g une première fois puis g une deuxième fois pour installer. (personnellement ça ne fonctionne pas car je dois être root pour installer, la commande **sudo aptitude install emacs lynx** est une alternative à ceci).

Lynx est un navigateurs web en mode texte utilisable via une console ou un terminal.
Emacs est un éditeur de texte très puissant, extensible et personnalisable.

# **Exercice 6. Installation d’un paquet par PPA**

On installe la version Oracle de Java (avec l'ajout des PPA) avec les commandes ci-dessous:
**sudo add-apt-repository ppa:linuxuprising/java**
**sudo apt update**
**sudo apt install oracle-java15-installer**

Via la commande **ls /etc/apt/sources.list.d** on remarque qu'en effet un nouveau fichier a été crée: linuxuprising-ubuntu-java-jammy.list
Ce fichier contient deux liens menant aux Launchpad PPA ("Personal Package Archive") qui sont des référentiels hébergés sur Launchpad que vous pouvez utiliser pour installer (ou mettre à niveau) des packages qui ne sont pas disponibles dans les référentiels officiels d'Ubuntu.

# **Exercice 7. Installation d’un logiciel à partir du code source**

Commençons par cloner le dépôt git https://gitlab.com/jallbrit/cbonsai avec la commande **git clone https://gitlab.com/jallbrit/cbonsai**.
Ceci nous permet de récupérer en local le code source du logiciel cbonsai.

On se place ensuite dans le dossier cbonsai **cd cbonsai** puis **sudo apt install libncursesw5-dev** pour télécharger la librairie ncursesw.
On continue avec **make install PREFIX=~/.local**.
Enfin, on lance avec **~/.local/bin/cbonsai**.

**sudo apt install checkinstall**; 
**sudo checkinstall**
Maintenant, un fichier /home/User/cbonsai/cbonsai_20220926-1_amd64.deb a été crée et on peut utiliser la commande cbonsai depuis n'importe quel dossier.


# **Exercice 8. Création de dépôt personnalisé**

## *Création d’un paquet Debian avec dpkg-deb*

1 - On créer d'abord le le sous-dossier origine-commande avec **mkdir script/origine-commande** dans lequel on créer un sous-dossier avec **cd script/origine-commande/DEBIAN**, ainsi que l'arborescence usr/local/bin avec **mkdir -p script/origine-commande/DEBIAN/usr/local/bin**. Enfin on y ajoute le script réalisé dans l'exercice 2 **mv origine-commande script/origine-commande/DEBIAN/usr/local/bin**.

2 - **touch control**, puis on l'ouvre dans nano pour y écrire:
Package: origine-commande
Version: 0.1
Maintainer: Emeric Daumarie
Architecture: all
Description: Cherche l'origine d'une commande
Section: utils
Priority: optional

3 - **dpkg-deb --build origine-commande** => building package 'origine-commande' in 'origine-commande.deb'.

## *Création du dépôt personnel avec reprepro*

1 - Dans mon dossier personnel, création du dossier repo-cpe qui sera la racine du dépôt: **mkdir repo-cpe**.

2 - On y ajoute deux sous-dossiers: **mkdir repo-cpe/conf ; mkdir repo-cpe/packages**.

3 - **cd repo-cpe/conf**, **touch distributions**. On lance le fichier depuis nano puis on tape:
Origin: Un nom, une URL, ou tout texte expliquant la provenance du dépôt
Label: Nom du dépôt
Codename: focal
Architectures: i386 amd64
Components: universe
Description: Une description du dépôt

4 - D'abord on installe la commande **reprepr** avec **sudo apt install reprepro**. Ensuite on exécute la commande **reprepro -b . export**.

5 - On copie le paquet à l'aide la commande **cp script/origine-commande.deb repo-cpe/packages** et qui va le coller dans le dossier packages. Puis à la racine du dépôt, repo-cpe, on exécute la commande **reprepro -b . includedeb cosmic origine-commande.deb**.

6 - On se place dans le dossier **cd /etc/apt/sources.list.d** puis on créer le fichier **sudo touch repo-cpe.list**. Enfin on tape **sudo nano repo-cpe.list** dans lequel on ajoute *deb file:/home/VOTRE_NOM/repo-cpe cosmic multiverse*.

7 - **sudo apt update**.

## *Signature du dépôt avec GPG*

1 - **gpg --gen-key**
(pour pas oublier:
Real name: Emeric
email: emeric.daumarie@cpe.fr
passphrase: CPElyonAdminLinux2022)

2 - On ouvre le fichier **nano repo-cpe/conf/distributions** et y ajoute la ligne SignWith: yes.

3 - Ajout de la clé au dépôt: on se place dans le dépôt **cd repo-cpe**, et on ajoute la clé avec **reprepro --ask-passphrase -b . export**.

4 - Clé publique ajoutée au dépôt **gpg --export -a "Emeric" > public.key**.

5 - **sudo apt-key add public.key** pour ajouter cette clé à la liste des clés fiables connues de apt.
