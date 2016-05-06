# hw-all

# Nix setup

Install `nix`:

    curl https://nixos.org/nix/install | sh

Your `~/.nixpkg/config.nix` needs to look something like this where `$SRC` is the directory where you will clone `hw-all`:

    {
      packageOverrides = super: let self = super.pkgs; in
      {
        haskellPackages = super.haskellPackages.override {
          overrides = self: super: {
            ambiata-mafia = self.callPackage $SRC/hw-all/ambiata-mafia {};
            hw-bits = self.callPackage $SRC/hw-all/hw-bits {};
            hw-conduit = self.callPackage $SRC/hw-all/hw-conduit {};
            hw-diagnostics = self.callPackage $SRC/hw-all/hw-diagnostics {};
            hw-prim = self.callPackage $SRC/hw-all/hw-prim {};
            hw-parser = self.callPackage $SRC/hw-all/hw-parser {};
            hw-rankselect = self.callPackage $SRC/hw-all/hw-rankselect {};
            hw-xml = self.callPackage $SRC/hw-all/hw-xml {};
          };
        };
        haskell = super.haskell // {
          packages = super.haskell.packages // {
            ghc7103 = super.haskell.packages.ghc7103.override {
              overrides = self: super: {
                ambiata-mafia = self.callPackage $SRC/hw-all/ambiata-mafia {};
                hw-bits = self.callPackage $SRC/hw-all/hw-bits {};
                hw-conduit = self.callPackage $SRC/hw-all/hw-conduit {};
                hw-diagnostics = self.callPackage $SRC/hw-all/hw-diagnostics {};
                hw-prim = self.callPackage $SRC/hw-all/hw-prim {};
                hw-parser = self.callPackage $SRC/hw-all/hw-parser {};
                hw-rankselect = self.callPackage $SRC/hw-all/hw-rankselect {};
                hw-xml = self.callPackage $SRC/hw-all/hw-xml {};
              };
            };
          };
        };
      };
    }


# Building hw-json
    git clone git@github.com:haskell-works/hw-all.git
    git submodule init
    git submodule update
    cd hw-json
    git checkout v0.0-branch
    ./scripts/nix-mk
    nix-shell "<nixpkgs>" -A haskellPackages.hw-json.env
    cabal sandbox init
    cabal install --only-dependencies
    cabal build

# Building hw-xml
    git clone git@github.com:haskell-works/hw-all.git
    git submodule init
    git submodule update
    cd hw-xml
    git checkout master
    ./scripts/nix-mk
    nix-shell "<nixpkgs>" -A haskellPackages.hw-xml.env
    cabal sandbox init
    cabal install --only-dependencies
    cabal build
