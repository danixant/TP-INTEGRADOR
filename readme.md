# TPServer - 404 Team Found

## 👥 Integrantes del grupo

- Daniel Antoniazzi
- Tomas Musso
- Assael Bussi

## 🧠 Descripción

Trabajo práctico realizado en una máquina virtual Debian 12 ARM64, cuyo objetivo principal fue configurar servicios básicos de red y servidor, implementar backups automáticos y comprender la administración de sistemas Linux.

---

## ⚙️ Configuración realizada

### 🖥️ Entorno

- SO: Debian 12 (arm64)
- Hostname configurado: `TPServer`
- IP estática configurada:
  - IP: `192.168.0.176`
  - Netmask: `255.255.255.0`
  - Gateway: `192.168.0.1`

### 🗺️ Diagrama topológico

                  +--------------------------+
                  |   Máquina Física (Host)  |
                  |         (Windows)        |
                  +--------------------------+
                           |
                           |
                  +--------------------------+
                  | Red Local (192.168.0.0/24) |
                  +--------------------------+
                           |
                           |
     +-------------------------------------------------------+
     |                Debian VM - TPServer                   |
     |                IP: 192.168.0.176 (estática)           |
     |                Hostname: TPServer                     |
     +-------------------------------------------------------+
            |                                      |
            |                                      |
+--------------------------+           +--------------------------+
|         Apache2          |           |         MariaDB          |
| DocumentRoot -> /www_dir |           | BD 'ingenieria' cargada  |
+--------------------------+           |      con db.sql          |
           |                           +--------------------------+
           |
 +-----------------------+
 | index.php + logo.png  |
 +-----------------------+


Almacenamiento (Disco /dev/sdc):
 +--------------------------------+
 | /www_dir (3GB - /dev/sdc1)     |
 +--------------------------------+
 | /backup_dir (6GB - /dev/sdc2)  |
 +--------------------------------+


Script:
 +-----------------------------------------------------+
 | /opt/scripts/backup_full.sh                         |
 | - Recibe [ORIGEN] como argumento                    |
 | - Valida que origen y destino (/backup_dir) existan |
 | - Genera .tar.gz con fecha (YYYYMMDD)               |
 | - Soporta opción -help                              |
 +-----------------------------------------------------+


Cronjobs (crontab -e):
 +---------------------------------------------------------+
 | - 00:00 todos los días -> backup /var/log               |
 | - 23:00 lunes, miércoles y viernes -> backup /www_dir   |
 +---------------------------------------------------------+

