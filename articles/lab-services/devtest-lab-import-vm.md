---
title: Azure DevTest Labs başka bir laboratuvarda Vm'leri alma | Microsoft Docs
description: Geçerli Azure DevTest Labs Laboratuvar içinde başka bir laboratuvarda sanal makineleri içeri aktarmayı öğrenin
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 8460f09e-482f-48ba-a57a-c95fe8afa001
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2018
ms.author: spelluru
ms.openlocfilehash: cb4a3ec9be82957b4c0366ec232f1147c52d0251
ms.sourcegitcommit: c884e2b3746d4d5f0c5c1090e51d2056456a1317
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60148781"
---
# <a name="import-vms-from-another-lab-in-azure-devtest-labs"></a>Azure DevTest Labs başka bir laboratuvarda Vm'lerden içeri aktarma
Azure DevTest Labs hizmeti geliştirme ve test etkinlikleri için sanal makinelerin (VM'ler) yönetim önemli ölçüde artırır. Başka bir takım olarak bir laboratuvarda bir VM'yi taşıma izin verir veya altyapı gereksinimleri değiştirin. Burada Bunu yapmak gerekebilir bazı yaygın senaryolar aşağıda verilmiştir:

- Bir kişi takımdaki kuruluş içinde başka bir gruba taşır ve yeni takımın laboratuvara geliştirme Vm'leri almak ister.
- Grup abonelik düzeyinde kotasına ulaştı ve birden çok abonelik teams'e bölüneceği istiyor.
- Express Route (veya başka bir yeni ağ topolojisi) taşımak şirket planları ve takım bu yeni altyapı kullanmak için Vm'leri taşımak istiyor.

## <a name="solution-and-constraints"></a>Çözüm ve kısıtlamalar
Azure DevTest Labs sanal makineleri bir kaynak laboratuvarda hedef laboratuvara almak Laboratuvar sahibi sağlar. Laboratuvar sahibi, isteğe bağlı olarak işlem hedef VM için yeni bir ad sağlayabilirsiniz. İçeri aktarma işlemi, diskleri, zamanlamalar, ağ ayarları vb. gibi tüm bağımlılıkları içerir. İşlemi biraz zaman alabilir ve kaynak makine ve hedef (örneğin, Güneydoğu Asya bölgesi için Doğu ABD bölgesinde) uzaklık ekli disklerin sayısı/boyutu tarafından etkilenir. İçeri aktarma işlemi tamamlandıktan sonra kaynak VM kapatma ve yeni bir hedef laboratuvarda çalıştırıyorsa kalır.

Vm'leri, başka bir laboratuvara içeri aktarmak planlama yaparken dikkat edilmesi gereken iki anahtar kısıtlamaları vardır:

- Abonelikler arasında ve çapraz Vm'leri alma bölgeler desteklenir, ancak aboneliklerin aynı Azure Active Directory kiracısı ile ilişkilendirilmesi gerekir.
- Vm'leri kaynak Laboratuvar talep edilebilir bir durumda olmalıdır.

Ayrıca, bir VM'nin bir laboratuvarda içeri aktarabilmek için laboratuvar kaynak ve hedef Laboratuvardaki Laboratuvar sahibini VM sahibi olmanız gerekir.

## <a name="steps-to-import-a-vm-from-another-lab"></a>Başka bir laboratuvarda sanal makine içeri aktarma adımları
Şu anda, bir VM bir laboratuvarda diğerine yalnızca Azure PowerShell ve REST API kullanarak içeri aktarabilirsiniz.

### <a name="use-powershell"></a>PowerShell kullanma
PowerShell komut dosyası gelen ImportVirtualMachines.ps1 indirme [Azure DevTest Lab Git deposu](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/Scripts/ImportVirtualMachines) yerel sürücünüze.

#### <a name="import-a-single-vm"></a>Tek bir sanal makine içeri aktarma
Tek bir VM bir kaynak laboratuvarda hedef laboratuvara içeri aktarmak için ImportVirtualMachines.ps1 betiği çalıştırın. DestinationVirtualMachineName paramer kullanarak kopyalanan VM için yeni bir ad belirtebilirsiniz.

```powershell
./ImportVirtualMachines.ps1 -SourceSubscriptionId "<ID of the subscription that contains the source VM>" `
                            -SourceDevTestLabName "<Name of the lab that contains the source VM>" `
                            -SourceVirtualMachineName "<Name of the machine. Optional. If not specified, all VMs are copied>" `
                            -DestinationSubscriptionId "<ID of the target/destination subscription>" `
                            -DestinationDevTestLabName "<Name of the lab to which the VM is copied>" `
                            -DestinationVirtualMachineName "<New name for the VM. Optional>"
```


#### <a name="importing-all-vms"></a>Tüm sanal makineleri içeri aktarma
Kaynak laboratuvarda VM belirtmezseniz ImportVirtualMachines.ps1 komut dosyasını çalıştırdığınızda, komut dosyası kaynak Laboratuvardaki tüm VM'ler hedef laboratuara içeri aktarır.

```powershell
./ImportVirtualMachines.ps1 -SourceSubscriptionId "<ID of the subscription that contains the source VM>" `
                            -SourceDevTestLabName "<Name of the lab that contains the source VM>" `
                            -DestinationSubscriptionId "<ID of the target/destination subscription>" `
                            -DestinationDevTestLabName "<Name of the lab to which the VMs are copied>"
```

### <a name="use-rest-api"></a>REST API’yi kullanma
Hedef/Laboratuvar karşı REST API'yi çağırmak ve kaynak Laboratuvar, abonelik ve VM bilgileri aşağıdaki örnekte gösterildiği gibi parametreler olarak geçirin:

```json
POST https://management.azure.com/subscriptions/<ID of the target/destination subscription>/resourceGroups/<Name of the resource group that contains the destination lab>/providers/Microsoft.DevTestLab/labs/<Name of the lab to which the VMs are copied>/ImportVirtualMachine?api-version=2017-04-26-preview
{
   sourceVirtualMachineResourceId: "/subscriptions/<ID of the subscription that contains the source VM>/resourcegroups/<Name of the resource group that contains the source lab>/providers/microsoft.devtestlab/labs/<Name of the lab that contains the source VM>/virtualmachines/MyVm",
   destinationVirtualMachineName: "MyVmNew"
}
```

## <a name="next-steps"></a>Sonraki adımlar

- VM yeniden boyutlandırma hakkında daha fazla bilgi için bkz: [VM'yi yeniden boyutlandırma](devtest-lab-resize-vm.md).
- Bir sanal makine yeniden dağıtıldığında hakkında daha fazla bilgi için bkz: [bir VM'yi yeniden dağıtma](devtest-lab-redeploy-vm.md).
