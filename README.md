# tctransfer

Un wrapper interactivo y minimalista de [tailcat](https://github.com/tailscale/tailcat)
para **transferir archivos rápido** y **abrir una consola SSH al instante** entre
dos máquinas, sobre túneles WireGuard punto a punto, sin cuenta y sin root.

Un solo script, dos pasos: elegís un lado (exponer o conectar) y una acción
(SSH o archivos). Nada más.

## Requisitos

[`tailcat`](https://github.com/tailscale/tailcat) tiene que estar instalado y en
el `PATH` (`brew install tailcat`, un binario estático, un paquete, etc.). El
wrapper solo lo invoca: verifica que el binario exista y, si falta, corta con un
aviso.

## Ejecutarlo

```bash
# ejecutar sin instalar
bash <(curl -fsSL https://raw.githubusercontent.com/ficopix/tctransfer/main/tctransfer)
```

```bash
# o dejarlo instalado
curl -fsSL https://raw.githubusercontent.com/ficopix/tctransfer/main/tctransfer -o ~/.local/bin/tctransfer
chmod +x ~/.local/bin/tctransfer
tctransfer
```

## Cómo funciona

```
tctransfer
├─ Exponer (esta máquina sirve)
│  ├─ Acceso SSH               → tailcat serve --ssh-authorized-keys=... ssh
│  ├─ Recibir archivos (buzón) → tailcat recv <dir>
│  └─ Compartir carpeta (ro)   → tailcat serve files
└─ Conectar (a una dirección tcXXXX)
   ├─ Abrir una sesión SSH     → tailcat ssh <addr>
   ├─ Enviar un archivo        → tailcat cp <archivo> <addr>:
   ├─ Traer un archivo         → tailcat cp <addr>:<remoto> <destino>
   └─ Listar archivos remotos  → tailcat ls -l <addr>
```

Cuando **exponés**, tailcat imprime una dirección corta `tcXXXX`: se la pasás a
quien se conecte. Cuando **conectás**, pegás esa dirección.

## Opciones

Variables de entorno:

| Variable       | Valor por defecto          | Para qué sirve                              |
| -------------- | -------------------------- | ------------------------------------------- |
| `TAILCAT_BIN`  | `tailcat`                  | Ruta al binario de tailcat                  |
| `TCT_SSH_KEYS` | `~/.ssh/authorized_keys`   | `authorized_keys` usado al servir SSH       |

`tctransfer --help` imprime el mismo resumen.

## Notas

- Es solo un wrapper de conveniencia; no reemplaza ni incluye a `tailcat`.
- `tailcat` **no garantiza estabilidad de su CLI**: si cambia un flag río arriba,
  puede que haya que ajustar la línea correspondiente del script.
