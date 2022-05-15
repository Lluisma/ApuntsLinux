# Linux : Ordres De Terminal


## Ordres

|                         |                                |
| ----------------------- | ------------------------------ |
| `type nom_ordre`        | Tipus ordre (intern o extern)
| `ordre1;ordre2`         | Execució simultània d'ordres
| `ordre1&&ordre2`  	  | Executa 2n només si ha executat el 1r
| `ordre1||ordre2`  	  | Executa 2n si NO ha executat el 1r
| `(ordre1;ordre2)>desti` |	Si es volen redireccionar tots dos
| `ordre1 $(ordre2)`      | Anidar ordre (p.e. dins un `echo`)

> interna 

> externa : cal incloure camí a $PATH (directoris on buscar ordres)

> àlies


## Usuaris i Grups

|                | Usuari				    | Grup
| -------------- | ------------------------ | -------------------
| Afegir         | `adduser nom_usuari`     | `addgroup nom_grup`
| Af. a grup     | `adduser nom_u nom_grup` |
| Modificar      | `usermod [] nom_usuari`  |
| Caducitat      | `chage [] nom_usuari`    |
| Eliminar       |  `userdel nom_usuari`    | `groupdel nom_grup`
| > també `home` | `userdel -r nom_usuari`  |
| Grups usuari   | `groups nom_usuari`      |
| Canvi Contr.   | `passwd nom_usuari`      |
| **Llistes**    |                          |
| Usuaris        | `/etc/passwd`            | `/etc/groups`
| > sudoers      | `/etc/sudoers`           |
| Passwords      | `/etc/shadow`            |
| Plantilla      | `/etc/skel`		| *Per aplicar a nous usuaris*


## echo

|                  |                          |
| ---------------- | ------------------------ 
| `echo $NOM_VAR`  | Mostrar el valor d'una variable
| `echo $SHELL`    | Quina *shell* utilitzem
| `echo $0`        | Ens mostra què està executant
|                  |   A la terminal: el tipus de *shell*
|                  |   En un *script*: el nom del propi *script*
| `echo -e "text"` | Formats específics de sortida
|   `\e[3m \e[0m`  | > Cursiva
|   `\n`           | > Salt de línia
|   `\t`           | > Tabulador
| `echo "text\c"`  | Eliminar el salt de línia final
| **Caràcters de protecció**
| `' '`            | No interpreta el contingut (ordres, variables)
| `\`			   | Escapament, el següent no s'aplica (`$`,`"`,`\`)
| `" "`			   | Anul·la tots excepte ordres, `$`, `"`, `\`

> `date +[formats]` : Data i hora

> `logger missatge` : Envia un missatge al *log*


## Operacions amb arxius	

| Acció      | Arxius              | Directoris 
| ---------- | ------------------- | -------------------------------
| Canviar    |                     | `cd nom_directori` / `cd ..`
| Contingut  |                     | `ls`
| Crear      | `touch nom_arxiu`   | `mkdir nom_dir`
| Esborrar   | `rm nom_arxiu`      | `rmdir nom_dir`/`rm -r nom_dir` 
| Moure      | `mv nom_arxiu`      | `mv nom_dir`
| Copiar     | `cp arxiu copia`    | `cp -R directori copia_dir`
| Tipus      | `file nom_arxiu`    |
| Propietari | `chown usu:grp arx` | `chown -R usu:grup direc`
| Permisos   | `chmod 700 nom_arx` | `chmod -R 700 nom_dir`
| (ugo)±(wrx)   | `chmod u+w nom_arx` |
| Còpia remota   | `scp origen* desti*`
| (*) Lloc remot | `nom_usuari@ip:/ruta_arxiu_desti`


## Expressions regulars

|      |                                    |
| ---- | ---------------------------------- |
| `*`  | Un conjunt qualsevol de caràcters
| `?`  | Substitució d'ún únic caràcter
| `[]` | Complir qualsevol de les condicions que posem a dins
|      | - Rang: `[a-f]` o `[1-5]` /
|      | - `[ab]*`: Arxius que comencin per `a` o `b`
| `!`  | Negació (ha d'anar dins dels corxets)


## `|` : Funcions útils per utilitzar en *pipes*

|                                |   |
| ------------------------------ | - |
| `grep patro`                   | Resultats coincidents amb patró
| `grep -r 'patró' nom_dire`     | Cercar patró en arxius
| `wc -l`                        | Recompte de línies
| `cut -d "sep" -f1:3 nom_arxiu` | Retalla els grup 1 i 3
| `sed 'patró' nom_arxiu`        | Canviar paraules en arxius
| `more`          | Paginació resultats retornats
| `less`          | Paginació resultats retornats
| `sort`          | Ordenar alfabèticament
| `sort -n`       | Ordenar numèricament
| `uniq`          | Filtra les repeticions
| `head (-n num)` | Primeres línies (per defecte num = 10)
| `tail (-n num)` | Darreres línies (per defecte num = 10)
| `tail -f arxiu` | Anirà mostrant l'arxiu en temps real
| `tr -s " "`     | Elimina els " " repetits

### Altres

> `awk`: És un llenguatge script en sí mateix, útil per processar patrons sobre línies de text

> `xargs`: Per processar la sortida de comandos


### Mètodes d'arranc del sistema
`/etc/init.d` (*Ubicació dels scripts*)

> `/etc/init.d/` : Tots 
> `/etc/init.d/nom_servei` : Accions
> `/etc/init.d/nom_servei nom_accio` : Retorna info (`systemctl` no)

> `/etc/rc.local` :	Històrics, en desús (reemplaçat per systemd)

Dimoni/servei: procés en segon pla, de forma persistent

* **Upstart** (antic mètode d'arranc Ubuntu, abandonat)

	`service nom_servei nom_accio`

	`service --status-all`	Serveis actius

* **`systemd`**

	`systemctl`					Llista opcions configurables
	`systemctl list-units -t service --all`	Tots
	`systemctl list-units -t service`			Els actius
	`systemctl nom_accio nom_servei`


### Scripts endemoniats
`/etc/systemd/system` (*Ubicació arxius d'unitat*)

	[Unit] 		Description, Before, ...
	[Service] 	Type, RemainAfterExit, ...
	[Install]	WantedBy
	# Comentaris mai al final de línia, línia independent

	systemctl daemon-reload				Recàrrega, informa errors sintaxi
	systemctl enable dimoni.service		Quan arrenqui el sistema
	systemctl enable dimoni.service		Arrencar-lo manualment
	systemctl status dimoni.service		
		o journalctl -u dimoni.service

	/etc/profile		Arxius de configuració dels usuaris
	/etc/bashrc

	trap				Captura senyals d'interrupció del sistema


## Temporització

Dimonis `systemd` també tenen *timers*, amb l'avantatge que és **estàndard** (a diferència de `cron`*)

Cal crear dos arxius (`.service` + `.timer`, ambdós amb igual nom):

`.timer`:

	[Unit]
	[Timer]
	OnCalendar=Dia_setamana Any-Mes-Dia H:m:s // Clàssica
	També rang (1..5), llista (1,5), weekly, hourly...
	RandomizedDelaySec= // retard per evitar
	o
	OnBootSec/OnActiveSec=5m // Rítmica
	OnUnitActiveSec=1w // Cada quan es torna a executar (1 setm)
	[Install]

N'hi haurà prou amb fer enable del `.timer` (del `.service` ja se'n cuidarà el *timer*)

> (*) *No totes les distribucions utilitzen el mateix `cron`*

## `cron` : Automatitzacions

Daemon en segon pla

És molt important la sincronització de la data/hora del servidor, i també entre el rellotge del hardware (BIOS) i el del sistema

	/etc/crontab				Cron general

	min(0-59) hor(0-23) dia(1-31) mes(1-12) dia_set(1-7) usuari /ruta_script
											dia_set(0-6) : 0 diumenge?

	/var/spool/cron/crontabs/	Només accessible pel root
								Ubuntu: cada usuari té el seu

	crontab -e
	sudo contrab -u nom_usuari

	cron -						tasques usuari actual
	sudo con -u nom_usuari -l

	min(0-59) hor(0-23) dia(1-31) mes(1-12) dia_set(1-7) /ruta_script
											dia_set(0-6) : 0 diumenge?
	> rangs					1,2,3,4,5 o 1-5
	> cada 2 hores			0 */2 * * *
	> cada e dies			0 * */e * *
	@reboot
	@yearly/@annually		0 0 1 1 *
	@monthly 				0 0 1 * *
	@weekly 				0 0 * * 0
	@daily/@midnight		0 0 * * * 
	@hourly					0 * * * *

Si el volem executar cada 30", repetim l'script amb delay (`sleep`):

	* * * * 1 nom_script
	* * * * 1 sleep 30 && script

> `/etc/cron.deny` : Afegir usuaris sense accés a `crontab`

> `/etc/cron.allow` :	Únics usuaris als que es dona accés

> `/var/log/syslog` o `/var/log/cron` : Errors del cron

### anacron 
`/etc/anacron`

Revisa arxius que cron no ha executat i que ho haurien d'haver fet	(p.e. ordinador apagat)

	periode(dies) temps_espera(minut) identificador ordre

## Temporització
*`apt install at`*

	sleep unitats		// 30 (segons) 5m (minuts)

	at	minuts:segons	// Programar tasca a exec. en un moment det
						/ (no per tasques periòdiques)
	atq					// Tasques programades
	atrm id_tasca		// Eliminar (el procés no pot matar amb kill)


## SSH : Connexió remota + securització
*`apt install openssh-server`*

Reconfiguració del servei SSH per protecció

	nano /etc/ssh/sshd_config
		Port Num_No_Estandard
		LoginGraceTime 60
		PermitRootLogin no
		X11Forwarding no	(opcional)
		+
		DebianBanner 			no
		MaxAuthTries 			num_max_intents_acces
		ClientAliveInterval 	segons_si_no_hi ha activitat
		MaxStartups 			num_max_de_finestres
		AllowUser 				nom_usuari (*)
	systemctl restart ssh

	ssh usuari@host <<<ordre>>>

(*) IMPORTANT: cal crear abans l'usuari (!)

> `sshpass`: Si es vol utilitzar dins d'scripts amb claus públiques

#### Muntar carpetes remotes per SSH
*`apt install sshfs`*

	cd /media
	mkdir sshfs
	sshfs usuari@ip_equip:/cami_carpeta /media/sshfs/* allow_other
		* punt de muntatge
	mount

##  Claus asimètriques

Per saltar petició autenticació a l'automatitzar feines amb SSH

	ssh-keygen -t rsa	// Generarà dos arxius clays (pub/priv)
	ssh-copy-id -i arxiu_clau_pub usuari@ip_serv	// Copiar clau_pub


## `ufw` : Uncomplicated Firewall

Simplifica la configuració de les iptables

Hia ha 65.536 ports per `TCP/IP`, els primers 1024 estan prefixats:
* 21 : `FTP`
* 22 : `SSH`
* 80 : `HTTP`
* 443 : `HTTPS`
* 3389 : `RDP`
* 3306 : `MYSQL`

|   |   |
| - | - |
| `ufw status` 					| 
| `ufw status numbered`         | llista amb el número de cada regla
| `ufw status verbose`          |
| `ufw enable`                  |
| `ufw disable`                 |
| `ufw reset`					| Esborra les regles
| `ufw default allow outgoing`	| Predeterminat, tot pot sortir
| `ufw allow 22`				| Obrim SSH (22 o el canviat)
| `ufw allow http`				| (podem obrir per port o nom)
| `uf allow 3306:4000`			| (també per rang de ports)
| `ufw delete allow http o ufw delete num_regla`
| `ufw allow 443/tcp`			| Només paquets TCP per HTTPS
| `ufw default deny incoming`	| Cap entrada (excepte anterior)
| `ufw app list`				| Llista els serveis que tenim
									OpenSSH, Apache, Nginx...
| `ufw allow from [IP/port] to 127.0.0.1 app OpenSSH`
								| Limit accés a servei a una IP

	/etc/ufw
		before.rules			Admet ping (protocol ICMP)
								Es poden comentar

> ***Axence netTools*** : App per l'escaneig de ports


## Informació 

|                               |                         |
| ----------------------------- | ----------------------- |
| `pwd`				            | Camí d'on ens trobem
| `uname -a`		          	| Versió Sistema Operatiu		
| `uname -r`		            | Versió del kernel
| `nmap -A -T4 -p2 IP.xx.xx.xx` | Informació d'un servidor


## Monitorització

|                       |                                     |
| --------------------- | ----------------------------------- |
| `uptime`				| Info de la càrrega del sistema (dels darrers 1 minut, 5', 15')
|                       | > 1, hi ha processos en espera
|                       | > 0.7*cores => problemàtic (*pot ser que estigui plena de processos que no consumeixen gaires recursos, poc ús CPU*)
| `top`					| Info uptime + detall tasques, CPU...
| `top -u username`		| Veure l'ús de memòria, ram, processos
| `htop *`				| permet interactuar amb processos
|						| > mostra l'arbre de processos i subpr.
| `/proc`				| Processos
| `ps -aux`				| Llista (estàtica) de processos
| `readlink`    		| Indica l'enllaç al que apunta un procés
| `kill id_proces`		| Matar un procés
| `who`					| Usuaris connectats
| `lsof -c	nom_proces`	| Arxius utilitzats per un proc (pe ssh)
| `lsof +D /directoris`	| Processos utilitats en aquell dir.
| `lsof -i [port/protocol]`	| Processos treballant en xarxa
| `lsof arxiu`			| Indica si l'arxiu està en ús
| `fuser -mv punt_muntatge`	| Info del punt de muntatge
| `netstat	-puta`		| Estat de les connexions de l'equip
| `route`				| Rutes preestablertes al PC, camins que
						segueixen els paquets per arribar als llocs
| `iptraf-ng *`			| Monitor de xarxa en temps real
| `iftop * -i nom_intf`	| Connexions i comunicació en temps real
						(l'interface el retorna ifconfig)
| `bmon *`				| Gràfiques del tràfic generat 
| `df -hT`				| Monitorització espai en disc
| `du -h`				| Ocupació de tots els arxius
| `du -hS /`			| Agrupar per directoris
| `cat /proc/cpuinfo res` |	Número de cores, processor...


## `logwatch` : Informes del que passa al servidor

	logwatch --detail 5 --range all (per defecte, ahir)
			--format html > nom_arxiu.html
			--mailto adreç@mail

> Amb correu local, instal·la *postfix*, cal configurar-lo:

	MailTo = adreç@mail
	Detail = Low | Medi | High (o un núm de 0 a 10)


## `rsync` : Sincronització entre màquines (incloses remotes)

Transmissió eficient de dades incrementals

|   |   |
| - | - |
| `rsync -az /origen /desti/`  | Copiar un arxiu comprimit
| `rsync -zr /origen /desti/`  | Copiar un directori comprimit
| `rsync -zur /origen /desti/` | Només actualitza increments
|		`-v` 			   	   | > Verbós
|		`-n`			   	   | > Simula, però no executa
|		`--delete`			   | > Esborra el que no és a origen
|		`-e protocol`		   | > p.e. ssh entre servidors

(*) Per automatitzar en servidors, no podríem posar passwords, faríem un intercanvi de claus

> `rsnapshot`, basat en rsync


## `tar` : Empaquetat (concatenat) i Comprimit

	tar opcions fitxer_tar fitxers a empaquetar
			c 	// Crear
			x	// Extreure
			z	// Comprimeix (amb gzip)	
		   -g arxiu_metadades

> `dd`: Còpia de dispositius sencers (clonació discs, sectors...)

> `mysqldump`: Còpia base de dades MySQL

