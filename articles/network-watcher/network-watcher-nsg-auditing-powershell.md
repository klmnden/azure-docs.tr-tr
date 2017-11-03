---
title: "Azure Ağ İzleyicisi güvenlik grubu görünümü ile NSG denetim otomatikleştirmek | Microsoft Docs"
description: "Bu sayfa bir ağ güvenlik grubunun denetimi yapılandırma hakkında yönergeler sağlar"
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: 78a01bcf-74fe-402a-9812-285f3501f877
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 57f2200e541eeb629f72d60ffa0acb2d8233c018
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="automate-nsg-auditing-with-azure-network-watcher-security-group-view"></a>Azure Ağ İzleyicisi güvenlik grubu görünümü ile NSG denetim otomatikleştirme

Müşteriler genellikle güvenlik yaklaşımı altyapılarını, doğrulama, bir güçlükle karşı karşıya kalmaktadır. Bu sorunu Azure Vm'leri için farklı değildir. Uygulanan ağ güvenlik grubu (NSG) kurallara göre benzer bir güvenlik profili olması önemlidir. Güvenlik grubu görünümü kullanarak, bir VM bir NSG içinde uygulanan kurallar listesi şimdi alabilirsiniz. Bir altın NSG güvenlik profili tanımlamak ve güvenlik grubu görünümü haftalık bir tempoyla üzerinde başlatmak ve altın profili çıkışı karşılaştırın ve bir rapor oluşturun. Bu şekilde, önceden belirlenen güvenlik profili uymayan tüm sanal makineleri kolayca tanımlayabilirsiniz.

Ağ güvenlik gruplarıyla tanımıyorsanız ziyaret [ağ güvenliğine genel bakış](../virtual-network/virtual-networks-nsg.md)

## <a name="before-you-begin"></a>Başlamadan önce

Bu senaryoda, bir sanal makine için döndürülen güvenlik grubu görünümü sonuçları bilinen iyi taban çizgisi karşılaştırın.

Bu senaryo zaten izlediğiniz adımlarda varsayar [bir Ağ İzleyicisi oluşturma](network-watcher-create.md) bir Ağ İzleyicisi oluşturmak için. Senaryo da geçerli bir sanal makine ile bir kaynak grubu kullanılacak var olduğunu varsayar.

## <a name="scenario"></a>Senaryo

Bu makalede ele alınan senaryo, bir sanal makine için güvenlik grubu görünümü alır.

Bu senaryoda, şunları yapacaksınız:

- Bilinen iyi kural kümesini Al
- Rest API sahip bir sanal makine alma
- Sanal makine için güvenlik grubu görünümü Al
- Yanıt değerlendir

## <a name="retrieve-rule-set"></a>Kural kümesini Al

İlk Bu örnekte varolan bir taban çizgisi ile çalışmak için adımdır. Aşağıdaki örnekte olduğu bir mevcut ağ güvenlik grubu kullanımından ayıklanan bazı json `Get-AzureRmNetworkSecurityGroup` taban çizgisi olarak bu örnek için kullanılan cmdlet.

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

## <a name="convert-rule-set-to-powershell-objects"></a>Kural kümesi için PowerShell nesneleri dönüştürme

Bu adımda, biz Bu örnek için ağ güvenlik grubu olması beklenen kurallar ile daha önce oluşturulmuş bir json dosyası okur.

```powershell
$nsgbaserules = Get-Content -Path C:\temp\testvm1-nsg.json | ConvertFrom-Json
```

## <a name="retrieve-network-watcher"></a>Ağ İzleyicisi alma

Ağ İzleyicisi örneği almak için sonraki adımdır bakın. `$networkWatcher` Değişkeni iletilir `AzureRmNetworkWatcherSecurityGroupView` cmdlet'i.

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="get-a-vm"></a>VM Al

Bir sanal makineyi çalıştırmak için gerekli `Get-AzureRmNetworkWatcherSecurityGroupView` karşı cmdlet'i. Aşağıdaki örnek, VM nesnesini alır.

```powershell
$VM = Get-AzurermVM -ResourceGroupName "testrg" -Name "testvm1"
```

## <a name="retrieve-security-group-view"></a>Güvenlik grubu görünümü alma

Güvenlik grubu Görünüm sonucu almak için sonraki adımdır bakın. Bu sonuç, daha önce gösterilen "temel" json karşılaştırılır.

```powershell
$secgroup = Get-AzureRmNetworkWatcherSecurityGroupView -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id
```

## <a name="analyzing-the-results"></a>Sonuçlarını çözümleme

Yanıt ağ arabirimleri tarafından gruplandırılır. Farklı tür döndürülen kuralların etkili olur ve güvenlik kuralları varsayılan. Sonuç daha fazla nasıl, bir alt ağ veya bir sanal NIC uygulandığı tarafından ayrılmıştır

Aşağıdaki PowerShell betiğini bir NSG varolan çıktısı güvenlik grubu görünümüne sonuçlarını karşılaştırır. Aşağıdaki örnekte nasıl sonuçları ile karşılaştırılabilir bir basit örneğidir `Compare-Object` cmdlet'i.

```powershell
Compare-Object -ReferenceObject $nsgbaserules `
-DifferenceObject $secgroup.NetworkInterfaces[0].NetworkInterfaceSecurityRules `
-Property Name,Description,Protocol,SourcePortRange,DestinationPortRange,SourceAddressPrefix,DestinationAddressPrefix,Access,Priority,Direction
```

Aşağıdaki örnek sonucudur. İki ilk kuralında ayarlanan kuralların Karşılaştırmada mevcut değil görebilirsiniz.

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

Ayarları değiştirilmiş olup [ağ güvenlik grupları yönet](../virtual-network/virtual-network-manage-nsg-arm-portal.md) söz konusu olan ağ güvenlik grubu ve güvenlik kuralları izlemek için.













