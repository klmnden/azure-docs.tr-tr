---
title: Azure Ağ İzleyicisi güvenlik grubu görünümü ile NSG denetim otomatik hale getirin | Microsoft Docs
description: Bu sayfa bir ağ güvenlik grubuna denetimi yapılandırma hakkında yönergeler sağlar.
services: network-watcher
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
ms.assetid: 78a01bcf-74fe-402a-9812-285f3501f877
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: kumud
ms.openlocfilehash: 016d68de90088314250fef1fcfdb57d7f155ef79
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64707152"
---
# <a name="automate-nsg-auditing-with-azure-network-watcher-security-group-view"></a>Azure Ağ İzleyicisi güvenlik grubu görünümü ile NSG denetim otomatikleştirin

Müşteriler, yaptıkları altyapı güvenlik duruşunu doğrulama sınama ile genellikle kalmaktadır. Bu sorunu, Azure Vm'leri için farklı değildir. Uygulanan ağ güvenlik grubu (NSG) kurallara göre benzer güvenlik profili olması önemlidir. Güvenlik grubu görünümünü kullanarak, artık bir VM bir NSG içinde uygulanan kurallar listesini alabilirsiniz. Altın bir NSG güvenlik profili tanımlamak ve haftalık temposu üzerinde güvenlik grubu görünümünü başlatmak ve altın profil çıkışı karşılaştırın ve rapor oluşturma. Bu şekilde önceden belirlenmiş güvenlik profiline uygun olmayan tüm sanal makineleri kolayca tanımlayabilirsiniz.

Ağ güvenlik gruplarıyla alışkın değilseniz bkz [ağ güvenliğine genel bakış](../virtual-network/security-overview.md).


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="before-you-begin"></a>Başlamadan önce

Bu senaryoda, bilinen bir iyi temel bir sanal makine için döndürülen güvenlik grubu görünümü sonuçlarını karşılaştırın.

Bu senaryo, zaten uyguladığınız adımları varsayar [Ağ İzleyicisi oluşturma](network-watcher-create.md) Ağ İzleyicisi oluşturmak için. Senaryo da kullanılacak geçerli bir sanal makine ile bir kaynak grubu var olduğunu varsayar.

## <a name="scenario"></a>Senaryo

Bu makalede ele alınan senaryo, bir sanal makine için güvenlik grubu görünümünü alır.

Bu senaryoda, şunları yapacaksınız:

- Bir bilinen iyi kural kümesi Al
- Bir sanal makine Rest API ile Al
- Sanal makine için güvenlik grubu görünümünü Al
- Yanıt değerlendir

## <a name="retrieve-rule-set"></a>Kural kümesi Al

Bu örnekte ilk adımda var olan bir taban çizgisi ile çalışmaktır. Aşağıdaki örnek, var olan bir kullanarak ağ güvenlik grubu ayıklanan bazı json `Get-AzNetworkSecurityGroup` Bu örnek için taban çizgisi olarak kullanılan cmdlet'i.

```json
[
    {
        "Description":  null,
        "Protocol":  "TCP",
        "SourcePortRange":  "*",
        "DestinationPortRange":  "3389",
        "SourceAddressPrefix":  "*",
        "DestinationAddressPrefix":  "*",
        "Access":  "Allow",
        "Priority":  1000,
        "Direction":  "Inbound",
        "ProvisioningState":  "Succeeded",
        "Name":  "default-allow-rdp",
        "Etag":  "W/\"d8859256-1c4c-4b93-ba7d-73d9bf67c4f1\"",
        "Id":  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/testvm1-nsg/securityRules/default-allow-rdp"
    },
    {
        "Description":  null,
        "Protocol":  "*",
        "SourcePortRange":  "*",
        "DestinationPortRange":  "111",
        "SourceAddressPrefix":  "*",
        "DestinationAddressPrefix":  "*",
        "Access":  "Allow",
        "Priority":  1010,
        "Direction":  "Inbound",
        "ProvisioningState":  "Succeeded",
        "Name":  "MyRuleDoNotDelete",
        "Etag":  "W/\"d8859256-1c4c-4b93-ba7d-73d9bf67c4f1\"",
        "Id":  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/testvm1-nsg/securityRules/MyRuleDoNotDelete"
    },
    {
        "Description":  null,
        "Protocol":  "*",
        "SourcePortRange":  "*",
        "DestinationPortRange":  "112",
        "SourceAddressPrefix":  "*",
        "DestinationAddressPrefix":  "*",
        "Access":  "Allow",
        "Priority":  1020,
        "Direction":  "Inbound",
        "ProvisioningState":  "Succeeded",
        "Name":  "My2ndRuleDoNotDelete",
        "Etag":  "W/\"d8859256-1c4c-4b93-ba7d-73d9bf67c4f1\"",
        "Id":  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/testvm1-nsg/securityRules/My2ndRuleDoNotDelete"
    },
    {
        "Description":  null,
        "Protocol":  "TCP",
        "SourcePortRange":  "*",
        "DestinationPortRange":  "5672",
        "SourceAddressPrefix":  "*",
        "DestinationAddressPrefix":  "*",
        "Access":  "Deny",
        "Priority":  1030,
        "Direction":  "Inbound",
        "ProvisioningState":  "Succeeded",
        "Name":  "ThisRuleNeedsToStay",
        "Etag":  "W/\"d8859256-1c4c-4b93-ba7d-73d9bf67c4f1\"",
        "Id":  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/testvm1-nsg/securityRules/ThisRuleNeedsToStay"
    }
]
```

## <a name="convert-rule-set-to-powershell-objects"></a>Kural kümesi PowerShell nesnelerine dönüştürebilirsiniz.

Bu adımda, biz daha önce bu örneği için ağ güvenlik grubu olması beklenen kurallar ile oluşturulmuş bir json dosyası okur.

```powershell
$nsgbaserules = Get-Content -Path C:\temp\testvm1-nsg.json | ConvertFrom-Json
```

## <a name="retrieve-network-watcher"></a>Ağ İzleyicisi alınamıyor

Ağ İzleyicisi örneğini almak için sonraki adımdır bakın. `$networkWatcher` Değişken geçirilir `AzNetworkWatcherSecurityGroupView` cmdlet'i.

```powershell
$nw = Get-AzResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="get-a-vm"></a>VM Al

Bir sanal makine çalıştırmak için gerekli olan `Get-AzNetworkWatcherSecurityGroupView` karşı cmdlet'i. Aşağıdaki örnek, bir sanal makine nesnesini alır.

```powershell
$VM = Get-AzVM -ResourceGroupName "testrg" -Name "testvm1"
```

## <a name="retrieve-security-group-view"></a>Güvenlik grubu görünümünü Al

Sonraki adım, güvenlik grubu görünümü sonucu almasını sağlamaktır. Bu sonuç, daha önce gösterilen "temel" json karşılaştırılır.

```powershell
$secgroup = Get-AzNetworkWatcherSecurityGroupView -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id
```

## <a name="analyzing-the-results"></a>Sonuçları analiz etme

Yanıt, ağ arabirimleri tarafından gruplandırılır. Döndürülen kuralları farklı türde etkilidir ve varsayılan güvenlik kuralları. Sonuç daha fazla nasıl, bir alt ağ veya sanal bir NIC'ye uygulandığı tarafından ayrılmıştır

Aşağıdaki PowerShell betiğini bir NSG mevcut bir çıktısına güvenlik grubu görünümü sonuçlarını karşılaştırır. Aşağıdaki örnek, basit bir örneğini nasıl sonuçları ile karşılaştırılabilir `Compare-Object` cmdlet'i.

```powershell
Compare-Object -ReferenceObject $nsgbaserules `
-DifferenceObject $secgroup.NetworkInterfaces[0].NetworkInterfaceSecurityRules `
-Property Name,Description,Protocol,SourcePortRange,DestinationPortRange,SourceAddressPrefix,DestinationAddressPrefix,Access,Priority,Direction
```

Aşağıdaki örnek sonuç olur. İki ilk kuralında ayarlanan kurallar Karşılaştırmada bulunmamaktadır görebilirsiniz.

```
Name                     : My2ndRuleDoNotDelete
Description              : 
Protocol                 : *
SourcePortRange          : *
DestinationPortRange     : 112
SourceAddressPrefix      : *
DestinationAddressPrefix : *
Access                   : Allow
Priority                 : 1020
Direction                : Inbound
SideIndicator            : <=

Name                     : ThisRuleNeedsToStay
Description              : 
Protocol                 : TCP
SourcePortRange          : *
DestinationPortRange     : 5672
SourceAddressPrefix      : *
DestinationAddressPrefix : *
Access                   : Deny
Priority                 : 1030
Direction                : Inbound
SideIndicator            : <=
```

## <a name="next-steps"></a>Sonraki adımlar

Ayarları değiştirilmiş olup [ağ güvenlik grupları yönet](../virtual-network/manage-network-security-group.md) söz konusu olan ağ güvenlik grubu ve güvenlik kuralları izlemek için.













