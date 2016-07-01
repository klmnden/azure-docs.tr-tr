<properties
    pageTitle="Azure şablonu kullanarak Güvenli bir Linux VM oluşturma | Microsoft Azure"
    description="Azure Resource Manager şablonu kullanarak Azure’da Güvenli bir Linux VM oluşturun."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="04/27/2016"
    ms.author="v-livech"/>

# Bir Azure şablonu kullanarak güvenli bir Linux VM oluşturma

Bir şablondan bir Linux VM oluşturmak için [Azure CLI](../xplat-cli-install.md)’yı kaynak yöneticisi modunda (`azure config mode arm`) kullanmanız gerekir.

## Hızlı Komut Özeti

```bash
chrisl@fedora$ azure group create -n <exampleRGname> -l <exampleAzureRegion> [--template-uri <URL> | --template-file <path> | <template-file> -e <parameters.json file>]
```

## Ayrıntılı Kılavuz

Şablonlar, kullanıcı adları ve ana bilgisayar adları gibi çalıştırma sırasında özelleştirmek istediğiniz ayarlarla Azure’da VM’ler oluşturmanızı sağlar. Bu makale için, yalnızca bir bağlantı noktası açık (SSH için 22) bir ağ güvenlik grubu (NG) oluşturan ve oturum açmak için SSH anahtarları gerektiren bir Ubuntu VM başlatmaya odaklanacağız.

Azure Resource Manager şablonları örneğin bu makalede olduğu gibi bir Ubuntu VM başlatmak gibi tek seferlik görevler için veya İşletim Sistemine ağ oluşturmadan uygulama yığın dağıtımına kadar test, geliştirme veya üretim geliştirme gibi eksiksiz ortamların Azure yapılandırmalarını oluşturmak üzere kullanılan JSON dosyalarıdır.

## Linux VM’i oluşturma

Aşağıdaki kod örneği [bu Azure Resource Manager şablonunu](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json) kullanarak bir kaynak grubu oluşturmak ve aynı zamanda bir SSH güvenlikli Linux VM dağıtmak için `azure group create` çağrısının nasıl gerçekleştireceğini açıklar. Örneğinizde ortamınıza özel adları kullanmanız gerektiğini unutmayın. Bu örnekte kaynak grubu adı olarak `quicksecuretemplate`, VM adı olarak `securelinux` ve alt etki alanı adı olarak `quicksecurelinux` kullanılır.

```bash
chrisl@fedora$ azure group create -n quicksecuretemplate -l eastus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
info:    Executing command group create
+ Getting resource group quicksecuretemplate
+ Creating resource group quicksecuretemplate
info:    Created resource group quicksecuretemplate
info:    Supply values for the following parameters
adminUserName: ops
sshKeyData: ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDDRZ/XB8p8uXMqgI8EoN3dWQw... user@contoso.com
dnsLabelPrefix: quicksecurelinux
vmName: securelinux
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/<guid>/resourceGroups/quicksecuretemplate
data:    Name:                quicksecuretemplate
data:    Location:            eastus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

`--template-uri` parametresini kullanarak yeni bir kaynak grubu oluşturabilir ve bir VM dağıtabilirsiniz veya bir şablon indirebilir ya da yerel olarak oluşturabilir ve `--template-file` parametresini şablon dosyasına bir bağımsız değişken olarak yönlendirerek kullanıp şablonu geçirebilirsiniz. Azure CLI sizden şablon için gerekli parametreleri isteyecektir.

## Sonraki adımlar

Şablonlarla Linux VM’lerinizi oluşturduktan sonra, şablonlarla kullanılmaya uygun diğer uygulama altyapılarını görmek isteyebilirsiniz. Daha sonra dağıtılacak uygulama altyapılarını keşfetmek için [şablon galerisi](https://azure.microsoft.com/documentation/templates/)nde arama yapın.



<!--HONumber=Jun16_HO2-->


