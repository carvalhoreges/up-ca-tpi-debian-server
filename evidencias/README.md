# Evidencias

Capturas tomadas en la VM y en la máquina física mientras se hacía cada fase. En las que muestran la pantalla entera, la VM es la ventana de VirtualBox; las pruebas "desde la máquina física" se hicieron en PowerShell o en el navegador de Windows.

| Captura | Qué prueba | Consigna |
|---|---|:-:|
| [F1-hostname-y-debian11.png](F1-hostname-y-debian11.png) | después de reiniciar: hostname `TPServer` y Debian 11.10 | 1.2 · 1.3 |
| [F2-debian12-kernel61.png](F2-debian12-kernel61.png) | Debian **12.15** (bookworm), kernel **6.1.0-54**, hostname intacto | 2.1 |
| [F3-ipconfig-y-red-de-la-vm.png](F3-ipconfig-y-red-de-la-vm.png) | antes de fijar la IP: la red de la VM (`ip -br a`, `ip route`, `resolv.conf`, `interfaces`) y el `ipconfig` de la máquina física, en la **misma red** y con el mismo gateway `192.168.1.1` | 3.1 |
| [F3-interfaces-static.png](F3-interfaces-static.png) | `/etc/network/interfaces` con `inet static` y `address`, `netmask`, `gateway`; la `.200` comprobada libre | 3.2 |
| [F3-ip-fija-tras-reboot.png](F3-ip-fija-tras-reboot.png) | después del `reboot`: `192.168.1.200/24` con `valid_lft forever` (estática), `ping` al router y a internet | 3.1 |
| [F3-ping-desde-maquina-fisica.png](F3-ping-desde-maquina-fisica.png) | `ping 192.168.1.200` desde PowerShell: 4 de 4 | 3.1 · nota |
| [F3-ssh-authorized-keys-huella.png](F3-ssh-authorized-keys-huella.png) | `openssh-server` corriendo en el puerto 22; `authorized_keys` en `600` con la clave de la cátedra (huella `SHA256:pvmzaQW4…`) | 2.2 |
| [F3-sshd-config-y-prueba-local.png](F3-sshd-config-y-prueba-local.png) | `PermitRootLogin prohibit-password` + `PubkeyAuthentication yes`, `sshd -T`, y la entrada con clave desde la propia VM | 2.2 |
| [F3-ssh-con-clave-y-sin-clave.png](F3-ssh-con-clave-y-sin-clave.png) | desde la máquina física: **entra con la clave** (`hostname` = `TPServer`) y **sin la clave rechaza hasta la contraseña correcta** | 2.2 · nota |
| [F4-php82-y-archivos-publicados.png](F4-php82-y-archivos-publicados.png) | PHP **8.2.35**, `php_module` en Apache, `mysqli`; `index.php` y `logo.png` en `/var/www/html` | 2.3 |
| [F4-mariadb-base-cargada.png](F4-mariadb-base-cargada.png) | base `ingenieria` con 3 tablas y 15 notas; el usuario `lcars` lee los alumnos | 2.4 |
| [F4-pagina-datos-de-alumnos.png](F4-pagina-datos-de-alumnos.png) | **la página funcionando en el navegador de la máquina física**: "Datos de Alumnos", logo y 15 filas | 2.3 · 2.4 · nota |
| [F5-disco-nuevo-sata2.png](F5-disco-nuevo-sata2.png) | el disco **nuevo** `TPVMCA_1.vdi` de 10 GB en el controlador SATA (puerto 2), reservado dinámicamente | 4.1 |
| [F5-fdisk-sdc-dos-particiones.png](F5-fdisk-sdc-dos-particiones.png) | `fdisk /dev/sdc`: tabla MBR nueva, `sdc1` de 3 GB y `sdc2` de 6 GB, primarias, tipo **83 Linux**, grabadas con `w` | 4.2 |
| [F5-mkfs-montaje-df.png](F5-mkfs-montaje-df.png) | `mkfs.ext4` en las dos particiones (etiquetas `www` y `backup`), `lsblk -f` con los UUID, montaje a mano en `/www_dir` y `/backup_dir`, `lost+found` | 4.2 |
| [F5-fstab-uuid-mount-a-verify.png](F5-fstab-uuid-mount-a-verify.png) | las dos líneas `UUID=… ext4 defaults 0 2` en `fstab`; `mount -a` sin errores y `findmnt --verify` con **0 errores** (los 2 avisos son del CD-ROM vacío) | 4.4 |
| [F5-reboot-montaje-automatico.png](F5-reboot-montaje-automatico.png) | después del `reboot`, las dos particiones montadas solas; `findmnt /www_dir` → `/dev/sdc1 ext4` | 4.5 |
| [F5-apache-www-dir-configtest.png](F5-apache-www-dir-configtest.png) | por SSH: `DocumentRoot /www_dir`, el bloque `<Directory /www_dir/>` en `apache2.conf`, `Syntax OK`, y el `access.log` con los pedidos de la máquina física | 4.3 |
| [F5-pagina-con-index-viejo-renombrado.png](F5-pagina-con-index-viejo-renombrado.png) | la página en el navegador de la máquina física **con el `index.php` de `/var/www/html` renombrado**: sale de `/www_dir` | 4.3 |
| [F5-prueba-renombre-y-opt-particion.png](F5-prueba-renombre-y-opt-particion.png) | el renombre de prueba (`ls /var/www/html` sin `index.php`) y `cat /proc/partitions > /opt/particion` | 4.3 · nota |
| [F6-script-permisos-y-ayuda.png](F6-script-permisos-y-ayuda.png) | `/opt/scripts/backup_full.sh` con `-rwxr-xr-x` y la salida de `-help` | 5.1 · 5.5 |
| [F6-pruebas-y-backups-manuales.png](F6-pruebas-y-backups-manuales.png) | las pruebas de error (sin argumentos, origen `/var/logs` inexistente, `/tmp` sin partición), los dos backups reales `log_bkp_20261110.tar.gz` y `www_dir_bkp_20261110.tar.gz`, y `tar -tzf` con el contenido | 5.2 · 5.3 · 5.4 · 5.6 |
| [F6-cron-journalctl-y-crontab.png](F6-cron-journalctl-y-crontab.png) | el diario de `cron` con `(root) CMD (/opt/scripts/backup_full.sh /www_dir /backup_dir)` y `crontab -l` con las dos líneas de la consigna | 5.7 |
| [F6-cron-corrida-de-las-00-00.png](F6-cron-corrida-de-las-00-00.png) | `log_bkp_20261111.tar.gz` creado por `cron` a las **00:00**, sin nadie conectado | 5.7 |
| [F7-entregables-lista-final.png](F7-entregables-lista-final.png) | los cinco `.tar.gz` y las 7 partes de `/var` en `/home/entregables` | 6.2 |
| [F7-scp-a-la-maquina-fisica.png](F7-scp-a-la-maquina-fisica.png) | la copia de los entregables a la máquina física con `scp`, entrando con la clave | 6.2 · 2.2 |
