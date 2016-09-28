<properties
    pageTitle="Azure şablonu kullanarak bir Linux VM oluşturma | Microsoft Azure"
    description="Azure Resource Manager şablonu kullanarak Azure’da bir Linux VM oluşturun."
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
    ms.date="08/17/2016"
    ms.author="v-livech"/>


# Bir Azure şablonu kullanarak bir Linux VM oluşturma

Bu makalede, Azure’da bir Azure Şablonu kullanarak hızlı bir şekilde Linux Sanal Makine dağıtma gösterilir.  Bu makale için bir Azure hesabının ([ücretsiz deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/)) yanı sıra, oturum açılmış (`azure login`) ve Resource Manager modunda (`azure config mode arm`) bir [Azure CLI'si](../xplat-cli-install.md) gereklidir.  [Azure portalını](virtual-machines-linux-quick-create-portal.md) veya [Azure CLI'sini](virtual-machines-linux-quick-create-cli.md) kullanarak da Linux VM'yi hızlı bir şekilde dağıtabilirsiniz.

## Hızlı Komut Özeti

```bash
azure group create \
-n quicksecuretemplate \
-l eastus \
--template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

## Ayrıntılı Kılavuz

Şablonlar, kullanıcı adları ve ana bilgisayar adları gibi çalıştırma sırasında özelleştirmek istediğiniz ayarlarla Azure'da VM'ler oluşturmanızı sağlar. Bu makalede; SSH'ye açık, 22 numaralı bağlantı noktasına sahip bir ağ güvenlik grubu (NSG) ile birlikte Ubuntu VM'yi kullanarak bir Azure şablonu başlatıyoruz.

Azure Resource Manager şablonları, bir defalık basit görevler (örneğin, bu makalede yapıldığı gibi bir Ubuntu VM'nin başlatılması) için kullanılabilen JSON dosyalarıdır.  Azure Şablonları; test, geliştirme veya üretim dağıtım yığını gibi tüm ortamların karmaşık Azure yapılandırmalarını oluşturmak için de kullanılabilir.

## Linux VM’i oluşturma

Aşağıdaki kod örneği [bu Azure Resource Manager şablonunu](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json) kullanarak bir kaynak grubu oluşturmak ve aynı zamanda bir SSH güvenlikli Linux VM dağıtmak için `azure group create` çağrısının nasıl gerçekleştireceğini açıklar. Örneğinizde ortamınıza özel adları kullanmanız gerektiğini unutmayın. Bu örnekte kaynak grubu adı olarak `quicksecuretemplate`, VM adı olarak `securelinux` ve alt etki alanı adı olarak `quicksecurelinux` kullanılır.

```bash
azure group create \
-n quicksecuretemplate \
-l eastus \
--template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

Çıktı

```bash
info:    Executing command group create
+ Getting resource group quicksecuretemplate
+ Creating resource group quicksecuretemplate
info:    Created resource group quicksecuretemplate
info:    Supply values for the following parameters
sshKeyData: ssh-rsa AAAAB3Nza<..ssh public key text..>VQgwjNjQ== vlivech@azure
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/<..subid text..>/resourceGroups/quicksecuretemplate
data:    Name:                quicksecuretemplate
data:    Location:            eastus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

Bu örnekte, `--template-uri` parametresi kullanılarak bir VM dağıtıldı.  Ayrıca şablon dosyasının yolu ile birlikte `--template-file` parametresini bağımsız değişken şeklinde kullanarak bir şablonu indirebilir, yerel olarak oluşturabilir ve şablonun geçişini sağlayabilirsiniz. Azure CLI sizden şablon için gerekli parametreleri isteyecektir.

## Sonraki adımlar

Daha sonra dağıtılacak uygulama altyapılarını keşfetmek için [şablon galerisi](https://azure.microsoft.com/documentation/templates/)nde arama yapın.



<!--HONumber=Sep16_HO3-->


