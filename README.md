# CS-Cart composer installation
This repository allows you to install cs-cart with composer (but still not really elegant as cs-cart is not supporting it natively). You can also checkout https://github.com/drahosistvan/cscart-installer, which is another (unofficial) commandline tool.

# Preparation
1. Point your webserver root for your domain to a folder `web` in this projects root (does not exist yet)
1. If needed, you can create the folder (is a sibling of `patches`, `shared` etc. then) - it will be replaced by a symlink later

# Installation of a fresh cs-cart instance
1. Run `composer update` and wait until it is finished
1. Run `composer init-fs`, now the file-system is prepared (you can check the script `init` in `composer.json` section `"scripts":`)
1. Now you have two installation options:
   1. BROWSER: 
       1. Go to `http(s)://YOURHOST/install` and follow the wizard or
   1. COMMAND-LINE:
       1. Does not work at the moment, check out https://docs.cs-cart.com/4.4.x/install/install_via_console.html

# Installation of add-ons
Console command example to install an add-on which is available already on the server (in folder `packages/ost-loyalty`):
```
# vendor/bin/cscart-sdk addon:symlink [OPTIONAL: --templates-to-design] [ADDON-NAME] [CURRENT-ADDON-PATH] web
vendor/bin/cscart-sdk addon:symlink --templates-to-design ost_loyalty packages/ost-loyalty web
```
The folder could be any folder, also one in `vendor` if you can install some extensions via composer. 
This command symlinks all folders in the addon-path to its corresponding location within the cs-cart structure. 
The command-line tool is officially supported by cs-cart (https://github.com/cscart/sdk).

You can also add the command(s) to `composer.json` and then run them via `composer setup-addons`
```json
  "scripts": {
    "init-fs": [
      "rm -rf web",
      "ln -s vendor/cscart/cscart-ultimate web",
      "chmod 666 web/config.local.php",
      "chmod -R 777 web/design web/images web/var",
      "find web/design -type f -print0 | xargs -0 chmod 666",
      "find web/images -type f -print0 | xargs -0 chmod 666",
      "find web/var -type f -print0 | xargs -0 chmod 666"
    ],
    "setup-addons": [
      "vendor/bin/cscart-sdk addon:symlink --templates-to-design ost_loyalty packages/ost-loyalty web"
    ]
  }
```