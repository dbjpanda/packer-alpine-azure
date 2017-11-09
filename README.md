# Packer template for Alpine Linux on Hyper-V and Azure

This Packer template will generate a VHD suitable for use in Hyper-V or Azure.

## How it works

- The Packer template downloads the Alpine 3.6 ISO from the official download site.
- It then uses `setup-alpine` to perform an [installation to disk](https://wiki.alpinelinux.org/wiki/Install_to_disk).
- The `answers` file is served using Packer's built-in HTTP server.
- It also installs the `hvtools` package and enables the `hv_kvp_daemon` service so Hyper-V can detect the VM is running and retrieve its IP address. (Read more about [Hyper-V Integration Services](https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services#start-and-stop-an-integration-service-from-a-linux-guest).)

The template does not currently perform any additional provisioning steps.

## How to use the template

The commands need to be run from an elevated PowerShell prompt so that they can interact with Hyper-V.

First run the template. This will generate a VHDX file locally, in `output-hyperv-iso`.

```
packer build alpinehv.json
```

To use the image in Azure, you need to convert the image to VHD using `convert.ps1`.

Finally the `deploy.cmd` script will upload the VHD to Azure and start a VM based on the image. Please amend the script variables as necessary.
