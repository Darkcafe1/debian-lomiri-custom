# Mirai Linux — Registro diario

Distro experimental basada en **Debian 13 (trixie, estable)** con **Lomiri** como escritorio, pensada como campo de pruebas de Lomiri en PC y como proyecto de aprendizaje para crear una distro.

> Estado: **pre-alfa**. Todo se probó solo en máquinas virtuales (VirtualBox). Ya hay ISOs de prueba construidas con `live-build`, pero ninguna es estable todavía.

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
| Última medición de la VM de trabajo (día 6) | 1210 paquetes, 5,61 GiB usados (24 %) |
| ISOs construidas | 3 útiles (ISO1: 2,0 GB · ISO2: 1,4 GB · ISO3: lista de 39 paquetes + `user-setup` y `tzdata`) |
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

- [x] Crear el repositorio (GitHub) y subir este registro (día 4)
- [ ] README inicial con capturas (fastfetch y escritorio)
- [x] Exportar la receta (día 6; falta re-exportar tras la limpieza): `apt-mark showmanual > paquetes-manuales.txt` y los ajustes de dconf
- [ ] Hacer los ajustes de Lomiri por defecto con un archivo `.gschema.override` (en vez de `gsettings` en hooks)
- [x] Primera ISO mínima con `live-build` (día 9)
- [ ] Probar en hardware real, sobre todo el WiFi
- [ ] Decidir cómo empaquetar OnlyOffice
- [ ] Revisar que el nombre "Mirai Linux" no choque con otro proyecto
- [ ] Gestor de software y opciones de pantalla en Lomiri: investigar si es limitación de la VM o de Lomiri

---

## Día 4 — 25/10/2026 · Repositorio en GitHub

**Hecho**
- Cuenta de GitHub creada.
- Repositorio público `debian-lomiri-custom` creado.
- Subido `registro-diario.md` (bitácora de los días 0 a 3).
- Creado `.gitignore` con reglas para ISO, VirtualBox y logs.

**Notas**
- Día con poco tiempo frente a la PC; no hubo cambios en la VM.

## Día 5 — 26/10/2026 · Receta verificada y limpieza

**Hecho**
- Redactados `README.md` y `receta.md` (software que se incluye, se quita y está en duda).
- Verificado el estado real de la VM con `apt-mark showmanual`, `dpkg -l` y `flatpak list`.
- Eliminados `eog`, `zutty`, `fonts-noto-extra` y `yelp`, más 61 paquetes huérfanos con `autoremove`.
- Paquetes: 1388 → 1315. Disco usado: 6,5 GiB → 5,9 GiB.
- Instantánea de la VM en VirtualBox antes de seguir limpiando.

**Hallazgos**
- Solo estaba instalado `gdebi-core`; falta `gdebi` (versión gráfica).
- Firefox y Resources están como Flatpak: son 2 apps y 5 runtimes (los "7 flatpak" de fastfetch).
- `yelp` llegó como paquete *recomendado* de `baobab` y `ayatana-indicator-session`, y arrastraba WebKit, FFmpeg y más (~276 MB).

**Decisiones**
- Firefox irá en la ISO como `firefox-esr` (paquete de Debian), no como Flatpak.
- Las apps de serie de Lomiri se conservan por ahora.
- Seguir con Debian 13 estable.

**Lecciones**
- Simular antes de borrar: `apt-get remove -s <paquete>`.
- Con `--apt-recommends` desactivado en `live-build`, cadenas como la de yelp no entrarían.
- La ISO debería construirse sumando desde una lista mínima, no restando a esta VM.

**Pendiente**
- Quitar el kernel viejo (6.12.107).
- Decidir visor de imágenes e instalar `gdebi` gráfico.
- Revisar `libonnxruntime`, `libpresage-data` y `libflite1`.
- Medir la RAM en reposo con un arranque limpio (con Firefox abierto llegó a ~1,35 GiB).

## Día 6 — 27/10/2026 · Paquetes manuales, red y limpieza final

**Hecho**
- Exportada la lista de paquetes instalados manualmente (220) en `paquetes-manuales.txt` y agrupada en `receta.md`.
- Verificado que `network-manager` (1.52.1) y `wpasupplicant` (2.10) están instalados.
- Revisado quién trae cada paquete pesado con `apt-cache rdepends --installed`.
- Eliminados `gstreamer1.0-plugins-bad` y 92 paquetes huérfanos (~223 MB), y después `ghostscript`.
- Quitado el kernel viejo (hecho el 26/10).

**Hallazgos**
- No hay ningún paquete `firmware-*` instalado: en hardware real el WiFi puede necesitarlos (sección `non-free-firmware`).
- `enp0s3` figura como "sin gestión" en NetworkManager, porque la red cableada la configuró `ifupdown`.
- `libpresage-data` y `samba-libs` no eran sobras: los usan los teclados de Lomiri y el gestor de archivos, así que se conservaron.
- Los ~90 paquetes de la cadena de `gstreamer1.0-plugins-bad` (códecs, ImageMagick, drivers VA-API) entraron por recomendados.

**Decisiones**
- Pospuesto el arreglo de la red cableada: se usa WiFi en el equipo anfitrión.
- Dejar de recortar la VM y pasar a la primera ISO con `live-build`.

**Métricas**
- Paquetes: 1388 (día 3) → 1210. Disco usado: ~6,5 GiB → 5,61 GiB (24 %).
- RAM: 1,39 GiB con Firefox abierto (no es medición en reposo).

**Pendiente**
- Decidir si la ISO incluye los drivers de aceleración de video (VA-API).
- Elegir idiomas de teclado `lomiri-keyboard-*` para la ISO.
- Identificar el chip WiFi de la PC de pruebas y su paquete `firmware-*`.
- Re-exportar `paquetes-manuales.txt`, que quedó desactualizado tras la limpieza.
- Instalar `gdebi` gráfico y decidir visor de imágenes (Viewnior o eog).
- Medir RAM en reposo con arranque limpio.
- Primera ISO mínima con `live-build`.

## Día 7 — 28/10/2026 · Receta y elección de apps

**Hecho**
- Se pide armar `receta.md` con la selección de software conversada hasta ahora.
- Se consultan opciones para monitor del sistema, navegador, tienda de software, multimedia, visor de imágenes y analizador de disco.

**Decisiones**
- Criterio: no hace falta que todo sea ultraligero; basta con que las apps sean **Qt5**.
- Visor de imágenes: **qimgv** (frente a Eye of GNOME y Viewnior).

## Día 8 — 29/10/2026 · Sin avances

- No hubo trabajo en el proyecto.

## Día 9 — 30/10/2026 · Verificación de paquetes y primera ISO

**Hecho**
- Con `apt-cache policy` se verifica que `qimgv`, `qdirstat`, `sayonara` (versión beta), `qpdfview`, `vlc`, `flameshot`, `keepassxc` y `gdebi` existen en trixie.
- `resources` **no** tiene paquete en trixie (solo Flatpak).
- Se instala `gdebi` (versión gráfica, sin recomendados: 2 paquetes). Se omite por ahora probarlo con un `.deb`.
- **Primera ISO construida con `live-build`** dentro de la VM de trabajo: `mirai-linux-amd64.hybrid.iso` (~2,0 GB), con `apt-recommends` activado y una lista mínima (`lomiri`, `lomiri-desktop-session`, `lightdm`, `network-manager`).

**Pendiente**
- Probar la ISO en una VM nueva.

## Día 10 — 31/10/2026 · Prueba de la ISO1 y segunda ISO

**Prueba de la ISO1** (VM nueva "mirai test")
- Arranca directo a Lomiri con autologin (usuario `user`), en unos 20-32 s desde los mensajes de arranque.
- RAM en reposo: ~665 MB. La resolución es buena y parece haber red.
- Problemas: las ventanas quedan pegadas a la esquina, el sistema y el teclado están en inglés, y el lanzador muestra todas las apps de Lomiri y LibreOffice (por tener `apt-recommends` activado).

**Construcción**
- La carpeta `~/mirai-iso` ocupa ~10,3 GiB (el chroot, ~5 GiB).
- Primer intento de la ISO2: se ejecutó por error en el directorio personal (`~`) en lugar de `~/mirai-iso` y generó una ISO genérica (`live-image-amd64.hybrid.iso`, ~400 MB) que no usa la configuración de Mirai. Hubo que limpiar `~` y repetir.
- **ISO2** (16:20, en `~/mirai-iso`): `apt-recommends` desactivado y lista explícita de 39 paquetes. Pesa **1,4 GB** (la ISO1 pesaba 2,0 GB). Aún sin probar ese día.

**Lección**: comprobar siempre el directorio de trabajo (`pwd`) antes de ejecutar `lb build`.

## Día 11 — 01/11/2026 · Sin avances

- No hubo trabajo en el proyecto.

## Día 12 — 02/11/2026 · ISO3 inestable y diagnóstico

**Hecho**
- **ISO3** (lista de 39 paquetes + `user-setup` y `tzdata`): arranca y muestra el escritorio de Lomiri, pero a los pocos segundos **vuelve a la consola** (inestable).
- **ISO4**: se le pidió a Qwen otra lista de paquetes y otros hooks; salió de solo ~260 MB y VirtualBox no la reconoce. Descartada.
- Plan: volver a la configuración de la ISO3 en `~/mirai-iso` y diagnosticar el cierre de Lomiri con los registros.

**Diagnóstico (noche)**
- Se obtienen registros desde la consola tty2 (el autologin con `user` funciona).
- LightDM inicia la sesión de Lomiri sin errores. El servicio `mediascanner-2.0` falló una vez con status 99.
- No apareció la causa del cierre de Lomiri. Escribir en la consola era casi imposible porque la sesión se reiniciaba en bucle.

**Lección**: no cambiar a la vez lista de paquetes y hooks; modificar una cosa por vez y probar.

**Pendiente**
- Encontrar por qué Lomiri se cierra en la ISO3 (ver logs de la sesión de Lomiri y de LightDM, y probar con otro backend gráfico / driver de video en la VM).
- Probar la ISO2 si no se hizo (y comparar con ISO3).
- Resolver idioma (español) y teclado en la ISO.
- Resolver las ventanas pegadas a la esquina en la ISO (`form-factor` por `.gschema.override`).
- Quitar LibreOffice y apps no deseadas del lanzador en la ISO.
- Decidir si se incluyen drivers VA-API y firmware WiFi (`firmware-*`).
- Re-exportar `paquetes-manuales.txt`.
