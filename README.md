# COURS1
# Partie 1
Étape 1 : Création d’une machine virtuelle (VM)

1. Ouvrez VirtualBox et cliquez sur « Nouvelle ».
2. Nommez la VM.
3. Configurez les ressources : 2 Go de RAM et 20 Go de disque.
4. Sélectionnez l’ISO Debian pour l’installation.
5. Démarrez la VM et installez Debian Server.
6. Choississez un miroir français
7. Choisissez d’installer un serveur minimal sans interface graphique avec ssh

Étape 2 : Installation de Samba

1. Testez la connexion internet
2. Connectez-vous en SSH si nécessaire ou directement dans la console de la VM. 
3. Mettez à jour les dépôts et installez Samba
4. Créer un utilisateur local
5. Ajouter l'utilisateur à la base de données Samba
6. Créez un dossier partagé /srv/samba/share
7. Donnez les droits d'accès au groupe de votre utilisateur au dossier
8. Changez le propriétaire du répertoire pour l'utilisateur
9. Donnez au propriétaire et au groupe des droits de lecture, d'écriture et d'exécution sur le dossier partagé
10. Ouvvrez le fichier de configuration Samba /etc/samba/smb.conf, descendez jusqu’à la fin du fichier et ajoutez la configuration suivante pour partager un dossier appelé share :
[share]
path = /srv/samba/share
valid users = nom_utilisateur
read only = no
browsable = yes

A quoi servent ces indications ?

11. Redémarrez Samba pour appliquer les modifications.
12. Depuis une machine sur le même réseau, accédez au partage Samba.

# Vous devez maintenant être en mesure d’ajouter, modifier et supprimer des fichiers dans le dossier partagé via Samba.

Étape 3 : Création d’une VM pour OwnCloud

1. Répétez les instructions pour créer une VM Debian.
2. Installez Apache, PHP, et MariaDB pour OwnCloud.
3. Créez une base de données pour OwnCloud :
sudo mysql -u root -p
CREATE DATABASE owncloud;
CREATE USER 'ownclouduser'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON owncloud.* TO 'ownclouduser'@'localhost';
FLUSH PRIVILEGES;
EXIT;

4. Téléchargez et configurez OwnCloud sur le serveur Apache :
- Télécharger le fichier : https://download.owncloud.com/server/stable/owncloud-complete-latest.tar.bz2
- Utiliser tar -xif
- Déplacer le dossier owncloud dans /var/www/
- Créer un utilisateur et l'ajouter au groupe www-data
- Changer le propriétaire du dossier owncloud par l'utilisateur et son groupe créé.
5. Changer le fichier de configuration :
sudo nano /etc/apache2/sites-available/owncloud.conf
<VirtualHost *:80>
    ServerAdmin admin@yourdomain.com
    DocumentRoot /var/www/owncloud
    ServerName IP_MACHINE
    <Directory /var/www/owncloud/>
        Options +FollowSymlinks
        AllowOverride All
        Require all granted
        <IfModule mod_dav.c>
            Dav off
        </IfModule>
        SetEnv HOME /var/www/owncloud
        SetEnv HTTP_HOME /var/www/owncloud
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

6. Redémarrez le service Apache pour que OwnCloud soit opérationnel 

# Partie 2
Étape 1 : Évaluation de la compatibilité

1. Testez l’accès au partage Samba depuis Windows et Linux.
2. Testez l’accès à OwnCloud via navigateur sur plusieurs systèmes.
3. Documentez toute incompatibilité ou difficulté d’utilisation.

Étape 2 : Évaluation de la sécurité

Samba :
1. Créez plusieurs utilisateurs Samba et attribuez-leur des permissions différentes sur le dossier partagé.
Définissez les permissions sur le dossier pour n’autoriser un user qu'à lire et écrire

2. Vérifiez les permissions dans Samba et testez les restrictions d’accès.
- Vérifiez que seuls les utilisateurs autorisés ont accès au dossier partagé.
- Notez l'absence de chiffrement natif.

Owncloud :
3. Connectez-vous à l'interface d'administration de OwnCloud via le navigateur en accédant à l’URL du serveur/owncloud
4. Activez le chiffrement des données.
5. Créez deux groupes (ex. Marketing et RH).
6. Ajoutez des utilisateurs dans chaque groupe et configurez des permissions pour chaque dossier partagé selon le groupe.

7. Comparez les options de sécurité disponibles.


Étape 3 : Mesurer le temps de transfert vers le serveur Samba
1. Utilisez rsync (installer rsync sur les serveurs) pour transférer le fichier vers le dossier partagé Samba et notez le temps total de transfert affiché.
Astuce : Utilisez l'option time pour calculer le temps

Étape 4 : Mesurer le temps de transfert vers le serveur OwnCloud
1. Utilisez rsync pour transférer le fichier vers le dossier de stockage OwnCloud et notez le temps. 

Étape 5 : Analyse des résultats de performance
1. Comparez les temps de transfert obtenus et discutez des raisons de toute différence observée.

Questions :
1. Quels sont les avantages et inconvénients de chaque solution en termes de sécurité ?
2. Sur quel système d’exploitation chaque solution fonctionne-t-elle le mieux ?
3. Quels critères sont les plus importants dans le choix d’une solution de partage de fichiers ?


# Partie 3

Étape 1 : Synthèse des résultats dans une grille d’évaluation
- Complétez une grille d’évaluation en attribuant une note sur 10 pour chaque critère et pondérez-les selon leur importance.

Étape 2 : Formuler une recommandation
- Rédigez un court rapport justifiant le choix final entre Samba et OwnCloud.
