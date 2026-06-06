# Ataque DoS mediante CDP
## Practica de Laboratorio - Seguridad de la Informacion | ITLA

> **Entorno:** Laboratorio academico controlado y autorizado  
> **Matricula:** 20250730   
> **Fecha:** Junio 2025

---

## Objetivo

Demostrar como el protocolo CDP (Cisco Discovery Protocol) puede ser explotado para realizar un ataque de Denegacion de Servicio (DoS) contra switches Cisco, agotando su tabla CDP y los recursos de memoria/CPU mediante el envio masivo de paquetes falsificados.

---

## Topologia
[Kali Linux - Atacante]
       IP: 202.50.73.10/24
              |
         [eth0 / Fa0/1]
              |
   +----------+----------+
   |  SWITCH CISCO (S1)  |
   |  Mgmt: 202.50.73.254|
   +----------+----------+
         |           |
     [Fa0/2]     [Fa0/3]
        |               |
  [PC Victima 1]  [PC Victima 2]
  202.50.73.20    202.50.73.30

| Dispositivo     | IP              | Rol                |
|-----------------|-----------------|--------------------|
| Kali Linux      | 202.50.73.10/24 | Atacante           |
| Switch Cisco S1 | 202.50.73.254   | Victima (objetivo) |
| PC Victima 1    | 202.50.73.20/24 | Host en la red     |
| PC Victima 2    | 202.50.73.30/24 | Host en la red     |

> Red base derivada de matricula **20250730** → `202.50.73.0/24`

---

## Requisitos

- **SO Atacante:** Kali Linux 2023.1+
- **Emulador:** GNS3 o EVE-NG con imagen Cisco IOS
- **Python:** 3.8 o superior
- **Libreria:** Scapy

---

## Instalacion

```bash
# Clonar el repositorio
git clone https://github.com/TU_USUARIO/ataque1-dos-cdp.git
cd ataque1-dos-cdp

# Instalar dependencia
sudo apt update && sudo apt install python3-scapy -y
```

---

## Uso

```bash
# Uso basico (interfaz eth0, paquetes ilimitados)
sudo python3 cdp_dos.py -i eth0

# Enviar exactamente 500 paquetes con delay de 10ms
sudo python3 cdp_dos.py -i eth0 -c 500 -d 0.01

# Ver comandos de contramedida
python3 cdp_dos.py --contramedida
```

### Verificacion en el switch (durante el ataque)

```ios
Switch# show cdp neighbors
Switch# show processes memory | include CDP
Switch# show processes cpu sorted
```

---

## Evidencias

Incluir las siguientes capturas en la carpeta `/evidencias`:

| Evidencia | Descripcion |
|-----------|-------------|
| `pre_ataque_cdp_neighbors.png` | `show cdp neighbors` antes del ataque |
| `wireshark_cdp_flood.png` | Captura Wireshark de los paquetes CDP |
| `post_ataque_memory.png` | `show processes memory` durante el ataque |
| `contramedida_config.png` | Configuracion `no cdp enable` aplicada |
| `verificacion_mitigacion.png` | Confirmacion de que el ataque fue mitigado |

---

## Contramedidas

```ios
! Deshabilitar CDP en puertos de acceso (recomendado)
Switch(config)# interface range FastEthernet0/1 - 24
Switch(config-if-range)# no cdp enable
Switch(config-if-range)# exit

! Verificar
Switch# show cdp interface FastEthernet0/1
Switch# clear cdp table
```

---

## Estructura del Repositorio

```
ataque1-dos-cdp/
├── README.md
├── cdp_dos.py                  # Script principal
├── Ataque1_DoS_CDP_20250730.docx  # Documentacion tecnica
└── evidencias/
    ├── pre_ataque_cdp_neighbors.png
    ├── wireshark_cdp_flood.png
    ├── post_ataque_memory.png
    ├── contramedida_config.png
    └── verificacion_mitigacion.png
```
---

## Autor

**Matricula:** 20250730  
**Institucion:** Instituto Tecnologico de las Americas (ITLA)  
**Fecha:** Junio 2025
