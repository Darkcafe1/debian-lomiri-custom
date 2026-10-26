# Mirai Linux

Distro experimental con **Debian 13 (trixie, estable)** y **Lomiri** como escritorio, pensada para PC.

> ⚠️ **Estado: pre-alfa.** Por ahora todo se prueba en una máquina virtual (VirtualBox). **Todavía no hay ISO descargable.**

<!-- Captura pendiente: ![fastfetch](docs/img/fastfetch.png) -->

## ¿Qué es esto?

Mirai Linux (*mirai* = "futuro" en japonés) es un proyecto personal con dos objetivos:

- **Campo de pruebas de Lomiri en PC**, usando Debian como base en lugar de Ubuntu.
- **Aprender a crear una distro** y documentar el camino completo, incluidos los errores.

La idea es una ISO **minimalista y limpia**: el sistema base con Lomiri y lo mínimo necesario, para que cada persona instale después las aplicaciones que prefiera.

## ¿Por qué Debian?

El proyecto parte de la idea de que Debian es una base estable, personalizable y ligera. Se eligió la rama **estable (trixie)** para tener un sistema sólido mientras se define el resto. Como contrapartida, Lomiri viene en la versión que incluye trixie (0.5.0-2), que no es la más reciente. Esa decisión se podrá revisar más adelante.

## Estado actual

| Dato | Valor |
|---|---|
| Base | Debian 13 "trixie" (estable) |
| Escritorio | Lomiri (sesión Wayland) con LightDM |
| Entorno de pruebas | VirtualBox |
| Consumo de RAM en reposo | ~644 MiB (medido en la VM, día 3) |
| ISO | No disponible todavía |

## Filosofía del software

- Sin Snap. Se usa **Flatpak** para aplicaciones externas.
- ISO limpia: no se incluye suite ofimática; la elige el usuario.
- Se prefieren aplicaciones ligeras y se evita arrastrar dependencias pesadas.

La lista detallada de lo que se incluye, lo que se quita y lo que está en duda está en [`receta.md`](receta.md).

## Contenido del repositorio

| Archivo | Descripción |
|---|---|
| [`registro-diario.md`](registro-diario.md) | Bitácora día a día del proyecto |
| [`receta.md`](receta.md) | Decisiones sobre el software de la ISO |
| `.gitignore` | Reglas para ISO, VirtualBox y logs |

## Hoja de ruta

- [x] Instalar Debian + Lomiri en una VM
- [x] Hacer funcionar Lomiri en modo escritorio
- [x] Limpiar aplicaciones innecesarias
- [x] Crear el repositorio y la bitácora
- [ ] Exportar la receta desde la VM (paquetes, ajustes, versiones)
- [ ] Primera ISO mínima con `live-build`
- [ ] Probar en hardware real (especialmente WiFi)
- [ ] Instalador (por definir)
- [ ] Branding y licencia

## Contribuir

El proyecto está en una etapa muy temprana. Ideas, correcciones y sugerencias son bienvenidas en los [Issues](../../issues).

## Aviso

Mirai Linux no está afiliado a Debian ni a UBports. Debian es una marca registrada de Software in the Public Interest, Inc. Lomiri es desarrollado por la comunidad UBports.
