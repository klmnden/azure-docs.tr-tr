---
title: Başka bir laboratuvar Azure DevTest Labs'de sanal makineler alma | Microsoft Docs
description: Sanal makineleri başka bir laboratuvarda geçerli laboratuara içeri aktarmayı öğrenin.
services: devtest-lab, lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2019
ms.author: spelluru
ms.openlocfilehash: 9cd2e5e211fcda7c59469d3b09e9c9e5bdefdbd6
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2019
ms.locfileid: "59546600"
---
# <a name="import-virtual-machines-from-another-lab-in-azure-devtest-labs"></a>Başka bir laboratuar ortamında Azure DevTest Labs sanal makineleri içeri aktarın
Bu makalede sanal makineleri başka bir laboratuvarda, laboratuvarda içeri aktarma hakkında bilgi sağlar. 

## <a name="scenarios"></a>Senaryolar
Vm'leri, başka bir laboratuvarda bir laboratuvarda alma gerek duyduğunuz senaryolara bazı senaryolar aşağıda verilmiştir: 

- Bir kişi takımdaki kuruluş içinde başka bir gruba taşınacağını ve yeni takımın DevTest Labs kullanarak Geliştirici Masaüstü almak ister.
- Grup erişti bir [abonelik düzeyi kota](../azure-subscription-service-limits.md) ve birkaç aboneliği teams'e yukarı split istiyor
- Express Route (veya başka bir yeni ağ topolojisi) şirket taşınıyor ve takım, bu yeni altyapı kullanmak için sanal makineleri taşımak istiyor

## <a name="solution-and-constraints"></a>Çözüm ve kısıtlamalar
Bu özellik, Vm'leri (kaynak) bir laboratuar ortamında başka bir laboratuvara (hedef) içeri aktarmanıza olanak sağlar. İsteğe bağlı olarak, hedef VM işlemi için yeni bir ad verebilirsiniz. İçeri aktarma işlemi, diskleri, zamanlamalar, ağ ayarları ve benzeri gibi tüm bağımlılıkları içerir.

Bu işlem biraz zaman alabilir ve aşağıdaki faktörlerden etkilenir:

- Sayı/boyutu (bir kopyalama işlemi ve taşıma işlemi olduğundan) kaynak makineye bağlı diskler 
- ' % S'hedef (örneğin, Güneydoğu Asya Doğu ABD bölgesinde) uzaklık.  

İşlem tamamlandıktan sonra kaynak sanal makine kapatma ve yeni bir hedef laboratuvarda çalıştırıyorsa kalır.

Sanal makineleri bir laboratuar ortamında başka bir laboratuvara aktarmak planlama yaparken dikkat edilmesi gereken iki anahtar kısıtlamaları vardır:

- Abonelikler arasında ve farklı bölgelerdeki sanal makine içeri aktarmalar desteklenir ancak aboneliklerin aynı Azure Active Directory kiracısı ile ilişkilendirilmesi gerekir.
- Sanal makineler kaynak Laboratuvar talep edilebilir bir durumda olmalıdır.
- VM'nin hedef Laboratuvardaki Laboratuvar sahibini ve kaynak Laboratuvar sahibi olmanız.
- Şu anda bu özellik yalnızca Powershell ve REST API desteklenir.

## <a name="use-powershell"></a>PowerShell kullanma
ImportVirtualMachines.ps1 dosyası indirin [GitHub](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/Scripts/ImportVirtualMachines). Betik, tek bir VM veya kaynak Laboratuvardaki tüm VM'ler hedef laboratuara içeri aktarmak için kullanabilirsiniz. 

### <a name="use-powershell-to-import-a-single-vm"></a>Tek bir VM için PowerShell kullanma
Bu powershell betiği yürütülürken, VM kaynak ve hedef Laboratuvar tanımlama ve isteğe bağlı olarak hedef makine için kullanılacak yeni bir ad sağlayarak gerektirir:

```powershell 
./ImportVirtualMachines.ps1 -SourceSubscriptionId "<ID of the subscription that contains the source lab>" `
                            -SourceDevTestLabName "<Name of the source lab>" `
                            -SourceVirtualMachineName "<Name of the VM to be imported from the source lab> " `
                            -DestinationSubscriptionId "<ID of the subscription that contians the destination lab>" `
                            -DestinationDevTestLabName "<Name of the destination lab>" `
                            -DestinationVirtualMachineName "<Optional: specify a new name for the imported VM in the destination lab>"
```

### <a name="use-powershell-to-import-all-vms-in-the-source-lab"></a>Kaynak Laboratuvardaki tüm sanal makineleri içeri aktarmak için PowerShell kullanma
Kaynak sanal makinenin belirtilmezse, betik DevTest labs'teki tüm sanal makineler otomatik olarak içeri aktarır.  Örneğin:
 
```powershell
./ImportVirtualMachines.ps1 -SourceSubscriptionId "<ID of the subscription that contains the source lab>" `
                            -SourceDevTestLabName "<Name of the source lab>" `
                            -DestinationSubscriptionId "<ID of the subscription that contians the destination lab>" `
                            -DestinationDevTestLabName "<Name of the destination lab>"
```

## <a name="use-http-rest-to-import-a-vm"></a>Bir VM almak için HTTP REST kullanma
REST araması, basit bir işlemdir. Size kaynak ve hedef kaynaklarını belirlemek için yeterli bilgi sunar. İşlem gerçekleştikten hedef Laboratuvar kaynakta unutmayın.

```REST
POST https://management.azure.com/subscriptions/<DestinationSubscriptionID>/resourceGroups/<DestinationResourceGroup>/providers/Microsoft.DevTestLab/labs/<DestinationLab>/ImportVirtualMachine?api-version=2017-04-26-preview
{
   sourceVirtualMachineResourceId: "/subscriptions/<SourceSubscriptionID>/resourcegroups/<SourceResourceGroup>/providers/microsoft.devtestlab/labs/<SourceLab>/virtualmachines/<NameofVMTobeImported>",
   destinationVirtualMachineName: "<NewNameForImportedVM>"
}
```

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın: 

- [Bir laboratuvara yönelik ilkeleri ayarlama](devtest-lab-get-started-with-lab-policies.md)
- [Sık sorulan sorular](devtest-lab-faq.md)
