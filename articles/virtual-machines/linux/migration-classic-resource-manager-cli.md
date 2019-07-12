---
title: Vm'leri Azure CLI kullanarak Resource Manager'a geçirme | Microsoft Docs
description: Bu makalede kaynakları platform destekli geçiş Klasik modelden Azure Resource Manager'da Azure CLI kullanarak gösterilmektedir
services: virtual-machines-linux
documentationcenter: ''
author: singhkays
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: d6f5a877-05b6-4127-a545-3f5bede4e479
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: kasing
ms.openlocfilehash: 0e21a962fb03a42af4cb32fcdf60cd59746a591d
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67667375"
---
# <a name="migrate-iaas-resources-from-classic-to-azure-resource-manager-by-using-azure-cli"></a>Azure CLI kullanarak Iaas kaynaklarını Klasik modelden Azure Resource Manager'a geçiş
Bu adımları olarak hizmet (Iaas) kaynaklarını Klasik dağıtım modelinden Azure Resource Manager dağıtım modeline altyapı geçirmek için Azure komut satırı arabirimi (CLI) komutlarını kullanmayı gösterir. Makale gerekir [Azure Klasik CLI](../../cli-install-nodejs.md). Azure CLI yalnızca Azure Resource Manager kaynakları için geçerli olduğundan, bu geçiş için kullanılamaz.

> [!NOTE]
> Burada açıklanan tüm işlemler bir kere etkili olur. Desteklenmeyen bir özellik veya bir yapılandırma hatası dışında bir sorun varsa, hazırlama, yeniden deneme öneririz durdurma veya işlemeyi işlemi. Platform, ardından işlemi yeniden deneyecek.
> 
> 

<br>
İşte adımları bir geçiş işlemi sırasında yürütülmek üzere gereken sırayı tanımlamak için bir akış çizelgesi

![Geçiş adımlarını gösteren ekran görüntüsü](../windows/media/migration-classic-resource-manager/migration-flow.png)

## <a name="step-1-prepare-for-migration"></a>1\. adım: Geçiş için hazırlanma
Resource Manager'a geçirme Iaas kaynaklarını Klasik portaldan değerlendirirken öneririz birkaç en iyi uygulamalar şunlardır:

* Okumak [desteklenmeyen yapılandırmalar veya özellik listesini](../windows/migration-classic-resource-manager-overview.md). Desteklenmeyen yapılandırmalar veya özellikleri kullanan sanal makineler varsa, özelliği/yapılandırma desteği Duyurulacak beklemenizi öneririz. Alternatif olarak, bu özelliği kaldırın veya bu yapılandırma dışında gereksinimlerinize uygun değilse, geçiş etkinleştirmeniz taşıyın.
* Altyapınızı ve uygulamalarınızı dağıtmak, bugün komut otomatik değilse, geçiş için bu komut dosyalarını kullanarak benzer bir test ayarı oluşturmayı deneyin. Alternatif olarak, Azure portalını kullanarak örnek ortamlarını ayarlayabilirsiniz.

> [!IMPORTANT]
> Uygulama ağ geçitleri, Resource Manager'a geçiş Klasik modelden için şu anda desteklenmemektedir. Bir uygulama ağ geçidi ile bir Klasik sanal ağ'ı geçirmek için ağ taşımak için bir hazırlama işlemi çalıştırmadan önce ağ geçidini kaldırın. Azure Resource Manager'daki ağ geçidi geçişi tamamlandıktan sonra yeniden bağlanın. 
>
>ExpressRoute ağ geçitleri başka bir Abonelikteki ExpressRoute devrelerine bağlama otomatik olarak geçirilemez. Böyle durumlarda, ExpressRoute ağ geçidini kaldırmak, sanal ağı geçirme ve ağ geçidini yeniden oluşturun. Lütfen [geçirme ExpressRoute devrelerini ve ilişkili sanal ağları Klasikten Resource Manager dağıtım modeline](../../expressroute/expressroute-migration-classic-resource-manager.md) daha fazla bilgi için.
> 
> 

## <a name="step-2-set-your-subscription-and-register-the-provider"></a>2\. adım: Sağlayıcıyı kaydetmek ve aboneliğinizi ayarlama
Geçiş senaryoları için her iki Klasik ortamınızı kurmanız gerekir ve Resource Manager. [Azure CLI'yı yükleme](../../cli-install-nodejs.md) ve [aboneliğinizi seçin](/cli/azure/authenticate-azure-cli).

Hesabınızda oturum.

    azure login

Aşağıdaki komutu kullanarak Azure aboneliğini seçin.

    azure account set "<azure-subscription-name>"

> [!NOTE]
> Kayıt olan bir zaman adımı ancak geçişi denemeden önce bir kez gerçekleştirilmesi gerekir. Kayıt olmadan aşağıdaki hata iletisini görürsünüz 
> 
> *BadRequest: Abonelik geçiş için kayıtlı değil.* 
> 
> 

Aşağıdaki komutu kullanarak geçiş kaynak sağlayıcısı ile kaydedin. Bazı durumlarda, bu komut zaman aşımına olduğunu unutmayın. Ancak, kayıt başarılı olur.

    azure provider register Microsoft.ClassicInfrastructureMigrate

Lütfen kaydı tamamlamak beş dakika bekleyin. Aşağıdaki komutu kullanarak onay durumunu kontrol edebilirsiniz. RegistrationState olduğundan emin olun `Registered` devam etmeden önce.

    azure provider show Microsoft.ClassicInfrastructureMigrate

Şimdi CLI sürümüne geçmek `asm` modu.

    azure config mode asm

## <a name="step-3-make-sure-you-have-enough-azure-resource-manager-virtual-machine-vcpus-in-the-azure-region-of-your-current-deployment-or-vnet"></a>3\. adım: Azure Resource Manager sanal makinesine yeterli Vcpu geçerli dağıtım veya VNET Azure bölgesinde olduğundan emin olun
Bu adım için geçiş gerekecektir `arm` modu. Aşağıdaki komutla bunu.

```
azure config mode arm
```

Azure Resource Manager'da sahip Vcpu geçerli sayısını denetlemek için aşağıdaki CLI komutunu kullanabilirsiniz. VCPU kotaları hakkında daha fazla bilgi için bkz: [sınırlarını ve Azure Resource Manager](../../azure-subscription-service-limits.md#limits-and-azure-resource-manager)

```
azure vm list-usage -l "<Your VNET or Deployment's Azure region"
```

İşiniz bittiğinde bu adımı doğrulama geri geçiş yapabilirsiniz `asm` modu.

    azure config mode asm


## <a name="step-4-option-1---migrate-virtual-machines-in-a-cloud-service"></a>4\. Adım: 1. seçenek - bulut hizmetindeki sanal makineleri geçirme
Aşağıdaki komutu kullanarak bulut Hizmetleri'nın listesini alın ve ardından geçirmek istediğiniz bulut hizmeti seçin. Bulut hizmetindeki sanal makineleri bir sanal ağda veya sahip oldukları web/çalışan rolleri, bir hata iletisi alırsınız unutmayın.

    azure service list

Bulut hizmeti için dağıtım adı ayrıntılı çıktısını almak için aşağıdaki komutu çalıştırın. Çoğu durumda, dağıtım adı bulut hizmeti adı ile aynıdır.

    azure service show <serviceName> -vv

Aşağıdaki komutlar kullanılarak bulut hizmetine geçirebilirsiniz, ilk olarak, doğrulama:

```shell
azure service deployment validate-migration <serviceName> <deploymentName> new "" "" ""
```

Geçiş için bulut hizmetindeki sanal makine hazırlayın. Aralarından seçim yapabileceğiniz iki seçeneğiniz vardır.

Platform tarafından oluşturulan bir sanal ağa sanal makineleri geçirmek istiyorsanız, aşağıdaki komutu kullanın.

    azure service deployment prepare-migration <serviceName> <deploymentName> new "" "" ""

Resource Manager dağıtım modelinde var olan bir sanal ağa geçirmek istiyorsanız, aşağıdaki komutu kullanın.

    azure service deployment prepare-migration <serviceName> <deploymentName> existing <destinationVNETResourceGroupName> <subnetName> <vnetName>

Hazırlama işlemi başarılı olduktan sonra sanal makinelerin geçiş durumu almak ve içinde olduklarından emin olmak için ayrıntılı çıkış aracılığıyla bakabilirsiniz `Prepared` durumu.

    azure vm show <vmName> -vv

CLI veya Azure portalını kullanarak hazırlanmış kaynaklar için yapılandırmasını denetleyin. Geçiş için hazır değildir ve eski durumuna geri dönmek istiyorsanız aşağıdaki komutu kullanın.

    azure service deployment abort-migration <serviceName> <deploymentName>

Hazırlanan yapılandırma iyi görünüyorsa, ileriye taşıyın ve aşağıdaki komutu kullanarak kaynakları işleyin.

    azure service deployment commit-migration <serviceName> <deploymentName>



## <a name="step-4-option-2----migrate-virtual-machines-in-a-virtual-network"></a>4\. Adım: Seçenek 2 - bir sanal ağdaki sanal makinelerin geçişini gerçekleştirin
Geçirmek istediğiniz sanal ağı seçin. Sanal ağ ile desteklenmeyen yapılandırmalar web/çalışan rolleri veya sanal makineleri içeriyorsa, bir doğrulama hata iletisi alırsınız unutmayın.

Tüm sanal ağları, abonelikte aşağıdaki komutu kullanarak alın.

    azure network vnet list

Çıktı aşağıdakine benzer görünecektir:

![Komut satırı ile tüm sanal ağ adı vurgulanmış ekran görüntüsü.](../media/virtual-machines-linux-cli-migration-classic-resource-manager/vnet.png)

Yukarıdaki örnekte, **virtualNetworkName** tüm adı **"Grubu classicubuntu16 classicubuntu16"** .

İlk olarak, aşağıdaki komutu kullanarak sanal ağa geçirirseniz doğrulama:

```shell
azure network vnet validate-migration <virtualNetworkName>
```

Seçtiğiniz sanal ağ, aşağıdaki komutu kullanarak, geçiş için hazırlayın.

    azure network vnet prepare-migration <virtualNetworkName>

CLI veya Azure portalını kullanarak hazırlanmış sanal makineler için yapılandırmayı denetleyin. Geçiş için hazır değildir ve eski durumuna geri dönmek istiyorsanız aşağıdaki komutu kullanın.

    azure network vnet abort-migration <virtualNetworkName>

Hazırlanan yapılandırma iyi görünüyorsa, ileriye taşıyın ve aşağıdaki komutu kullanarak kaynakları işleyin.

    azure network vnet commit-migration <virtualNetworkName>

## <a name="step-5-migrate-a-storage-account"></a>5\. Adım: Bir depolama hesabını geçirin
İşiniz bittiğinde sanal makineleri geçiriyorsanız, depolama hesabını geçirme öneririz.

Aşağıdaki komutu kullanarak depolama hesabı geçiş için hazırlama

    azure storage account prepare-migration <storageAccountName>

CLI veya Azure portalını kullanarak hazırlanmış bir depolama hesabı için yapılandırmasını denetleyin. Geçiş için hazır değildir ve eski durumuna geri dönmek istiyorsanız aşağıdaki komutu kullanın.

    azure storage account abort-migration <storageAccountName>

Hazırlanan yapılandırma iyi görünüyorsa, ileriye taşıyın ve aşağıdaki komutu kullanarak kaynakları işleyin.

    azure storage account commit-migration <storageAccountName>

## <a name="next-steps"></a>Sonraki adımlar

* [Iaas kaynaklarının Klasik modelden Azure Resource Manager'a platform destekli geçişe genel bakış](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Klasik modelden Azure Resource Manager’a platform destekli geçişe ayrıntılı teknik bakış](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [IaaS kaynaklarının Klasik’ten Azure Resource Manager’a geçişini planlama](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Iaas kaynaklarının Klasik'ten Azure Resource Manager'a geçiş için PowerShell kullanma](../windows/migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Iaas kaynaklarını Klasik modelden Azure Resource Manager'a geçişini ile Yardım için topluluk araçları](../windows/migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [En sık karşılaşılan geçiş hatalarını gözden geçirme](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Gözden geçirme Iaas kaynaklarını Klasik modelden Azure Resource Manager'a hakkında sık sorulan sorular](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
