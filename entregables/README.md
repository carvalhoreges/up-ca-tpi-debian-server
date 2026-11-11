# Entregables (consigna 6)

Los directorios de la VM `TPVMCA`, comprimidos uno por uno el 11/11/2026, con las carpetas completas:

| Archivo | Carpeta | Cómo se hizo |
|---|---|---|
| `root.tar.gz` | `/root` | `tar -czf /home/entregables/root.tar.gz /root` |
| `etc.tar.gz` | `/etc` | `tar -czf /home/entregables/etc.tar.gz /etc` |
| `opt.tar.gz` | `/opt` | `tar -czf /home/entregables/opt.tar.gz /opt` |
| `www_dir.tar.gz` | `/www_dir` | `tar -czf /home/entregables/www_dir.tar.gz /www_dir` |
| `backup_dir.tar.gz` | `/backup_dir` | `tar -czf /home/entregables/backup_dir.tar.gz /backup_dir` |
| `var.tar.gz.part_aa` … `part_ag` | `/var` | `tar -czf /home/entregables/var.tar.gz /var` y después `split -b 50M /home/entregables/var.tar.gz /home/entregables/var.tar.gz.part_` |

`/var` va en 7 partes de hasta 50 MB porque GitHub no acepta archivos de más de 100 MB.

## Cómo volver a armar `/var`

```bash
cat var.tar.gz.part_* > var.tar.gz
tar -tzf var.tar.gz
```

El shell expande `part_*` en orden alfabético (`aa`, `ab`, … `ag`); `cat` las pega en ese orden; `>` manda el resultado a `var.tar.gz`. `tar -tzf` lista el contenido sin extraer, para comprobar que quedó entero.
