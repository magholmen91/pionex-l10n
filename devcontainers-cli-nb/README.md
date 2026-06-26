# Dev Container CLI

Dette arkivet inneholder CLI-en for dev container, som kan ta en devcontainer.json og opprett og konfigurere en dev container fra den.

## Kontekst

En development container lar deg bruke en container som et fullstendig utviklingsmiljø. Den kan brukes til å kjøre en applikasjon, til å separere verktøy, biblioteker eller runtimes som trengs for å arbeide med en eksisterende applikasjon, eller for å kjøre et bygningsystem.

![Diagram of inner and outerloop development with dev containers](/images/dev-container-stages.png)

Denne CLI-en er under aktiv utvikling. Nåværende status:

- [x] `devcontainer build` - Muliggjør bygging/forbygging av bilder
- [x] `devcontainer up` - Starter containere med `devcontainer.json` innstillinger brukt
- [x] `devcontainer run-user-commands` - Kjører livssyklus-kommandoer som `postCreateCommand`
- [x] `devcontainer read-configuration` - Skriver ut nåværende konfigurasjon for arbeidsrom
- [x] `devcontainer exec` - Utfører en kommando i en container med `userEnvProbe`, `remoteUser`, `remoteEnv`, og andre egenskaper brukt
- [x] `devcontainer outdated` - Viser utdaterte låsfilsegenskaper
- [x] `devcontainer upgrade` - Oppgrader låsfilsegenskaper
- [x] `devcontainer features <...>` - Verktøy for å hjelpe til med å forfattere og teste [Dev Container Features](https://containers.dev/implementors/features/)
- [x] `devcontainer templates <...>` - Verktøy for å hjelpe til med å forfattere og teste [Dev Container Templates](https://containers.dev/implementors/templates/)
- [ ] `devcontainer stop` - Stopper containere
- [ ] `devcontainer down` - Stopper og sletter containere

Låsfiler (`.devcontainer-lock.json`) genereres som standard når du kjører `build` eller `up` for å feste egenskapsversjoner for reproduserbare bygg. Bruk `--no-lockfile` for å velge bort, eller `--frozen-lockfile` for å sikre at låsfilen ikke blir oppdatert.

## Prøv det ut

Vi vil gjerne at du prøver ut dev container CLI og gir oss tilbakemeldinger. Du kan raskt prøve det ut i bare noen få enkle trinn, enten ved å bruke installasjonsskriptet eller installere npm-pakken.

### Installasjonsskript

Du kan installere CLI-en med et eget skript som laster ned en buntbar Node.js runtime, så ingen forhåndsinstallert Node.js er nødvendig. Det fungerer på Linux og macOS (x64 og arm64):

```bash
curl -fsSL https://raw.githubusercontent.com/devcontainers/cli/main/scripts/install.sh | sh
```

Legg deretter til installasjonslokalisjonen i PATH:

```bash
export PATH="$HOME/.devcontainers/bin:$PATH"
```

Du kan også spesifisere en versjon, en tilpasset installasjonsmappa, eller oppdatere/avinstallere en eksisterende installasjon:

```bash
# Installer en spesifikk versjon
sh install.sh --version 0.82.0

# Installer til en tilpasset mappa
sh install.sh --prefix ~/.local/devcontainers

# Oppdater til nyeste
sh install.sh --update

# Avinstaller
sh install.sh --uninstall
```

### npm install

For å installere npm-pakken må du ha Python og C/C++ installert for å bygge en av avhengighetene (se f.eks. [her](https://github.com/microsoft/vscode/wiki/How-to-Contribute) for instruksjoner):

```bash
npm install -g @devcontainers/cli
```

Bekreft at du kan kjøre CLI-en og se hjelpeteksten:

```bash
devcontainer <command>

Commands:
  devcontainer up                   Opprett og kjør dev container
  devcontainer build [path]         Bygg et dev container bilde
  devcontainer run-user-commands    Kjør brukerkommunandoer
  devcontainer read-configuration   Les konfigurasjon
  devcontainer features             Funksjoner kommandoer
  devcontainer templates            Maler kommandoer
  devcontainer exec <cmd> [args..]  Utfør en kommando på en kjørende dev container

Options:
  --help     Vis hjelp                                                 [boolean]
  --version  Vis versjonsnummer                                       [boolean]
```

### Prøv CLI-en

Når du har CLI-en, kan du prøve den ut med et eksempelprosjekt, som denne [Rust-eksempelen](https://github.com/microsoft/vscode-remote-try-rust).

Klone Rust-eksempelen til maskinen din, og start en dev container med CLI-ens `up` kommando:

```bash
git clone https://github.com/microsoft/vscode-remote-try-rust
devcontainer up --workspace-folder <path-to-vscode-remote-try-rust>
```

Dette vil laste ned containerbildet fra et containerregister og starte containeren. Din Rust-container skal nå kjøres:

```bash
[88 ms] dev-containers-cli 0.1.0.
[165 ms] Start: Run: docker build -f /home/node/vscode-remote-try-rust/.devcontainer/Dockerfile -t vsc-vscode-remote-try-rust-89420ad7399ba74f55921e49cc3ecfd2 --build-arg VARIANT=bullseye /home/n[...]
[+] Building 0.5s (5/5) FINISHED
 => [internal] load build definition from Dockerfile                       0.0s
 => => transferring dockerfile: 38B                                        0.0s
 => [internal] load .dockerignore                                          0.0s
 => => transferring context: 2B                                            0.0s
 => [internal] load metadata for mcr.microsoft.com/vscode/devcontainers/r  0.4s
 => CACHED [1/1] FROM mcr.microsoft.com/vscode/devcontainers/rust:1-bulls  0.0s
 => exporting to image                                                     0.0s
 => => exporting layers                                                    0.0s
 => => writing image sha256:39873ccb81e6fb613975e11e37438eee1d49c963a436d  0.0s
 => => naming to docker.io/library/vsc-vscode-remote-try-rust-89420ad7399  0.0s
[1640 ms] Start: Run: docker run --sig-proxy=false -a STDOUT -a STDERR --mount type=bind,source=/home/node/vscode-remote-try-rust,target=/workspaces/vscode-remote-try-rust -l devcontainer.local_f[...]
Container started
{"outcome":"success","containerId":"f0a055ff056c1c1bb99cc09930efbf3a0437c54d9b4644695aa23c1d57b4bd11","remoteUser":"vscode","remoteWorkspaceFolder":"/workspaces/vscode-remote-try-rust"}
```

Du kan deretter kjøre kommandoer i denne dev containeren:

```bash
devcontainer exec --workspace-folder <path-to-vscode-remote-try-rust> cargo run
```

Dette vil kompilere og kjøre Rust-eksempelen, og gi ut:

```bash
[33 ms] dev-containers-cli 0.1.0.
   Compiling hello_remote_world v0.1.0 (/workspaces/vscode-remote-try-rust)
    Finished dev [unoptimized + debuginfo] target(s) in 1.06s
     Running `target/debug/hello_remote_world`
Hello, VS Code Remote - Containers!
{"outcome":"success"}
```

Gratulerer, du har nettopp kjørt dev container CLI og sett den i aksjon!

## Flere CLI-eksempler

Mappen [example-usage](./example-usage) inneholder noen enkle shell-skript for å illustrere hvordan CLI-en kan brukes til:

- Injiser verktøy for bruk inne i en utviklingscontainer
- Bruk en dev container som ditt CI-byggmiljø for å bygge en applikasjon (selv om det ikke distribueres som en container)
- Bygg et containerbildet fra en devcontainer.json fil som inkluderer [dev container-funksjoner](https://containers.dev/implementors/features/#devcontainer-json-properties)

## Bygg fra kilder

Dette arkivet har en [dev container-konfigurasjon](https://github.com/devcontainers/cli/tree/main/.devcontainer), som du kan bruke for å sikre at du har riktig avhengigheter installert.

Kompiler CLI-en med yarn:
```sh
yarn
yarn compile
```

Bekreft at du kan kjøre CLI-en og se hjelpeteksten:
```sh
node devcontainer.js --help
```

## Spesifikasjon

Dev container CLI er en del av [Development Containers Specification](https://github.com/devcontainers/spec). Denne spesifikasjonen søker å finne måter å berike eksisterende formater med vanlige utviklingscontainer-definisjonsfunksjoner.

Les mer på [dev container spesifikasjon-nettstedet](https://devcontainers.github.io/).

## Tilleggsressurser

Du kan gjennomgå andre ressurser som er en del av spesifikasjonen i [`devcontainers` GitHub-organisasjonen](https://github.com/devcontainers).

### Dokumentasjon

- Ytterligere informasjon om bruk av den innebygde [Features testing command](./docs/features/test.md).

## Bidrag

Se hvordan du bidrar til CLI-en i [CONTRIBUTING.md](CONTRIBUTING.md).

## Lisens

Dette prosjektet er under en [MIT-lisens](LICENSE.txt).