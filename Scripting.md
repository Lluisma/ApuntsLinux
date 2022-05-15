# Scripting en bash

DUBTE:  podem guardar una password en variables d'entorn?
Executar un script com sudo


| Execució | L'arxiu d'script ha de tenir permisos d'execució (x) |
| -------- | ---------------------------------------------------- |
| `./nom_script`         |
| `bash nom_script`      | Imposa bash al shebang definit a l'script
| `bash -x nom_script`   | Permet veure l'execució línia a línia
| `bash -vx nom_script`  | Verbós
| **Ubicació**           | 
| `/bin`                 | Execució directa dels *scripts*
|                        | Sinó incloure el camí `$PATH`
| `bash camí/nom_script` | Sinó cal incloure el camí a la crida
| `source nom_script`    | Executar un *script* dins d'un *script*
||*L'arxiu cridat no cal que tingui *shebangs* ni sigui executable*

El *subscript* s'executa dins d'una *subshell*, però dins del mateix procés (compartiran variables, p.e.)

> `shellckeck` : Anàlisi + recomanacions de millora


## Estructura d'un arxiu .sh

Extensió `.sh` recomanable (no requisit)

> ***shebang***: Indica que és un *script* i la *shell* que s'utilitzarà (bash, python...) : 

    `#!/bin/bash`   

> Per forçar aturada per errors (a l'inici de l'*script*, en DEV) :

    set -o errexit     // Aturada amb un error en execució
    set -o pipefail    // en una pipe
    set -o nounset     // Variables no declarades

> Definir variables que siguin exportades com a variables d'entorn:

    export mi_var=valor     
    local mi_var=valor      // Com a locals per a les funcions


## Estats de sortida

| Estats    |   | 
| --------- | - |
| 0         | Sense error
| 1         | Problemes menors 
| 2         | Problemes seriosos
|           | 
| `echo $?` | Retorna l'estat després de la darrera ordre executada
| `exit`    | Per establir l'estat de sortida (que retornarà `$?`)


## Variables

Canviar les variables d'entorn només afecta al shell que tenim obert

| VARIABLES D'ENTORN          | (en majúsccles)  | 
| --------------------------- | - |
| **variables locals**        | **(en minúscules)**
| `set`                       | Llista completa 
| `NOM_VAR="valor"`           | Assigna el valor 
| `nom_var=valor`             |
| `nom_var=$(date)`           | Assignar el resultat d'una ordre
| `nom_var="text amb espais"` | Per prevenir comportament variables
| `unset nom_var`       	  |	Esborra variable
| `echo $NOM_VAR`             | Mostrar el valor	
| `${nom_var}/cadena`         |	Concatenar amb altres cadenes
| `${#nom_var} `              | Número de caràcters de la variable
| `${variable//text/mon}`     | Canvia "text" del contingut per "mon"
| `${variable:inici:long}`    | Retallar variable
| `read nom_var`	          | Lectura del teclat* (o espera intro)
|   `-p`                      | Afegir prompt
|   `-s`                      | Ocultar entrada usuari
|                             | (*) *Admet múltiples variables*
| **Variables numèriques**    |
| `declare -i nom_var_enter=valor`
| `a++    a+3`                | Increments
| `a--`                       | Decrements
| `$(())`                     | Tracta com numèric `suma=$((10+20))`
| `bc`                        | Calculadora de bash (en *pipe*)
|                             | *bash no admet `/` entre float*

> `div=$(echo "scale=2; $a/$b" | bc)`


##	Paràmetres (variable d'entrada de l'script)

| Variables paramètriques  | - |
| ------------------------ | - |
| `$0`					   | Nom de l'script
| `$1, $2`...			   | Primer paràmetre, segon, etc...
| `${11}`			       | > 9
| `$*`					   | Tots els paràmetres
| `$@`                     | Tots, de forma separada
| `$#`					   | Recompte de paràmetres
| `${1-valor_per_defecte}` |
| `shift`                  | `$1` passa a `$0`, `$2` a `$1`, etc...

  
### Execució en segon pla : afegir & al final

	fg	Per tornar a posar-lo en primer pla

> relacionats: bg, ps, jobs

### nohup Evitar matar un procés quan tanquem la sessió

### Entrades i sortides estàndard + Redireccions

	< passar com a dada d'entrada a una ordre (0: entrada estàndard)
	
	1>desti (també >desti)	Enviar sortida estàndard (1) a l'arxiu indicat
	>>desti	Afegir sense sobreescriure

	2>desti	Per la sortida d'error (2)

	2>/dev/null	Per ocultar (en aquest cas, p.e., els errors)

    &>	Redirigim la sortida normal i la d'errors

	tee desti		Duplica la sortida estandard (a banda de en pantalla)

	sdin(0)
	sdout(1)	
	sderr(2)

Pot ser interessant redirigir les sortides quan estem executant una ordre en segon pla (s'afegeix `&` al final de l'ordre)

    sort < prueba.txt
    cat << FIN > prueba.txt  Demana entrada fins que teclegem la cadena indicada (FIN)
    apt upgrade < tecla.txt
	A tecla.txt conté 's'

## Condicionals

    if[condició] then  o   if test condició then
    elif[condició2] then
    else
    fi

    case $variable in
        valor1) ordres;;
        valor2) ordres;;
        [condicio3]) ordres;;
    esac

| Condicions | - |
| ---------- | - |
| `-d`                    | Existeix/És un directori
| `-f`                    | Existeix/Ès un arxiu
| `-s`                    | És un arxiu no vuit
| `-r`, `-w`, `-x`        | Verifica permisos
| `-z`, `-n`              | Cadena de longitud nul·la o no nul·la
| `-eq`,`-ne`,`-lt`,`-gt` | Comparador numèrics: =, <>, <, >
| `=`,`!=`                | Comparador de cadenes: =, <>
| `!`                     | Inversió del paràmetre

## Bucles

    for variable do
    for variable in llista (*)
    for variable  in {0..12..2}         // inici..final(..increment)
    for (valor_inicial;condició;increment)

    while expressió

    until expressió

El contingut a executar és el mateix per tots

    do
        >>>ordres<<<
        break                           // surt del bucle
        continue                        // salta una iteració
    done

(*) Es pot posar les línies d'un arxiu com a llista de valors d'un `for`

    for var in $(cat nom_arxiu.txt)

També aplicable a `while`

    done < nom_arxiu

## Funcions

Per defecte les variables són globals a tot l'script

Les variables paramètriques dins la funció fan referència a la pròpia funció

    nom_funcio() {                  // Obsolet: function nom(){}
        local variable_local=valor  // Definició forçada de var. local
                                    // Usar-los quan es facin càlculs, p.e.
        return valor_a_retornar     // Equivalent a exit (sols x funcions)
                                    // echo $? (sols immediatament després)
    }

## Matrius (Arrays) > Unidimensionals

    matriu=(primer segon tercer)
    matriu[0]                           // Referenciar valors de la matrius
    cadena="u dos    trs"
    matriu2=($cadana)                                   
    matriu[5]=valor                     // Nous índexs no han de ser consec.

    echo ${matriu[*]}                   // Els mostra tots
    echo ${#matriu[*]}                  // Recompte d'elements
    echo ${!matriu[*]}                  // Mostra els índexs utilitzats

    matriu=($(ordre))                   // Omple la matriu amb una ordre (!!)

## Selector (menús)

    select nom in llista valors separats per espai
    do
        <<<ordres>>>                    // if/case $nom
    done

## dialog             
*`apt install dialog`*

    dialog -menu "text" params_format 1 "Opc 1" 2 "Ppc 2" ... 2>tmp/output.tmp
    $valor_seleccionat=$(cat /tmp/output.tmp)

    dialog --checklist "Test" mides 1 "opc 1" on "opc 2 " off ... 2>...
