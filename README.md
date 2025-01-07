# Boulette - A terminal confirmation prompt.

Prevents you from accidentally shutting down remote hosts.

If you've ever finished a late night codding session
by typing "shutdown -h now" in the wrong terminal.

## Usage

Type `boulette` before a critical command and a confirmation prompt will show up.

```sh
boulette "shutdown -h now"
```

![boulette prompt](https://github.com/pipelight/boulette/blob/main/public/images/ask_challenge.png)

### Ssh Only

Boulette confirmation prompt can be triggerd inside **ssh session only** thanks to the `--ssh-only` option.

```sh
boulette "shutdown -h now" --ssh-only
```

### Challenges

In order to execute the provided command you can choose between 2 challenges to be resolved:

- ask, with `--challenge ask` so you have to type 'y' or 'n' to resume commande execution.

  ![boulette prompt](https://github.com/pipelight/boulette/blob/main/public/images/ask_challenge.png)

- hostname, with `--challenge hostname` so you must type the host name to resume command execution.

  ![boulette prompt](https://github.com/pipelight/boulette/blob/main/public/images/hostname_challenge.png)

### Aliases

The idea is to enforce a prompt on your most dangerous commands.
We can do so by creating aliases of those commands and prefix them with boulette.

For example, setting the following alias, will prompt you whenever you type `shutdown -h now`.

```sh
alias off='boulette "shutdown"'
```

`shutdown` becomes `boulette shutdown`.

Here are the ones I use the most frequently.

```sh
alias off='boulette "shutdown -h now" --ssh-only --challenge hostname'
alias sus='boulette "systemctl suspend" --ssh-only --challenge hostname'
alias reboot='boulette reboot --ssh-only --challenge hostname'
```

You can also prefix every shutdown commands wit boulette.
This way `shutdown` and `shutdown -h now` will both require confirmation.

- bash/zsh

```sh
shutdown () {
  boulette "shutdown $argv" --ssh-only --challenge hostname
}
```

- fish

```fish
function shutdown;
  boulette "shutdown $argv" --ssh-only --challenge hostname
end
```

You can display a usefull help message with minimal examples.

```sh
boulette --help
```

## Install

with cargo:

```sh
cargo install --git https://github.com/pipelight/boulette

```

with nix(flakes):

```sh
nix-shell -p https://github.com/pipelight/boulette

```

Greatly inspired by [Molly-guard](https://salsa.debian.org/debian/molly-guard).
