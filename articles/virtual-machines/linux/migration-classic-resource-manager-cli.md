---
title: "Azure CLI kullanarak kaynak yöneticisi Vm'leri geçirme | Microsoft Docs"
description: "Bu makalede kaynakları platform desteklenen geçiş Klasikten Azure Resource Manager'da Azure CLI kullanarak anlatılmaktadır"
services: virtual-machines-linux
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d6f5a877-05b6-4127-a545-3f5bede4e479
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: kasing
ms.openlocfilehash: 607ab59dbeb414c69a6272d0aeb00299296bca6a
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2017
---
# <a name="migrate-iaas-resources-from-classic-to-azure-resource-manager-by-using-azure-cli"></a>Iaas kaynaklarını Klasikten Azure Resource Manager'da Azure CLI kullanarak geçirme
Bu adımlar Azure komut satırı arabirimi (CLI) komutları altyapı Klasik dağıtım modeli hizmet (Iaas) kaynaklardan Azure Resource Manager dağıtım modeline olarak geçirmek için nasıl kullanılacağını gösterir. Makale gerektirir [Azure CLI 1.0](../../cli-install-nodejs.md). Azure CLI 2.0 yalnızca Azure Resource Manager kaynakları için geçerli olduğundan, bu geçiş için kullanılamaz.

> [!NOTE]
> Burada açıklanan tüm ıdempotent işlemleridir. Desteklenmeyen bir özellik veya bir yapılandırma hatası başka bir sorun varsa, hazırlama yeniden deneme öneririz, veya tamamlanmaya işlemi. Platform sonra eylemi yeniden deneyecek.
> 
> 

<br>
İşte adımları geçiş işlemi sırasında yürütülmesi gerekir siparişi tanımlamak için bir akış çizelgesi

![Geçiş adımlarını gösteren ekran görüntüsü](../windows/media/migration-classic-resource-manager/migration-flow.png)

## <a name="step-1-prepare-for-migration"></a>1. adım: geçiş için hazırlama
Geçirme Iaas kaynaklardan Klasik Kaynak Yöneticisi değerlendirirken öneririz birkaç en iyi uygulamalar şunlardır:

* Okuyun [desteklenmeyen yapılandırmaları veya özellikleri listesi](../windows/migration-classic-resource-manager-overview.md). Desteklenmeyen yapılandırmaları veya özellikleri kullanan sanal makineler varsa, duyurdu özellik/yapılandırma desteği beklemenizi öneririz. Alternatif olarak, bu özelliği kaldırmak veya gereksinimlerinize uygun değilse, geçiş etkinleştirmek için bu yapılandırma dışında taşıyın.
* Bugün, altyapı ve uygulamalarınızı dağıtma komut otomatik değilse, geçiş için bu komut dosyalarını kullanarak benzer bir test Kurulum oluşturmayı deneyin. Alternatif olarak, Azure portalını kullanarak örnek ortamları ayarlayabilirsiniz.

> [!IMPORTANT]
> Uygulama ağ geçitleri, Klasik geçiş için kaynak yöneticisi için şu anda desteklenmemektedir. Bir uygulama ağ geçidi Klasik sanal ağla geçirmek için ağ taşımak için bir hazırlama işlemi çalıştırmadan önce ağ geçidi kaldırın. Geçişi tamamladıktan sonra Azure Kaynak Yöneticisi'nde ağ geçidi yeniden bağlanın. 
>
>ExpressRoute ağ geçidi başka bir Abonelikteki ExpressRoute bağlantı hatları bağlanma otomatik olarak geçirilemez. Böyle durumlarda, ExpressRoute ağ geçidi kaldırın, sanal ağ geçirmek ve ağ geçidi oluşturun. Lütfen bakın [geçirmek ExpressRoute bağlantı hattına ve Resource Manager dağıtım modeli için Klasik sanal ağlar ilişkili](../../expressroute/expressroute-migration-classic-resource-manager.md) daha fazla bilgi için.
> 
> 

## <a name="step-2-set-your-subscription-and-register-the-provider"></a>2. adım: aboneliğinizi ayarlamak ve sağlayıcısını Kaydet
Geçiş senaryoları için her iki Klasik ortamınızı ayarlamanız gerekir ve Resource Manager. [Azure CLI yükleme](../../cli-install-nodejs.md) ve [aboneliğinizi seçin](/cli/azure/authenticate-azure-cli).

Oturum hesabınıza açın.

    azure login

Aşağıdaki komutu kullanarak Azure aboneliğini seçin.

    azure account set "<azure-subscription-name>"

> [!NOTE]
> Kayıt olan bir zaman adım ancak kez geçiş işlemini denemeden önce yapılması gerekir. Kaydettirmeden aşağıdaki hata iletisini görürsünüz 
> 
> *BadRequest: Abonelik geçiş için kayıtlı değil.* 
> 
> 

Geçiş kaynak sağlayıcısı ile aşağıdaki komutu kullanarak kaydedin. Bazı durumlarda, bu komut zaman aşımına uğruyor olduğunu unutmayın. Ancak, kayıt başarılı olur.

    azure provider register Microsoft.ClassicInfrastructureMigrate

Kaydın son beş dakika bekleyin. Aşağıdaki komutu kullanarak onay durumunu kontrol edebilirsiniz. RegistrationState olduğundan emin olun `Registered` devam etmeden önce.

    azure provider show Microsoft.ClassicInfrastructureMigrate

Şimdi için CLI geçiş `asm` modu.

    azure config mode asm

## <a name="step-3-make-sure-you-have-enough-azure-resource-manager-virtual-machine-vcpus-in-the-azure-region-of-your-current-deployment-or-vnet"></a>3. adım: yeterli Azure Resource Manager sanal makine Vcpu'lar geçerli dağıtım veya VNET Azure bölgesinde olduğundan emin olun
Bu adım için geçiş gerekir `arm` modu. Aşağıdaki komutla bunu.

```
azure config mode arm
```

Azure Kaynak Yöneticisi'nde sahip Vcpu'lar geçerli sayısını denetlemek için aşağıdaki CLI komutu kullanabilirsiniz. VCPU kotaları hakkında daha fazla bilgi için bkz: [sınırları ve Azure Resource Manager](../../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager)

```
azure vm list-usage -l "<Your VNET or Deployment's Azure region"
```

İşiniz bittiğinde bu adımı doğrulanıyor, size geri dönebilirsiniz `asm` modu.

    azure config mode asm


## <a name="step-4-option-1---migrate-virtual-machines-in-a-cloud-service"></a>4. adım: 1. seçenek - sanal makineler bir bulut hizmetinde geçirme
Aşağıdaki komutu kullanarak bulut Hizmetleri listesini almak ve geçirmek istediğiniz bulut hizmeti seçin. Bulut hizmetindeki sanal makineleri bir sanal ağ veya web/çalışan rolleri varsa, bir hata iletisi alırsınız unutmayın.

    azure service list

Bulut hizmeti için dağıtım adı ayrıntılı çıktısını almak için aşağıdaki komutu çalıştırın. Çoğu durumda, dağıtım adı bulut hizmet adı ile aynıdır.

    azure service show <serviceName> -vv

İlk olarak, aşağıdaki komutları kullanarak bulut hizmeti geçirirseniz doğrulama:

```shell
azure service deployment validate-migration <serviceName> <deploymentName> new "" "" ""
```

Sanal makine bulut hizmeti geçiş için hazırlayın. Aralarından seçim yapabileceğiniz iki seçeneğiniz vardır.

Platform tarafından oluşturulan bir sanal ağ sanal makineleri geçirmek istiyorsanız, aşağıdaki komutu kullanın.

    azure service deployment prepare-migration <serviceName> <deploymentName> new "" "" ""

Resource Manager dağıtım modelinde var olan bir sanal ağa geçirmek istiyorsanız, aşağıdaki komutu kullanın.

    azure service deployment prepare-migration <serviceName> <deploymentName> existing <destinationVNETResourceGroupName> <subnetName> <vnetName>

Hazırlama işlemi başarılı olduktan sonra sanal makineleri geçiş durumunu alma ve içinde olduklarından emin olmak için ayrıntılı çıktı aracılığıyla bakabilirsiniz `Prepared` durumu.

    azure vm show <vmName> -vv

CLI veya Azure portalını kullanarak hazırlanmış kaynakların yapılandırmasını denetleyin. Geçiş için hazır olmayan ve eski durumuna geri dönmek istiyorsanız aşağıdaki komutu kullanın.

    azure service deployment abort-migration <serviceName> <deploymentName>

Hazırlanan yapılandırma iyi görünüyorsa, ilerlemek ve aşağıdaki komutu kullanarak kaynakları uygulayın.

    azure service deployment commit-migration <serviceName> <deploymentName>



## <a name="step-4-option-2----migrate-virtual-machines-in-a-virtual-network"></a>4. adım: 2. seçenek - bir sanal ağdaki sanal makineleri geçirme
Geçirmek istediğiniz sanal ağ seçin. Sanal ağ ile desteklenmeyen yapılandırmalar web/çalışan rolleri veya VM'ler içeriyorsa, bir doğrulama hata iletisi alırsınız unutmayın.

Tüm sanal ağları, aşağıdaki komutu kullanarak abonelikte alın.

    azure network vnet list

Çıktı aşağıdakine benzer görünecektir:

![Vurgulanan tüm sanal ağ adıyla komut satırının ekran görüntüsü.](../media/virtual-machines-linux-cli-migration-classic-resource-manager/vnet.png)

Yukarıdaki örnekte, **virtualNetworkName** tüm adı **"Grubu classicubuntu16 classicubuntu16"**.

İlk olarak, aşağıdaki komutu kullanarak sanal ağ geçirirseniz doğrulama:

```shell
azure network vnet validate-migration <virtualNetworkName>
```

Seçtiğiniz sanal ağ, aşağıdaki komutu kullanarak geçiş için hazırlayın.

    azure network vnet prepare-migration <virtualNetworkName>

CLI veya Azure portalını kullanarak hazırlanmış sanal makineler için yapılandırmayı denetleyin. Geçiş için hazır olmayan ve eski durumuna geri dönmek istiyorsanız aşağıdaki komutu kullanın.

    azure network vnet abort-migration <virtualNetworkName>

Hazırlanan yapılandırma iyi görünüyorsa, ilerlemek ve aşağıdaki komutu kullanarak kaynakları uygulayın.

    azure network vnet commit-migration <virtualNetworkName>

## <a name="step-5-migrate-a-storage-account"></a>5. adım: bir depolama hesabını geçirin
İşiniz bittiğinde sanal makineleri geçiriyorsanız, depolama hesabı geçir öneririz.

Aşağıdaki komutu kullanarak depolama hesabı geçiş için hazırlama

    azure storage account prepare-migration <storageAccountName>

CLI veya Azure portalını kullanarak hazırlanmış depolama hesabı için yapılandırmasını denetleyin. Geçiş için hazır olmayan ve eski durumuna geri dönmek istiyorsanız aşağıdaki komutu kullanın.

    azure storage account abort-migration <storageAccountName>

Hazırlanan yapılandırma iyi görünüyorsa, ilerlemek ve aşağıdaki komutu kullanarak kaynakları uygulayın.

    azure storage account commit-migration <storageAccountName>

## <a name="next-steps"></a>Sonraki adımlar

* [Platform desteklenen geçişi Iaas Klasik kaynaklardan Azure Resource Manager'a genel bakış](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Teknik ya ilişkin ayrıntılar platform desteklenen geçiş Klasik'ten Azure Kaynak Yöneticisi](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [IaaS kaynaklarının Klasik’ten Azure Resource Manager’a geçişini planlama](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Iaas kaynaklarına Klasikten Azure Resource Manager geçirmek için PowerShell kullanma](../windows/migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Iaas Klasik kaynaklardan Azure Resource Manager için geçiş ile Yardım için topluluk araçları](../windows/migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [En sık karşılaşılan geçiş hatalarını gözden geçirme](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Gözden geçirme Iaas kaynaklardan Klasik Azure Kaynak Yöneticisi hakkında en sık sorulan sorular](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
