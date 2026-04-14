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

<img width="653" height="316" alt="image" src="https://github.com/user-attachments/assets/14c89385-968f-4ef3-b5c0-295524f32034" />

---

## 5. Verificación del nuevo disco con `lsblk`

Después de reiniciar o refrescar el sistema, se verificó que el nuevo disco fue detectado correctamente:

```bash
lsblk
```

 

<img width="681" height="223" alt="image" src="https://github.com/user-attachments/assets/e28d7c6a-33de-490f-91a8-522d499f4eac" />


## 6. Creación de partición con `fdisk` o `parted`

Se creó una nueva partición en el disco adicional utilizando `fdisk` o `parted`.
<img width="675" height="273" alt="image" src="https://github.com/user-attachments/assets/e33e9e87-45b8-4273-80d7-177d369fa1c8" />

<img width="643" height="347" alt="image" src="https://github.com/user-attachments/assets/4e969bda-8e77-4c0d-a22c-78ab23e50190" />


## Resultado final de la partición
![Uploading image.png…]()


