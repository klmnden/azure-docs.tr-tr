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
    ms.date="04/29/2016"
    ms.author="v-livech"/>

# Bir Azure şablonu kullanarak bir Linux VM oluşturma

Bu makalede, Azure’da bir Azure Şablonu kullanarak hızlı bir şekilde Linux Sanal Makine dağıtma gösterilir.  Makale bir Azure hesabı ([ücretsiz bir deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/)) ve oturum açılmış (`azure login`) ve kaynak yöneticisi modunda (`azure config mode arm`) [Azure CLI](../xplat-cli-install.md) ve gerektirir.  Ayrıca bir [Azure Portal](virtual-machines-linux-quick-create-portal.md) veya [Azure CLI](virtual-machines-linux-quick-create-cli.md) kullanarak bir Linux VM dağıtabilirsiniz.


## Hızlı Komut

Aşağıdaki komut örneklerinde, &lt; ile &gt; arasındaki değerleri kendi ortamınızdaki değerlerle değiştirin.

```bash
azure group create \
-n quicksecuretemplate \
-l eastus \
--template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

## Ayrıntılı Kılavuz

Şablonlar, kullanıcı adları ve ana bilgisayar adları gibi çalıştırma sırasında özelleştirmek istediğiniz ayarlarla Azure’da VM’ler oluşturmanızı sağlar. Bu makale için, yalnızca bir bağlantı noktası açık (SSH için 22) bir ağ güvenlik grubu (NG) oluşturan ve oturum açmak için SSH anahtarları gerektiren bir Ubuntu VM başlatmaya odaklanacağız.

Azure Resource Manager şablonları örneğin bu makalede olduğu gibi bir Ubuntu VM başlatmak gibi tek seferlik görevler için veya İşletim Sistemine ağ oluşturmadan uygulama yığın dağıtımına kadar test, geliştirme veya üretim geliştirme gibi eksiksiz ortamların Azure yapılandırmalarını oluşturmak üzere kullanılan JSON dosyalarıdır.

## Linux VM’i oluşturma

Aşağıdaki kod örneği [bu Azure Resource Manager şablonunu](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json) kullanarak bir kaynak grubu oluşturmak ve aynı zamanda bir SSH güvenlikli Linux VM dağıtmak için `azure group create` çağrısının nasıl gerçekleştireceğini açıklar. Örneğinizde ortamınıza özel adları kullanmanız gerektiğini unutmayın. Bu örnekte kaynak grubu adı olarak `quicksecuretemplate`, VM adı olarak `securelinux` ve alt etki alanı adı olarak `quicksecurelinux` kullanılır.

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

`--template-uri` parametresini kullanarak yeni bir kaynak grubu oluşturabilir ve bir VM dağıtabilirsiniz veya bir şablon indirebilir ya da yerel olarak oluşturabilir ve `--template-file` parametresini şablon dosyasına bir bağımsız değişken olarak yönlendirerek kullanıp şablonu geçirebilirsiniz. Azure CLI sizden şablon için gerekli parametreleri isteyecektir.

## Sonraki adımlar

Şablonlarla Linux VM’lerinizi oluşturduktan sonra, şablonlarla dağıtılmaya uygun diğer uygulama altyapılarını görmek isteyebilirsiniz. Daha sonra dağıtılacak uygulama altyapılarını keşfetmek için [şablon galerisi](https://azure.microsoft.com/documentation/templates/)nde arama yapın.



<!--HONumber=Jun16_HO2-->


