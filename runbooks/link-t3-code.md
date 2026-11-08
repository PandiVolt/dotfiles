# Link T3 Code

Link this workstation to the owner's T3 Code to let the owner open and work in its T3 Code environment from their desktop.

## Prerequisites

- The platform owner provides tailnet membership for the workstation and the owner's desktop.
- Apply script 70 installs and starts the workstation's T3 Code service.
- This repository sets `linkedEnvironmentsEnabled` to `true` in `~/.t3/userdata/settings.json`.

## Expose the service

On the workstation, run these commands once to expose the local T3 service on the tailnet:

```bash
sudo tailscale set --operator=$USER
tailscale serve --bg --https=3773 http://127.0.0.1:3773
tailscale serve status
```

Check that the status shows HTTPS on port 3773 forwarding to `http://127.0.0.1:3773`. This is a manual step. Tailscale stores it in its own state, not in this repository. Repeat it after rebuilding the machine.

## Link and pair

1. In the owner's T3 Code, add a linked environment with the address `https://<machine>.<tailnet>.ts.net:3773` and choose full access.
2. On the workstation, run plain `t3 pair`.
3. Enter only the pairing token in the link card before it expires. Never put the token in chat.
4. Check that the link shows reachable in the owner's T3 Code.

The owner's SSH access to this machine belongs in the owner's personal dotfiles. Use password-only authentication without agent forwarding.

## Troubleshooting

- If you see "Nothing at ... answered as a T3 Code environment", check `tailscale serve status`. Start the manual serve on port 3773 and check the address and port in the link card.
- If you see "Linked environments is off on that machine", apply the dotfiles, check `jq .linkedEnvironmentsEnabled ~/.t3/userdata/settings.json` returns `true`, and restart `t3code.service`.
- If you see `TailscaleServeFailedError ... port 443`, do not use `t3 pair --tailscale`. Use the manual serve on port 3773 above and run plain `t3 pair`.
