>

## 1. Configuración general en VMwar1

Se creó una máquina virtual en VMware con los recursos necesarios para soportar la instalación de Red Hat Enterprise Linux.




---

## 2. Particionado del disco durante la instalación

Durante el asistente de instalación de RHEL se configuró el esquema de particionado del disco principal.

<img width="1893" height="905" alt="image" src="https://github.com/user-attachments/assets/d4abc74a-0bd6-4f06-86cd-75603d9361c2" />
<img width="1901" height="967" alt="image" src="https://github.com/user-attachments/assets/130569df-ec04-4b6e-82bb-c35a5892d2aa" />



### Esquema sugerido de particiones

| Punto de montaje | Tamaño recomendado | Sistema de archivos |
|-----------------|-------------------|---------------------|
| `/boot`         | 1 GB              | `xfs`               |
| `/`             | 20 GB+            | `xfs`               |
| `/home`         | Variable          | `xfs`               |
| `swap`          | 2–4 GB            | `swap`              |

---

## 3. Proceso de instalación del sistema operativo

Se ejecutó el proceso de instalación completo de RHEL, incluyendo la selección de paquetes y la configuración inicial del sistema.

<img width="656" height="358" alt="image" src="https://github.com/user-attachments/assets/9ee6e5a8-d627-4f27-a026-e996d19538b5" />


---

## 4. Agregar un disco duro adicional de 30 GB

Una vez instalado el sistema, se añadió un segundo disco duro de **30 GB** desde la configuración de la máquina virtual en VMware.

[![Se agrega un disco duro de 30G](images/image4.png)](https://github.com/vice123222/prueba1/blob/main/foto5.png?raw=true)

---

## 5. Verificación del nuevo disco con `lsblk`

Después de reiniciar o refrescar el sistema, se verificó que el nuevo disco fue detectado correctamente:

```bash
lsblk
```

Salida esperada (ejemplo):

```
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda      8:0    0   20G  0 disk
├─sda1   8:1    0    1G  0 part /boot
└─sda2   8:2    0   19G  0 part /
sdb      8:16   0   30G  0 disk          ← nuevo disco
```

![Verificación del nuevo disco detectado en el sistema con lsblk](images/image9.png)

---

## 6. Creación de partición con `fdisk` o `parted`

Se creó una nueva partición en el disco adicional utilizando `fdisk` o `parted`.

![Creación de partición en el nuevo disco](images/image6.png)

![Resultado final de la partición](images/image10.png)

### Con `fdisk`

```bash
sudo fdisk /dev/sdb
```

Pasos dentro de `fdisk`:
- `n` → nueva partición
- `p` → primaria
- Aceptar valores por defecto para usar todo el disco
- `w` → escribir y salir

### Con `parted`

```bash
sudo parted /dev/sdb
(parted) mklabel gpt
(parted) mkpart primary xfs 0% 100%
(parted) quit
```

### Formatear y montar

```bash
# Formatear
sudo mkfs.xfs /dev/sdb1

# Crear punto de montaje
sudo mkdir /mnt/datos

# Montar
sudo mount /dev/sdb1 /mnt/datos

# Verificar
df -h /mnt/datos
```

### Montaje permanente en `/etc/fstab`

```bash
# Obtener UUID del disco
sudo blkid /dev/sdb1

# Agregar al fstab
echo "UUID=<tu-uuid>  /mnt/datos  xfs  defaults  0  0" | sudo tee -a /etc/fstab
```

![Resultado final - partición montada](images/image3.png)

---

## 🛠️ Requisitos

- VMware Workstation / ESXi
- ISO de Red Hat Enterprise Linux (requiere suscripción o cuenta en [developers.redhat.com](https://developers.redhat.com))
- Mínimo 2 GB RAM y 20 GB de disco para la VM

---

## 📎 Referencias

- [Documentación oficial RHEL](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/)
- [Red Hat Developer Program (gratis)](https://developers.redhat.com/register)
- [Guía de particionado RHEL](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/9/html/performing_a_standard_rhel_9_installation/index)
