---
title: "VM'ler ve diğer Azure altyapısında sağlayacak Terraform yükleyip | Microsoft Docs"
description: "Yükleyin ve Azure kaynaklarını oluşturmak için Terraform yapılandırma hakkında bilgi edinin"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: echuvyrov
manager: jtalkar
editor: na
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/14/2017
ms.author: echuvyrov
ms.openlocfilehash: da567097be38ac649c6bf1de1508de24d21cb877
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="install-and-configure-terraform-to-provision-vms-and-other-infrastructure-into-azure"></a>Yükleme ve Azure'da VM'ler ve diğer altyapıya sağlamak için Terraform yapılandırma 
Bu makalede yüklemek ve Azure'da sanal makineleri gibi sağlama kaynaklarına Terraform yapılandırmak için gerekli adımları açıklanmaktadır. Oluşturma ve bulut kaynaklarına güvenli bir şekilde sağlamak Terraform etkinleştirmek için Azure kimlik bilgilerini kullanan öğreneceksiniz.

HashiCorp Terraform tanımlamak ve bulut altyapısı HashiCorp yapılandırma dili (HCL) adlı bir özel şablon dili kullanarak dağıtmak için kolay bir yol sağlar. Bu özel dil [yazmak kolay ve kolay anlaşılır](terraform-create-complete-vm.md). Kullanarak Ayrıca, `terraform plan` komutu, görselleştirmek değişiklikleri altyapınız için bunları kullanmadan önce. Azure ile Terraform kullanmaya başlamak için aşağıdaki adımları izleyin.

## <a name="install-terraform"></a>Terraform yükleyin
Terraform, yüklemek için [karşıdan](https://www.terraform.io/downloads.html) bir ayrı bir işletim sistemi için uygun paket yükleme dizini. Yükleme için genel bir yol da tanımlamanız gerekir bir tek bir yürütülebilir dosya içerir. Linux ve Mac'de yolunu ayarlama hakkında daha fazla yönerge için Git [bu Web sayfası](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux). Windows üzerinde yolunu ayarlama hakkında daha fazla yönerge için Git [bu Web sayfası](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows). Kurulumunuzu doğrulamak için çalıştırın `terraform` komutu. Çıkış olarak kullanılabilir Terraform seçeneklerin bir listesini görmelisiniz.

Ardından, altyapı sağlamak için Azure aboneliğinizin Terraform erişmesine izin vermek gerekir.

## <a name="set-up-terraform-access-to-azure"></a>Azure Terraform erişimi ayarlama
Terraform sağlama kaynaklara Azure'da etkinleştirmek için Azure Active Directory (Azure AD) iki varlık oluşturmanız gerekir: Azure AD uygulaması ve bir Azure AD hizmet sorumlusu. Ardından, Terraform komut dosyalarınızı bu varlıkları tanımlayıcılar kullanın. Bir hizmet sorumlusu genel Azure yerel örneğidir AD uygulaması. Bir hizmet sorumlusu Genel kaynaklar için ayrıntılı yerel erişim denetimi sağlar.

Azure AD uygulaması ve bir Azure AD hizmet sorumlusu oluşturmak için birkaç yolu vardır. Kolay ve hızlı şekilde bugün Azure CLI 2.0 kullanmaktır hangi [indirin ve Windows, Linux veya Mac yükleyin](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli). Gerekli güvenlik altyapısı oluşturmak için PowerShell veya Azure CLI 1.0 kullanabilirsiniz. Aşağıdaki yönergelerde Terraform Azure için tüm bu yaklaşım kullanarak yapılandırmak nasıl gösterir.

### <a name="use-azure-cli-20-for-windows-linux-or-mac-users"></a>Azure CLI 2.0 (Windows, Linux veya Mac kullanıcıları için) kullanın 
Karşıdan yükleyip sonra [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli), aşağıdaki komutu gönderdikten Azure aboneliğinizi yönetmek oturum açın:

```
az login
```

>[!NOTE]
>Çin, Azure Almanya veya Azure kamu Bulutlar kullanıyorsanız, önce bu bulut ile çalışmak için Azure CLI yapılandırmanız gerekir. Aşağıdaki komutu çalıştırarak bunu yapabilirsiniz:

```
az cloud set --name AzureChinaCloud|AzureGermanCloud|AzureUSGovernment
```

Birden çok Azure aboneliğiniz varsa, bunların ayrıntıları tarafından döndürülen `az login` komutu. Ayarlama `SUBSCRIPTION_ID` döndürülen değeri tutmak için ortam değişkeni `id` kullanmak istediğiniz abonelikten alan. 

Bu oturum için kullanmak istediğiniz aboneliği ayarlayın.

```
az account set --subscription="${SUBSCRIPTION_ID}"
```

Abonelik kimliği ve Kiracı kimliği değerleri almak için hesap sorgu.

```
az account show --query "{subscriptionId:id, tenantId:tenantId}"
```

Ardından, farklı kimlik bilgileri için Terraform oluşturun.

```
az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/${SUBSCRIPTION_ID}"
```

AppID, parola, sp_name ve Kiracı döndürülür. Uygulama kimliği ve parola not edin.

Kimlik bilgilerinizi (hizmet sorumlusu) onaylamak için yeni bir Kabuğu'nu açın ve aşağıdaki komutları çalıştırın. Sp_name, şifresi ve Kiracı için döndürülen değerleri değiştirin:

```
az login --service-principal -u SP_NAME -p PASSWORD --tenant TENANT
az vm list-sizes --location westus
```

### <a name="use-powershell-for-windows-users"></a>(Windows kullanıcıları için) PowerShell kullanma 
Yazma ve yürütme Terraform komut dosyalarınızı ve yapılandırma görevleri için PowerShell kullanmak için bir Windows makinesine kullanmak için makinenizde sağ PowerShell araçları ile yapılandırın. 

1. İçindeki adımları izleyerek PowerShell araçlarını yükleme [yükleyin ve Azure PowerShell yapılandırma](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps). 

2. İndir ve Yürüt [azure setup.ps1 betik](https://github.com/echuvyrov/terraform101/blob/master/azureSetup.ps1) PowerShell konsolundan.

3. Azure setup.ps1 komut dosyasını çalıştırmak için onu indir ve Yürüt `./azure-setup.ps1 setup` PowerShell konsolundan komutu. Ardından Azure aboneliğinize yönetici ayrıcalıklarıyla oturum açın.

4. Bir uygulama adı (isteğe bağlı dize gerekli) sağlamanız istendiğinde. İsteğe bağlı olarak, istendiğinde güçlü bir parola sağlayın. Bir parola sağlamazsanız, güçlü bir parola sizin için .NET güvenlik kitaplıklarını kullanarak oluşturulur.

### <a name="use-azure-cli-10-for-linux-or-mac-users"></a>Azure CLI 1.0 (Linux veya Mac kullanıcıları için) kullanın
Linux makineler veya Azure CLI 1.0 ile Mac'ler Terraform ile çalışmaya başlamak için uygun kitaplıkları makinenize yükleyin.  

1. İçindeki adımları izleyerek Azure xPlat CLI araçlarını yükleme [Azure CLI 2.0 yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli). 

2. Karşıdan yükleyip JSON işlemci yönergeleri takip ederek [karşıdan jq](https://stedolan.github.io/jq/download/).

3. İndir ve Yürüt [azure setup.sh betik](https://github.com/mitchellh/packer/blob/master/contrib/azure-setup.sh) komut konsolundan bash.

4. Azure setup.sh komut dosyasını çalıştırmak için onu indir ve Yürüt `./azure-setup setup` konsolundan komutu. Ardından Azure aboneliğinize yönetici ayrıcalıklarıyla oturum açın.
 
5. Bir uygulama adı (isteğe bağlı dize gerekli) sağlamanız istendiğinde. İsteğe bağlı olarak, istendiğinde güçlü bir parola sağlayın. Bir parola sağlamazsanız, güçlü bir parola sizin için .NET güvenlik kitaplıklarını kullanarak oluşturulur.

Önceki tüm betikler, bir Azure AD uygulaması ve hizmet sorumlusu oluşturun. Hizmet sorumlusu abonelikte katılımcı veya sahibi düzeyinde erişim alır. Yüksek verilen erişim düzeyi nedeniyle, bu komut dosyaları tarafından üretilen güvenlik bilgileri her zaman korumanız gerekir. Bu komut dosyaları tarafından sağlanan güvenlik bilgileri tüm dört adet Not: AppID, parola, ABONELİK_KİMLİĞİ ve tenant_id.

## <a name="set-environment-variables"></a>Ortam değişkenlerini ayarlama
Oluşturma ve Azure AD hizmet sorumlusu yapılandırdıktan sonra Kiracı kimliği, abonelik kimliği, istemci kimliği ve istemci parolası kullanmak için bilmeniz Terraform izin gerekir. Bu değerleri Terraform komut dosyalarınızı gömerek açıklandığı gibi bunu yapabilirsiniz [Terraform kullanarak temel altyapı oluşturma](terraform-create-complete-vm.md). Alternatif olarak, aşağıdaki ortam değişkenleri ayarlayabilir (ve dolayısıyla yanlışlıkla etme veya kimlik bilgilerinizi paylaşımı kaçının):

- ARM_SUBSCRIPTION_ID
- ARM_CLIENT_ID
- ARM_CLIENT_SECRET
- ARM_TENANT_ID

Bu değişkenleri ayarlamak için bu örnek Kabuk betiği kullanabilirsiniz:

```
#!/bin/sh
echo "Setting environment variables for Terraform"
export ARM_SUBSCRIPTION_ID=your_subscription_id
export ARM_CLIENT_ID=your_appId
export ARM_CLIENT_SECRET=your_password
export ARM_TENANT_ID=your_tenant_id
```

Ayrıca, Azure Çin veya ya da Azure kamu içinde veya Azure Almanya ile Terraform kullanıyorsanız, ortam değişkenine uygun şekilde ayarlamanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar
Şimdi Terraform yükledikten ve Azure aboneliğinize altyapısı dağıtma başlayabilmeniz için Azure kimlik yapılandırılmış. Ardından, bilgi nasıl [Terraform ile altyapı oluşturma](terraform-create-complete-vm.md).
