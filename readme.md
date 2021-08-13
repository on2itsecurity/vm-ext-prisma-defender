# Summary

Script to easily deployment PAN Prisma Cloud Compute Defender on Azure Windows VMs by using the VM Extensions.


## Azure CLI example 

```bash
az vm extension set -n CustomScriptExtension --version "1.10" --publisher Microsoft.Compute --vm-name VM -g RG-ON2IT-SINGLE-VM --settings "{\"fileUris\": [\"https://raw.githubusercontent.com/RobM83/vm-ext-prisma-defender/master/install.ps1\"], \"commandToExecute\": \"powershell.exe -Command ./install.ps1 -url europe-west3.cloud.twistlock.com -tenant eu-123456 -bearer XXXXX\"}"
```

## Terraform

```terraform
resource "azurerm_virtual_machine_extension" "vm_ext" {
  name                 = "DefenderInstall"
  virtual_machine_id   = azurerm_virtual_machine.vm.id
  publisher            = "Microsoft.Compute"
  type                 = "CustomScriptExtension"
  type_handler_version = "1.10"

  settings = <<SETTINGS
    {
        "fileUris": [
          "https://raw.githubusercontent.com/on2itsecurity/vm-ext-prisma-defender/master/install.ps1"
        ],
        "commandToExecute": "powershell.exe -Command ./install.ps1 -url '${var.CONSOLE_FQDN}' -tenant '${var.TENANT}' -bearer '${var.TOKEN}'"
    }
  SETTINGS
}
```