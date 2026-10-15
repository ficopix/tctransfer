# tctransfer

A tiny interactive wrapper around [tailcat](https://github.com/tailscale/tailcat)
for **quick file transfers** and **opening an SSH console fast** between two
machines — over WireGuard point-to-point tunnels, with no account and no root.

One script, two steps: pick a side (expose or connect), pick an action (SSH or
files). Nothing else.

## Requirements

[`tailcat`](https://github.com/tailscale/tailcat) must be installed and on your
`PATH` (`brew install tailcat`, a static binary, a package, etc.). The wrapper
only drives it — it checks for the binary and bails out if it is missing.

## Run it

```bash
# run without installing
bash <(curl -fsSL https://raw.githubusercontent.com/ficopix/tctransfer/main/tctransfer)
```

```bash
# or keep it around
curl -fsSL https://raw.githubusercontent.com/ficopix/tctransfer/main/tctransfer -o ~/.local/bin/tctransfer
chmod +x ~/.local/bin/tctransfer
tctransfer
```

## How it works

```
tctransfer
├─ Expose (this machine serves)
│  ├─ SSH access              → tailcat serve --ssh-authorized-keys=... ssh
│  ├─ Receive files (drop box)→ tailcat recv <dir>
│  └─ Share a folder (ro)     → tailcat serve files
└─ Connect (to a tcXXXX address)
   ├─ Open an SSH session     → tailcat ssh <addr>
   ├─ Send a file             → tailcat cp <file> <addr>:
   ├─ Fetch a file            → tailcat cp <addr>:<remote> <dest>
   └─ List remote files       → tailcat ls -l <addr>
```

When you **expose**, tailcat prints a short `tcXXXX` address — share it with
whoever connects. When you **connect**, you paste that address in.

## Options

Environment overrides:

| Variable       | Default                    | Purpose                               |
| -------------- | -------------------------- | ------------------------------------- |
| `TAILCAT_BIN`  | `tailcat`                  | Path to the tailcat binary            |
| `TCT_SSH_KEYS` | `~/.ssh/authorized_keys`   | `authorized_keys` used when serving SSH |

`tctransfer --help` prints the same summary.

## Notes

- It is only a convenience wrapper; it does not replace or bundle `tailcat`.
- `tailcat` makes **no CLI stability guarantees** — if a flag changes upstream,
  the matching line in the script may need a small tweak.
