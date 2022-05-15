# Instal·lació de paquets

## DEB: paquets Debian/Ubuntu (tècnica antiga)

| |   |
|-| - |
| `dpkg -i  nom_paquet.deb`   | |
| `dpkg -r nom_paquet`        |
| `dpkg --remove -force --force-remove-reinstreq nom_paquet` | Permet identificar paquets trencats
| `dpkg -P nom_paquet`        | Elimina arxius de configuració
| `dpkg -l`                   | Llista completa de paquets
| `dpkg -L nom_paquet`        | Arxius que instal·la un paquet
| `dpkg -p nom_paquet`        | Informació del paquet
| `dpkg -C/-I nom_paquet.deb` |Anteriors però del paquet d'instal·lació 
| `dpkg -S nom_arxiu`         | Quin paquet ha creat aquest arxiu
| `dpkg -V nom_paquet`        | Problemes d'instal·lació del paquet (null=ok)
| `dpkg --get-selections > paq-inst.txt` | Llista paquets instal·lats


---

## APT : Adavanced Packaging Tool 

Millora gestió de dependències

~~~
/etc/apt/sources.list`
/etc/apt/sources.list.d/`
    `deb url_repositori versio_distribucio tipus_repo`
~~~

| Tipus derepositoris | |
| ------------------- | - 
| **Main**            | Suport oficial
| **Restricted**      | Limitacions de copyright
| **Universe**        | Paquets de la comunitat
| **Multiverse**      | No són lliures (però oberts)
| **Backports**       | Darreres versions

---

### Gestió de repositoris

`add-apt-repository ppa:nom_repositori/versio`

	* el guarda a /etc/apt/sources.list.d/
	* retorna la clau de verificació
	* fer update sempre després

`add-apt-repository --remove ppa:nom_repositori/versio`

`ppa-purge ppa:nom_respositori:versio`

Esborra el repo com si no hagués existit [`apt install ppa-purge`]

---

##	Instal·lació de repositoris

| `apt / apt-get`       |  |
| --------------------- | - |
| `update`              | Actualitza la llista de repositoris
| `upgrade`             | 
| `dist-upgrade`        | Actualitza i neteja versions obsoletes
| `autoremove`          | Neteja dependències no necessàries
| `autoclean`           | Clean up any partial packages
| `clean`               | Clean up the apt cache
| `install nom_paquet`  | Instal·lar una aplicació
| `install -f`          | (força instal·lació dependències pendents - encara que s'hagi fet amb dpkg)
| `remove nom_paquet`   | Eliminar paquet
| `--purge remove nom_paquet` | Eliminar també dependències
| `clean`               | Neteja cache paquets descarregats
| **`apt / apt-cache`** |
| `search nom_paquet`   | Busca paquets a repos
| `show nom_paquet`     | Info
	

> Si queden paquets pendents
	*> Synaptic > Estat > No instal·lat (configuració residual)*

### Per compilar (imprescindible)

	sudo apt-get update
	sudo apt-get install build-essential


## Instal·lació arxius `.bin/.run`

	sudo chmod u+x nom_paquet.bin	[o 755]
	sudo ./nom_paquet.bin

## YUM: Sistema d'instal·lació RedHat, CentOS, Fedora

	/etc/yum.repos.d/
	yum install nom_paquet
	yum remove nom_paquet

## ZYPPER: Sistema d'instal·lació Suse, OpenSuse

	zypper install nom_paquet
	zypper remove nom_paquet

## Instal·lació des del codi font
