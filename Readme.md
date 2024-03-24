# Usage

Get a shell with [SST ion](https://github.com/sst/ion):

```sh
$ nix shell github:sellooh/sst-flake -c $SHELL
```

Install in your nix profile:

```sh
$ nix profile install github:sellooh/sst-flake
```

Make your own flake, you can use this as a starting point:

Example:
```nix
{
  inputs = {
    nixpkgs.url = "github:nixos/nixpkgs/nixos-unstable";
    flake-utils.url = "github:numtide/flake-utils";
    sst-input = {
      url = "github:sellooh/sst-flake/main";
      inputs.nixpkgs.follows = "nixpkgs";
      inputs.flake-utils.follows = "flake-utils";
    };
  };

  outputs = {
    self,
    nixpkgs,
    flake-utils,
    sst-input,
    ...
  }: flake-utils.lib.eachDefaultSystem(system:
    let
      pkgs = import nixpkgs {
        inherit system;
      };
      sst = sst-input.packages.${system}.default;
    in
    with pkgs;
    {
      devShells.default = mkShell {
        shellHook = ''
          # Add your commands
        '';
        buildInputs = [
          # Dev Tools
          git

          # AWS
          awscli2
          awsume

          # SST
          sst

          # Node
          nodejs-18_x
        ];
      };
    }
  );
}
```
