# Trabajo práctico integrador grupal · Computación Aplicada

Universidad de Palermo · Computación Aplicada · 2.º cuatrimestre 2026 · **Grupo 2**

Servidor **Debian 12** armado sobre la máquina virtual de la cátedra (`TPVMCA`): acceso de root por SSH con clave, Apache con PHP, MariaDB, un disco nuevo con dos particiones y un script de backup programado con `cron`.

## Integrantes

- Reges Carvalho
- Juan Lucio Fioravanti
- Mateo Mendia
- Ramiro Obelar
- Federico Wilhelem

## Entregables (consigna 6)

Están en la carpeta [`entregables/`](entregables/README.md):

| Directorio | Archivo |
|---|---|
| `/root` | `root.tar.gz` |
| `/etc` | `etc.tar.gz` |
| `/opt` | `opt.tar.gz` |
| `/www_dir` | `www_dir.tar.gz` |
| `/backup_dir` | `backup_dir.tar.gz` |
| `/var` | `var.tar.gz.part_aa` a `var.tar.gz.part_ag` (7 partes de hasta 50 MB) |

Para volver a armar `/var`: `cat var.tar.gz.part_* > var.tar.gz`.

## Qué se hizo en cada consigna

La columna "Dónde verlo" indica el archivo dentro de los entregables. Las capturas están en [`evidencias/`](evidencias/README.md).

| Consigna | Cómo quedó | Dónde verlo | Captura |
|---|---|---|---|
| **1.1** Ensamblar la VM | Las partes `.rar` se ensamblaron y la VM se importó en VirtualBox | — | — |
| **1.2** Clave de root | Blanqueada desde GRUB y cambiada por `palermo` | `etc.tar.gz` → `etc/shadow` | [F1](evidencias/F1-hostname-y-debian11.png) |
| **1.3** Hostname | `TPServer` | `etc.tar.gz` → `etc/hostname` | [F1](evidencias/F1-hostname-y-debian11.png) |
| **2.1** Actualizar el SO | Debian **12.15**, kernel 6.1.0-54 | `etc.tar.gz` → `etc/debian_version` | [F2](evidencias/F2-debian12-kernel61.png) |
| **2.2** SSH con clave | `PermitRootLogin prohibit-password` y `PubkeyAuthentication yes`; la clave pública de la cátedra en `authorized_keys`; el par de claves en el home de root | `etc.tar.gz` → `etc/ssh/sshd_config` · `root.tar.gz` → `root/.ssh/authorized_keys` | [configuración](evidencias/F3-sshd-config-y-prueba-local.png) · [`authorized_keys`](evidencias/F3-ssh-authorized-keys-huella.png) · [desde la máquina física](evidencias/F3-ssh-con-clave-y-sin-clave.png) |
| **2.3** Web | Apache con PHP **8.2.35** sirviendo `index.php` y `logo.png` | `etc.tar.gz` → `etc/apache2/` · `www_dir.tar.gz` | [PHP y archivos](evidencias/F4-php82-y-archivos-publicados.png) · [página](evidencias/F4-pagina-datos-de-alumnos.png) |
| **2.4** Base de datos | MariaDB con `db.sql` cargado: base `ingenieria`, 3 tablas | partes de `/var` → `var/lib/mysql/ingenieria/` | [base cargada](evidencias/F4-mariadb-base-cargada.png) |
| **Nota de la 2** Pruebas desde la máquina física | `ping`, SSH y la página web se probaron desde Windows, en la misma red | partes de `/var` → `var/log/apache2/access.log` | [`ping`](evidencias/F3-ping-desde-maquina-fisica.png) · [SSH](evidencias/F3-ssh-con-clave-y-sin-clave.png) · [página](evidencias/F4-pagina-datos-de-alumnos.png) |
| **3.1** IP estática | `192.168.1.200/24` en `enp0s3`, mismo rango que la máquina física | `etc.tar.gz` → `etc/network/interfaces` | [antes](evidencias/F3-ipconfig-y-red-de-la-vm.png) · [después del reinicio](evidencias/F3-ip-fija-tras-reboot.png) |
| **3.2** `address`, `netmask`, `gateway` | `address 192.168.1.200` · `netmask 255.255.255.0` · `gateway 192.168.1.1` | `etc.tar.gz` → `etc/network/interfaces` | [archivo](evidencias/F3-interfaces-static.png) |
| **4.1** Disco nuevo de 10 GB | `sdc`, agregado en el controlador SATA | `opt.tar.gz` → `opt/particion` | [disco](evidencias/F5-disco-nuevo-sata2.png) |
| **4.2** Dos particiones tipo 83 | `sdc1` de 3 GB y `sdc2` de 6 GB, en ext4 | `opt.tar.gz` → `opt/particion` | [`fdisk`](evidencias/F5-fdisk-sdc-dos-particiones.png) · [`mkfs`](evidencias/F5-mkfs-montaje-df.png) |
| **4.3** `/www_dir` y Apache | `index.php` y `logo.png` en `/www_dir`; `DocumentRoot /www_dir` en `000-default.conf` y `<Directory /www_dir/>` en `apache2.conf` | `etc.tar.gz` → `etc/apache2/sites-available/000-default.conf` y `etc/apache2/apache2.conf` · `www_dir.tar.gz` | [configuración](evidencias/F5-apache-www-dir-configtest.png) · [página](evidencias/F5-pagina-con-index-viejo-renombrado.png) |
| **4.4 y 4.5** Montaje automático | `/www_dir` y `/backup_dir` en `fstab`, por UUID | `etc.tar.gz` → `etc/fstab` | [`fstab`](evidencias/F5-fstab-uuid-mount-a-verify.png) · [después del reinicio](evidencias/F5-reboot-montaje-automatico.png) |
| **Nota de la 4** `/opt/particion` | `cat /proc/partitions > /opt/particion` | `opt.tar.gz` → `opt/particion` | [captura](evidencias/F5-prueba-renombre-y-opt-particion.png) |
| **5.1** Script | `/opt/scripts/backup_full.sh`, con permisos `755` | `opt.tar.gz` → `opt/scripts/backup_full.sh` | [permisos y ayuda](evidencias/F6-script-permisos-y-ayuda.png) |
| **5.2** Nombre con fecha | `<carpeta>_bkp_AAAAMMDD.tar.gz`, con `date +%Y%m%d` | `backup_dir.tar.gz` | [backups](evidencias/F6-pruebas-y-backups-manuales.png) |
| **5.3** Destino | Los backups se guardan en `/backup_dir` | `backup_dir.tar.gz` | [backups](evidencias/F6-pruebas-y-backups-manuales.png) |
| **5.4** Argumentos | `backup_full.sh ORIGEN DESTINO` | `opt.tar.gz` → `opt/scripts/backup_full.sh` | [pruebas](evidencias/F6-pruebas-y-backups-manuales.png) |
| **5.5** Ayuda | `backup_full.sh -help` | `opt.tar.gz` → `opt/scripts/backup_full.sh` | [permisos y ayuda](evidencias/F6-script-permisos-y-ayuda.png) |
| **5.6** Validaciones | Comprueba que el origen y el destino existan y que en el destino haya una partición montada | `opt.tar.gz` → `opt/scripts/backup_full.sh` | [pruebas](evidencias/F6-pruebas-y-backups-manuales.png) |
| **5.7** Calendario | En el `crontab` de root: `0 0 * * *` para `/var/log` y `0 23 * * 1,3,5` para `/www_dir` | partes de `/var` → `var/spool/cron/crontabs/root` | [`crontab` y diario](evidencias/F6-cron-journalctl-y-crontab.png) · [corrida de las 00:00](evidencias/F6-cron-corrida-de-las-00-00.png) |
| **6.1 y 6.2** Entregables | Este repositorio | [`entregables/`](entregables/README.md) | [lista final](evidencias/F7-entregables-lista-final.png) |

## Aclaraciones

- **`/var/log` y no `/var/logs`.** La consigna 5.7 dice `/var/logs`, pero en Debian el directorio es `/var/log` (como en el ejemplo de la 5.2). El `crontab` usa `/var/log`; con `/var/logs` el script informa que el origen no existe.
- **Fecha con cuatro cifras de año.** La consigna 5.2 escribe `YYYMMDD` y el ejemplo es `20240302`; se usó `AAAAMMDD`.
- **Backups que hay en `/backup_dir`.** `log_bkp_20261111.tar.gz` lo generó `cron` a las 00:00. Los del 10/11 se hicieron al probar el script a mano y con una línea de prueba en el `crontab`, que después se quitó.
- **Línea `@reboot` del `crontab` de root.** Ya venía en la máquina virtual; no se modificó.
- **Carpetas completas.** Los `.tar.gz` se armaron sin excluir nada, como pide la consigna; por eso incluyen las claves de la máquina virtual de práctica.
- **Red.** `192.168.1.0/24` es la red doméstica donde se hizo el trabajo.
