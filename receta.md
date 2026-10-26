# Mirai Linux — Receta de software

Decisiones sobre el software de la ISO. **Última actualización: 26/10/2026 (día 5).**

Base: Debian 13 (trixie) + Lomiri. Criterio general: ISO limpia, ligera y sin dependencias innecesarias.

## ✅ Se incluye

| Software | Notas |
|---|---|
| Analizador de disco | En la VM se usó baobab (y ncdu en terminal) |
| Monitor de resolución | _Por confirmar qué paquete es exactamente_ |
| Flatpak | Para aplicaciones externas (sin Snap) |
| Firefox (Flatpak) | Reemplaza al navegador Morph |
| Gestor de software | _Por confirmar cuál_ |
| GDebi | Instalador de archivos `.deb` |
| Synaptic | Gestor de paquetes gráfico |

## ❌ Se quita

| Software | Estado |
|---|---|
| Cámara | Por quitar |
| Contactos | Por quitar |
| debian-reference | Ya eliminado (día 3) |
| Dekko | Ya eliminado (día 3) |
| Galería | Ya eliminado (día 2) |
| LibreOffice | Ya eliminado (día 2) |
| Navegador Morph | Por quitar (se usa Firefox Flatpak) |
| Teleports | Ya eliminado (día 3) |

## 🤔 En duda: visor de imágenes

| Opción | Pros | Contras |
|---|---|---|
| Eye of GNOME (eog) | Visor completo y conocido | Pesó ~300 MB al instalarlo; arrastra paquetería de GNOME |
| Viewnior | Más ligero, menos dependencias | Menos funciones |

Inclinación actual: evitar depender de la paquetería de GNOME.

## 🚫 No se incluye (decisión de diseño)

| Software | Motivo |
|---|---|
| OnlyOffice | La ISO queda limpia; cada usuario elige su suite ofimática |
| Cualquier otra suite ofimática | Mismo motivo |

## Notas técnicas para la ISO

- Una ISO de `live-build` instala paquetes `.deb` de forma sencilla, pero **Flatpak y Firefox (Flatpak) necesitan un paso extra** (por ejemplo, un script de primer arranque), porque las apps Flatpak no se instalan como paquetes normales.
- Para medir el peso real de un paquete antes de instalarlo: `apt-get install -s <paquete>`.
- Para exportar la lista real de paquetes: `apt-mark showmanual > paquetes-manuales.txt`.

## Pendiente de aclarar

- [ ] Qué paquetes son exactamente "Monitor de resolución" y "Gestor de software"
- [ ] Decidir visor de imágenes
- [ ] Quitar Cámara, Contactos y Morph de la VM
- [ ] Verificar la lista final con `apt-mark showmanual`
