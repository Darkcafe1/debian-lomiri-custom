# Mirai Linux — Registro diario

Distro experimental basada en **Debian 13 (trixie, estable)** con **Lomiri** como escritorio, pensada como campo de pruebas de Lomiri en PC y como proyecto de aprendizaje para crear una distro.

> Estado: **pre-alfa**. Todo se probó solo en una máquina virtual (VirtualBox). Todavía no hay ISO.

---

## Resumen rápido

| Dato | Valor |
|---|---|
| Base | Debian 13 "trixie" (estable) |
| Escritorio | Lomiri (sesión Wayland), con LightDM |
| Versión de Lomiri instalada | 0.5.0-2 (la de trixie) |
| Kernel (día 3) | 6.12.113+deb13-amd64 |
| Paquetes (día 3) | 1388 (dpkg) + 5 flatpak |
| RAM en reposo (día 3) | ~644 MiB de 1,93 GiB |
| Disco (día 3) | 6,45 GiB usados de 23,22 GiB |
| Entorno de pruebas | VirtualBox, 2 CPU, 2 GB de RAM |

---

## Día 0 — 28/09/2026 · La idea

- Se plantea el proyecto: una distro con base Debian y Lomiri, como sucesora espiritual de Ubuntu Unity y campo de pruebas de Lomiri fuera de Ubuntu.
- Nombre elegido: **Mirai Linux** (*mirai* = "futuro" en japonés).
- Punto de partida: nunca había usado Debian a fondo (antes tuvo problemas con la detección del WiFi) ni Lomiri en Debian.
- Plan inicial: probar primero en una máquina virtual.

## Día 1 — 22/10/2026 · Primer intento de instalación (GNOME Boxes)

**Hecho**
- Se instala Debian (netinst, instalación no gráfica) en GNOME Boxes ("Cajas").
- Lomiri no aparece en la lista de escritorios del instalador. Solución: instalar el sistema sin escritorio y añadir Lomiri después con `apt`.
- Se instalan `lomiri`, `lomiri-desktop-session` y `lightdm`.

**Problemas y soluciones**

| Problema | Causa | Solución |
|---|---|---|
| Al terminar la instalación volvía al instalador | La ISO seguía montada al reiniciar | Quitar la ISO de la unidad óptica antes de pulsar "Continuar" |
| `sudo` y `reboot`: "orden no encontrada" | Se desmarcó "Utilidades estándar del sistema" en el instalador | Se trabajó como root (`su -`); para apagar se usó la VM desde fuera |
| Lomiri: pantalla negra sin aceleración 3D, y con 3D errores visuales | GNOME Boxes no permite subir la memoria de video | Cambiar a VirtualBox |

**Lección**: dejar marcadas las utilidades estándar del sistema en el instalador.

## Día 2 — 23/10/2026 · Lomiri funcionando en VirtualBox

**Hecho**
- Nueva VM en VirtualBox (video 128 MB, aceleración 3D, controlador gráfico VMSVGA). Se omitió la "instalación desatendida" para controlar el proceso.
- **Lomiri arranca y funciona en Debian.**
- Se detectan fallos: Galería no abre, no hay gestor de software, no hay opciones de pantalla/resolución, y muchas apps se quedan pegadas en la esquina superior izquierda.
- **Arreglo de ventanas**: en Debian el esquema es `com.lomiri.Shell` (no `com.canonical.lomiri.*`). Se aplicó `gsettings set com.lomiri.Shell form-factor 'Desktop'` (valores posibles: Automatic, Desktop, Netbook, TV) y las ventanas ya se pueden mover.
- Limpieza: se eliminan LibreOffice y Galería. Snap nunca estuvo instalado.
- Se configura Flatpak. Se instalan baobab y ncdu (análisis de disco), eog y viewnior (visores de imágenes) y fastfetch.
- Se confirma que Unity Tweak Tool no sirve en Lomiri y que el lanzador no se puede mover abajo.

**Decisiones**
- Sin Snap; Flatpak (con Warehouse) como alternativa.
- OnlyOffice en lugar de LibreOffice, más adelante.

**Pendiente de verificar**: `edge-barrier-sensitivity` y `edge-drag-width` dieron error con valor 0; solo `form-factor` está confirmado como causa del arreglo.

## Día 3 — 24/10/2026 · Limpieza y planificación

**Hecho**
- Se consulta cómo se crea una ISO: herramienta `live-build`, lista de paquetes y hooks de personalización; Calamares como posible instalador.
- Se planea documentar el proyecto en GitHub o Codeberg (README, docs, config, assets, scripts).
- Se eliminan `lomiri-teleports-app`, `dekko`, `lomiri-dekko-app`, `debian-reference-common` y `debian-reference-es`, más 40 dependencias huérfanas con `autoremove` (unos 19,5 MB + 247 MB liberados, incluido Qt WebEngine).
- Se conserva Zutty.
- **Hallazgo**: la VM usa Debian 13 **estable**, no testing como se creía. Lomiri está en 0.5.0-2.
- **Decisión**: seguir con la estable para no reinstalar todo. Objetivo por ahora: dejar la distro lo más limpia posible y decidir después qué incluir en la ISO.
- Consumo en reposo: unos 500-600 MB de RAM, muy satisfactorio para Lomiri.

**Pendiente**
- Warehouse (Flatpak): la instalación global falló por permisos; la alternativa es `flatpak install --user`. Sin confirmar.

---

## Pendientes generales

- [ ] Crear el repositorio (Codeberg o GitHub) y subir este registro
- [ ] README inicial con capturas (fastfetch y escritorio)
- [ ] Exportar la receta: `apt-mark showmanual > paquetes-manuales.txt` y los ajustes de dconf
- [ ] Hacer los ajustes de Lomiri por defecto con un archivo `.gschema.override` (en vez de `gsettings` en hooks)
- [ ] Primera ISO mínima con `live-build` ("hola mundo")
- [ ] Probar en hardware real, sobre todo el WiFi
- [ ] Decidir cómo empaquetar OnlyOffice
- [ ] Revisar que el nombre "Mirai Linux" no choque con otro proyecto
- [ ] Gestor de software y opciones de pantalla en Lomiri: investigar si es limitación de la VM o de Lomiri

---

## Plantilla para próximas entradas

```markdown
## Día N — DD/MM/AAAA · Título corto

**Hecho**
-

**Problemas y soluciones**
| Problema | Causa | Solución |
|---|---|---|

**Decisiones**
-

**Pendiente**
-

**Métricas** (opcional): paquetes, RAM en reposo, disco
```
