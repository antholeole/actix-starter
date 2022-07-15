# Actix Web VSC Starter

This is a barebones Actix Web starter project for VSCode.

The only benefit over running `cargo init` yourself is that you get debugging right out the box.

1. Install the [vadimcn.vscode-lldb] extension.
2. Run the VSCode task `backend dev`. Boom! You now get breakpoints and stuff!

Debuggers are underutilized magic and hopefully this makes your life on Actix plenty easier.


### Why it was tricky to setup

To exit a Actix web application, we need to send the process a SIGINT. LLDB always breaks on signals, like SIGINT. This means that you cannot possibly stop the server and auto-reload, since ever time it stops, it'll break. The solution to that was to, on LLDB boot, immediately send it a signal to not break on SIGINT.

The other problem is that hot reloading doesn't work with LLDB, since `cargo watch` literally just produces a new binary, and it already loaded the old one into memory. We have to stop and restart LLDB every time, so we have a `rerun.sh` script that kills the server, and then reinvokes the vscode extension.

Happy Hacking!