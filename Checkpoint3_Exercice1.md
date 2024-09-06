# Exercice 1 : Manipulations pratiques sur VM Windows (temps estimé : 1h30)

## Partie 1 : Gestion des utilisateurs


**Q.1.1.1 Créer l'utilisateur Lionel Lemarchand avec les même attribut de société que Kelly Rhameur.**

![](https://github.com/joris511000/image/blob/check3/Capture%20d'%C3%A9cran%202024-09-06%20091135.png?raw=true)
![](https://github.com/joris511000/image/blob/check3/Capture%20d'%C3%A9cran%202024-09-06%20091220.png?raw=true)


**Q.1.1.2 Créer une OU DeactivatedUsers et déplace le compte désactivé de Kelly Rhameur dedans.**

![](https://github.com/joris511000/image/blob/check3/Capture%20d'%C3%A9cran%202024-09-06%20091553.png?raw=true)
![](https://github.com/joris511000/image/blob/check3/Capture%20d'%C3%A9cran%202024-09-06%20091636.png?raw=true)
![](https://github.com/joris511000/image/blob/check3/Capture%20d'%C3%A9cran%202024-09-06%20091715.png?raw=true)

**Q.1.1.3 Modifier le groupe de l'OU dans laquelle était Kelly Rhameur en conséquence.**

![](https://github.com/joris511000/image/blob/check3/Capture%20d'%C3%A9cran%202024-09-06%20134935.png?raw=true)
![](https://github.com/joris511000/image/blob/check3/Capture%20d'%C3%A9cran%202024-09-06%20134950.png?raw=true)

**Q.1.1.4 Créer le dossier Individuel du nouvel utilisateur et archive celui de Kelly Rhameur en le suffixant par -ARCHIVE.**

![](https://github.com/joris511000/image/blob/check3/Capture%20d'%C3%A9cran%202024-09-06%20092106.png?raw=true)

## Partie 2 : Restriction utilisateurs
**Q.1.2.1 Faire en sorte que l'utilisateur Gabriel Ghul ne puisse se connecter que du lundi au vendredi, de 7h à 17h.**

![](https://github.com/joris511000/image/blob/check3/Capture%20d'%C3%A9cran%202024-09-06%20092551.png?raw=true)
![](https://github.com/joris511000/image/blob/check3/Capture%20d'%C3%A9cran%202024-09-06%20092635.png?raw=true)


**Q.1.2.2 De même, bloquer sa connexion au seul ordinateur CLIENT01.**

![](https://github.com/joris511000/image/blob/check3/Capture%20d'%C3%A9cran%202024-09-06%20092715.png?raw=true)

**Q.1.2.3 Mettre en place une stratégie de mot de passe pour durcir les comptes des utilisateurs de l'OU LabUsers.**

![](https://github.com/joris511000/image/blob/check3/Capture%20d'%C3%A9cran%202024-09-06%20092818.png?raw=true)
![](https://github.com/joris511000/image/blob/check3/Capture%20d'%C3%A9cran%202024-09-06%20093324.png?raw=true)

## Partie 3 : Lecteurs réseaux
**Q.1.3.1 Créer une GPO Drive-Mount qui monte les lecteurs E: et F: sur les clients.**
![](https://github.com/joris511000/image/blob/check3/Capture%20d'%C3%A9cran%202024-09-06%20094052.png?raw=true)