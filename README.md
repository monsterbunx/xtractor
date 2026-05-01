# xtractor

> Strings i18n para los instaladores de [jhin](https://github.com/monsterbunx/jhin), sourceables vía curl.

```
   ═╗ ╦╔╦╗╦═╗╔═╗╔═╗╔╦╗╔═╗╦═╗
   ╔╩╦╝ ║ ╠╦╝╠═╣║   ║ ║ ║╠╦╝
   ╩ ╚═ ╩ ╩╚═╩ ╩╚═╝ ╩ ╚═╝╩╚═
```

Cada archivo expone variables `XT_*` con strings ya coloreados (vía [klors](https://github.com/monsterbunx/klors)) listos para `printf`/`echo`.

## Estructura

```
xtractor/
├── docker/es
├── go/es
├── nodejs/es
├── python/es
└── php/es
```

URL pattern: `https://monsterbunx.github.io/xtractor/<app>/<lang>`.

## Uso

```sh
# 1) Source klors (define c_red, c_ok, etc.)
. <(curl -fsSL https://monsterbunx.github.io/klors/klors)

# 2) Source xtractor para tu app+lang (define XT_*)
. <(curl -fsSL https://monsterbunx.github.io/xtractor/docker/es)

# 3) Usar
echo "$XT_TITLE"
printf '%b\n' "$(printf "$XT_DEP_OK" iptables)"
```

## Convenciones

- **Variables `XT_*`**: scoped por app (cada archivo sólo carga las suyas; sourcear otro app sobreescribe).
- **Placeholders `%s`**: las strings con variables las usas con `printf`. Ejemplo: `printf "$XT_DOWNLOAD\n" "containerd_2.2.3-1.deb"`.
- **Colores aplicados al sourcear**: `$(...)` se evalúa al cargar el archivo, así que cada string ya viene con sus secuencias ANSI.
- **Auto-detect TTY**: heredado de klors — si stdout no es terminal o `NO_COLOR` está set, todo viene en texto plano.

## Añadir un idioma

Drop file: `xtractor/<app>/<lang>` con las mismas keys `XT_*`. El loader de jhin (`xt-<app>`) hará fallback a `es` si el lang pedido no existe.

## Para qué sirve

Separar texto de lógica en scripts shell distribuidos por curl-pipe, sin gettext, sin frameworks, sin servidor — un archivo por idioma, una variable por mensaje.
