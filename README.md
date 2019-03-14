Règles d'évaluation
===================

La notation se fait selon la grille suivante, lue de façon flexible.  Par
"flexible" comprendre que dans le cas d'un investissement manifeste mais avec
des défauts, nous serons cléments ; en revanche, s'attendre à une notation
sévère sur un rendu mal fini.

Une parfaite réalisation jusqu'au point "if-then-else" assure un 8/20.  La
notation finale du cours est ainsi faite :

    if min(project, controle) <= 6 / 20 then
       (project + controle) / 2
    else
       max(project, controle)

La date de rendu est le **15 Avril 12h**.

## tarball correcte (1pt)
Une tarball jagger-FOO.tar.bz2 où FOO est le nom du chef de groupe.  Quand on
ouvre cette tarball, elle fabrique un répertoire jagger-FOO dans lequel sont
tous les fichiers.  Je ne veux pas de fichiers qui traîntent à côté de la
tarball, ni de répertoire de nom différent.

Cette tarball doit contenir un fichier README.txt qui détaille les membres du
groupe, puis explique le rendu (qu'est-ce qui est fait, quelles sont les
difficultés rencontrées, les originalités, éventuellement les problèmes
bloquants etc.)

## make marche (1pt)
Je dois pouvoir compiler le projet sur une machine macOS sans difficulté, en
ligne de commande.  Ça veut dire, pas de projet Eclipse ou que sais-je.

## make check marche (1pt)
Il doit y avoir une batterie de tests non ridicule.  Il doit y avoir des tests
positifs (le programme donné à votre jagger est correct et se comporte comme
attendu), mais aussi des tests négatifs (vérification de détection d'erreurs).

## les entiers avec les bonnes priorités (2pt)
Toute l'arithmétique (les binaires +, -, *, /, et les unaires +, -),
et les comparaisons (=, <>, <=, <, >=, >).  Les parenthèses aussi bien sûr.

## une primitive `print` (1pt)
Introduire le support d'une fonction prédéfinie "print", qui prend un unique
argument, et l'affiche sur stdout.

    print(1+2*3)
    => 7

Les parenthèses sont obligatoires.  Vous pouvez, à cette étape, traiter `print`
comme un mot clé.

A se stade, l'exécution d'un programme consiste en l'affichage du source
(pretty_print), puis l'affichage du résultat (eval).

Ne cherchez pas à faire calculatrice interactive, faites un interpréteur par
lot : lire un fichier en entier, le pretty-printer, puis l'évaluer.

## support de if-then-else (2pt)
Des expressions telles que `if 1 then 2 else 3`, `if if 0 then 1 else 2 then 1+2
else 2+3` doivent fonctionner.  Noter que dans ce dernier cas, c'est bien le
`2+3` qui est "sous" le `else` : les opérations sont plus prioritaires que les
structures de contrôle.

## support des chaînes de caractères (3pt)
La concaténation (`"foo" + "bar"` donne "foobar"), les comparaisons
(=, <>, <=, <, >=, >).

Vérifier les types statiquement.  Par exemple

     "1" = 1
    if 1 then "1" else 1

sont invalides.

----------------------------------------------------------------------

## support des variables et des scopes (1pt pretty-print, 4pt bind)
Attention, au dela de ce point, ne plus chercher à ressembler à une
calculatrice.  En particulier, on ne peut pas introduire une variable dans une
ligne et vouloir s'en servir dans un calcul plus loin.

La syntaxe est la suivante :

    let
      var foo := 1
      var bar := 1 + foo
      var baz := bar * bar
    in
      foo, bar * baz
    end

ou encore

    let in 1 + 1 end

    let var i := 1 in print(i) end

Le programme suivant doit afficher 10, puis 100, puis 10.

    let var i := 10
    in
      print(i),
      let var i := i * i
      in
        print(i)
      end,
      print(i)
    end

Les exemples suivants sont incorrects.

    let var foo in end

    let var foo := 1
        var bar := 1
        var foo := 1
    in 1 end


## Affectation (1pt)

La structure `var i := 1` n'est pas une affectation, c'est une
déclaration/définition de la variable `i`.  Si l'on s'arrêtait ici, nous aurions
les variables "pures" des langages fonctionnels, i.e., des variables constantes,
ou des abréviations pour des expressions complexes en quelque sorte.

Notre langage est impératif, on veut désormais pouvoir changer la valeur d'une
variable.  Ajouter le support de `i := 42`, `i := i + 1`, etc.

## support de while (2pt)

Dans l'exemple suivant, outre le pretty-printing (toujours obligatoire),
l'évaluation conduira à l'affichage de 1, 2, 3, et 4.

    let var i := 1
    in
      while i < 5 do
        (print(i), i := i + 1)
    end

## support de for (1pt)

L'exemple suivante affiche `2, 3, 1`.

    let var i := 1
    in
      for i := 2 to 3 do print(i),
      print(i)
    end

Celui-ci est correct, mais n'affiche rien

    for i := 2 to 1 do print(i)

L'exemple suivant est incorrect.

    (for i := 1 to 2 do i, print(i))

## and beyond
Votre imagination est la seule limite.  Si vous en manquez, nous pouvons vous
donner des idées supplémentaires, demandez-nous.
