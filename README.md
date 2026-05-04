# cc-switch Nix Flake

Nix packaging for [CC Switch](https://github.com/farion1231/cc-switch) — All-in-One Assistant for Claude Code, Codex & Gemini CLI.

Packages the pre-built `.deb` from GitHub Releases using `dpkg` + `autoPatchelfHook`. No compilation required.

## Usage

### Run directly

```sh
nix run github:farion1231/cc-switch
```

### Install to profile

```sh
nix profile install github:farion1231/cc-switch
```

### Add to your flake

```nix
{
  inputs = {
    nixpkgs.url = "github:NixOS/nixpkgs/nixpkgs-unstable";
    cc-switch.url = "github:farion1231/cc-switch";
  };

  outputs = { nixpkgs, cc-switch, ... }:
    let
      pkgs = import nixpkgs {
        system = "x86_64-linux";
        overlays = [ cc-switch.overlays.default ];
      };
    in
    {
      # cc-switch is now available as pkgs.cc-switch
      environment.systemPackages = [ pkgs.cc-switch ];
    };
}
```

## Supported Platforms

| Platform | Status |
|----------|--------|
| x86_64-linux | Supported |
| aarch64-linux | Supported |

## Updating

When a new version is released upstream:

1. Update `version` in `flake.nix`
2. Get the new hashes:
   ```sh
   nix-prefetch-url "https://github.com/farion1231/cc-switch/releases/download/v<VERSION>/CC-Switch-v<VERSION>-Linux-x86_64.deb"
   nix-prefetch-url "https://github.com/farion1231/cc-switch/releases/download/v<VERSION>/CC-Switch-v<VERSION>-Linux-arm64.deb"
   ```
3. Convert to SRI format:
   ```sh
   nix hash to-sri --type sha256 <hash>
   ```
4. Update `debHashes` in `flake.nix`
