# Linux : Miscel·lània

## Control de finestres

|                      |                   |                |
| -------------------- | ----------------- | -------------- 
| Terminal             | `Ctrl+Alt+F1`     | `Ctrl+Alt+F7`
| Cercar a l'historial | `Ctrl+R`
| Registre historial   | `history`         | *amb un espai abans l'ordre no es registra*
| Manual               | `man`		       | `q`
|                      |                   |
| **Tmux**             |       | *Múltiples pestanyes a la consola*
| - Introducció de comandes    | `Ctrl+B`
| - Divisió horitzontal shell  | `"`	
| - Divisió vertical           | `%`
| - Desplaça per les finestres | `<-` `->`
| - Reorganitza finestres      | ` `	
| - Tanca la pestanya activa   | `exit`
		

## Control màquina

|                | `sudo`            |
| -------------- | -----------------
| Reiniciar      | `reboot`
| Canviar a ROOT | `-s`
| Tancar sessió  | `pkill -9 -u username`


### Configuració de les adreces de xarxa

	/etc/network/interfaces

Caldrà reiniciar el servei per aplicar els canvis

	/etc/init.d/networking restart

(també es pot cridar com service networkin restart)

	O desactivar/activar la connexió ifdown/ifup


### Gestió de contrasenyes

* KeyPass

	URL			Podem definir una URL per llançar una app
	Auto-Type	Autoemplenat en finestres
* LastsPass		<no va tant bé en Linux>


### Serveis

* `mysql` (*Requisit mysql-client*)
* `samba`
* `ssh`


### Android

* **`JuiceSSH* : Connexió remota des de mòbil

- Connectar via  quan es vol accedir a un equip intern
		>> provar si es pot accedir des de web?????

* **`AndFTP`**

* **`SideSync`** Mòbil en PC