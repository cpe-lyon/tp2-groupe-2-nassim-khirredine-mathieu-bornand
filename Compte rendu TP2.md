# Rendu TP2 - Nassim KHIRREDINE & Matthieu BORNAND

#### Exercice 1. Variables d’environnement
1. Dans quels dossiers bash trouve-t-il les commandes tapées par l’utilisateur?
- En tapant ```printenv PATH``` on aperçoit que les dossiers bash sont les suivants :
```/Users/labavette/opt/anaconda2/bin:/Users/labavette/opt/anaconda2/condabin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin```
2. Quelle variable d’environnement permet à la commande cd tapée sans argument de vous ramener dans votre répertoire personnel?
- C'est la variable d'environnement ```HOME``` qui contient le chemin vers notre répertoire personnel, ```HOME=/home/serveur```
3. Explicitez le rôle des variables LANG, PWD, OLDPWD, SHELL et _.
    * ```PWD``` est le chemin actuel
    * ```OLDPWD``` est le chemin précedent
    * ```LANG``` est le language d'encryptage des fichiers
    * ```SHELL``` est le chemin vers le fichier d'éxecution ```SHELL=/bin/bash```
    * ```_.``` est le chemin pour accéder à la liste des v.e.
4. Créez une variable locale MY_VAR (le contenu n’a pas d’importance). Vérifiez que la variable existe.
- ```MY_VAR="abcdef"```
- ```echo $MY_VAR```
5. Tapez ensuite la commande bash. Que fait-elle? La variable MY_VAR existe-t-elle? Expliquez. A la fin de cette question, tapez la commande exit pour revenir dans votre session initiale.
- La commande créer une sorte de nouvelle instance et donc la variable créée juste avant n'existe plus. Il faut sortir (exit) de cette nouvelle instance pour qu'on puisse retrouver la variable car elle est locale.
6. Transformez MY_VAR en une variable d’environnement et recommencez la question précédente. Expliquez.
- ```export MY_VAR="abcdef"``` ici on créer la variable d'environnement
- ```bash``` on créer une nouvelle instance pour voir si la variable d'environnement s'est bien créée
- ```echo $MY_VAR``` on affiche le contenu de la variable pour vérifier que la variable d'environnement existe bien.
7. Créer la variable d’environnement NOMS ayant pour contenu vos noms de binômes séparés par un espace. Aﬀicher la valeur de NOMS pour vérifier que l’affectation est correcte.
- ```export NOMS="Nassim Matthieu"```
- ```bash; echo $NOMS``` changement d'instance + afficher la nouvelle v.e.
8. Ecrivez une commande qui aﬀiche ”Bonjour à vous deux, binôme1 binôme2!” (où binôme1 et binôme2 sont vos deux noms) en utilisant la variable NOMS.
- ```echo Bonjour à vous deux, $NOM !```
9. Quelle différence y a-t-il entre donner une valeur vide à une variable et l’utilisation de la commande unset?
- La commande unset supprime la variable alors qu'en donnant une valeur vide la variable reste mais sans valeur.
10. Utilisez la commande echo pour écrire exactement la phrase :$HOME =chemin (où chemin est votre dossier personnel d’après bash)
- ```echo \$HOME = $HOME```

### Programmation Bash
Ajoutez le chemin vers script à votre PATH de manière permanente.
- ```export PATH=$PATH:/home/nkmb15/script``` on ajoute ```$PATH:/``` pour pas que ce qu'il y a dans la variable parte, c'est pour "append".
#### Exercice 2. Contrôle de mot de passe
```bash
#!/bin/bash
PASSWORD=azerty
read -s -p 'Entrez le mot de passe: ' a
echo
if [ $PASSWORD = $a ]
then
        echo "Mot de passe correct"
else
        echo "Mot de passe incorrect"

fi
```
#### Exercice 3. Expressions rationnelles
```bash
#!/bin/bash
  
function is_number()
{
        re='^[+-]?[0-9]+([.][0-9]+)?$'

        if ! [[ $1 =~ $re ]] ; then
                return 1
        else
                return 0
        fi
}

is_number $1

if [ $? = 1 ]
then
        echo 'ERROR : not a number'
else
        echo "$1 is a number"

fi
```

#### Exercice 4. Contrôle d’utilisateur

```bash

#!/bin/bash
  

if [ $# != 1 ]

then
        echo "Utilisation : ${0##*/} nom_utilisateur" 


else
        cat /etc/passwd | cut -d ":" -f 1 | grep -w $1 >/dev/null
        a=$?
        if [ $a != "1" ]
        then
                echo 'Utilisateur valide'
        else
                echo 'Utilisateur invalide'
        fi
fi

```
#### Exercice 5. Factorielle

```bash
#!/bin/bash
  

if [ $# != 1 ]
then
        exit 0
fi

nb=$1
result=1

while [ $nb -gt 1 ]
do
        result=$(($result*$nb))
        let nb--
done


echo $result
```

#### Exercice 6. Le juste prix

```bash

#!/bin/bash
  

nbToFind=$(($RANDOM%1000))




while true
do

read -p 'Saisir une valeur entre 1 et 1000 : ' nbGiven

if [ $nbGiven -eq $nbToFind ]
then
        echo "Félicitations !"
        exit 0
elif [ $nbGiven -gt $nbToFind ]
then
        echo "C'est moins !"

elif [ $nbGiven -lt $nbToFind ]
then
        echo "C'est plus !"
fi


done


```


#### Exercice 7. Statistiques
1. Écrivez un script qui prend en paramètres trois entiers (entre -100 et +100) et affiche le min, le max et la moyenne. Vous pouvez réutiliser la fonction de l’exercice 3 pour vous assurer que les paramètres sont bien des entiers.
+
2. Généralisez le programme à un nombre quelconque de paramètres (pensez à SHIFT).
```bash
#!/bin/bash

min=$1
moyenne=0
nbv=0
max=$1

nb_val=$#

for i in $(seq 1 $#)
do
    if [[ $1 -gt 100 || $1 -lt "-100" ]]
	then
        echo "Error"
        exit 1
    else
    if [ $1 -lt $min ]
	then
		min=$1;  
    else
		max=$1;
    fi
    nbv=$(($nbv+$1))
    shift
    fi
done
    nbv=$(($nbv/$nb_val))
    echo "Max: "$max
    echo "Min: "$min
    echo "Moyenne: "$moy

```
3. Modifiez votre programme pour que les notes ne soient plus données en paramètres, mais saisies et stockées au fur et à mesure dans un tableau.

```bash
#!/bin/bash

min=$1
moyenne=0
nbv=0
max=$1
nb_val=$1

for i in $(seq 1 $#)
do; read -p "Choisissez une valeur:" note
    if [[ $note -gt 100 || $note -lt "-100" ]]
	then
        echo "Error"
        exit 1
    else
    if [ $note -lt $min ]
	then
		min=$note;  
    else
		max=$note;
    fi
    nbv=$(($nbv+$note))
    shift
    fi
done
    nbv=$(($nbv/$nb_val))
    echo "Max: "$max
    echo "Min: "$min
    echo "Moyenne: "$moy
```
