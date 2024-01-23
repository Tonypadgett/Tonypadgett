[#GORILLA TAG MOD]
| Action                                      |   Shortcut
 | ------------------------------------------- |:-----------------------------
 | Switch fullscreen mode                      | <kbd>MOD</kbd>+<kbd>f</kbd>
 | -------------------------------------------| :-----------------------------

    <%(8) [FirstMod FirstModFLY]>
  <%(9) [FirstMod secondModFLY]>
<%(10) [FirstMod thirdModFLY]>
data "coder_workspace" "me" {}

module "coder-base" {
  source = "github.com/my-organization/coder-base"

  # Modules take in variables and can provision infrastructure
  vpc_name            = "devex-3"
  subnet_tags         = { "name": data.coder_workspace.me.name }
  code_server_version = 4.14.1
}

resource "coder_agent" "dev" {
  # Modules can provide outputs, such as helper scripts
  startup_script=<<EOF
  #!/bin/sh
  ${module.coder-base.code_server_install_command}
  EOF
}
