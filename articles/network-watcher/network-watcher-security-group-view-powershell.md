---
title: Azure Ağ İzleyicisi güvenlik grubu görünümü - PowerShell ile ağ güvenlik analiz | Microsoft Docs
description: Bu makalede, bir sanal makine güvenliği'güvenlik grubu görünümü ile çözümlemek için PowerShell kullanmayı anlatmaktadır.
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: 04e76b49-6a1b-4d0f-9a9b-51cf2f4df5a2
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: b22dd2dcf575362b96d150ef98148076f4ec631f
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59047594"
---
# <a name="analyze-your-virtual-machine-security-with-security-group-view-using-powershell"></a>PowerShell kullanarak bir güvenlik grubu görünümü ile sanal makine güvenlik analiz edin

> [!div class="op_single_selector"]
> - [PowerShell](network-watcher-security-group-view-powershell.md)
> - [Azure CLI](network-watcher-security-group-view-cli.md)
> - [REST API](network-watcher-security-group-view-rest.md)

Güvenlik grubu görünümü bir sanal makineye uygulanan yapılandırılmış ve etkin ağ güvenlik kuralları döndürür. Bu yetenek, Denetim ve ağ güvenlik grupları ve trafiği okunuyor emin olmak için bir VM üzerinde yapılandırılan kurallardan tanılamak kullanışlıdır doğru bir şekilde izin verilen veya reddedilen. Bu makalede, PowerShell kullanarak bir sanal makine için yapılandırılmış ve etkili güvenlik kuralları alma gösteriyoruz


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="before-you-begin"></a>Başlamadan önce

Bu senaryoda, çalıştırdığınız `Get-AzNetworkWatcherSecurityGroupView` güvenlik kuralı bilgileri almak için cmdlet'i.

Bu senaryo, zaten uyguladığınız adımları varsayar [Ağ İzleyicisi oluşturma](network-watcher-create.md) Ağ İzleyicisi oluşturmak için.

## <a name="scenario"></a>Senaryo

Bu makalede ele alınan senaryo, belirli bir sanal makine için yapılandırılmış ve etkili güvenlik kurallarını alır.

## <a name="retrieve-network-watcher"></a>Ağ İzleyicisi alınamıyor

Ağ İzleyicisi örneğini almak için ilk adımdır bakın. Bu değişkenin geçirilen `Get-AzNetworkWatcherSecurityGroupView` cmdlet'i.

```powershell
$nw = Get-AzResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" }
$networkWatcher = Get-AzNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName
```

## <a name="get-a-vm"></a>VM Al

Bir sanal makine çalıştırmak için gerekli olan `Get-AzNetworkWatcherSecurityGroupView` karşı cmdlet'i. Aşağıdaki örnek, bir sanal makine nesnesini alır.

```powershell
$VM = Get-AzVM -ResourceGroupName testrg -Name testvm1
```

## <a name="retrieve-security-group-view"></a>Güvenlik grubu görünümünü Al

Sonraki adım, güvenlik grubu görünümü sonucu almasını sağlamaktır.

```powershell
$secgroup = Get-AzNetworkWatcherSecurityGroupView -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id
```

## <a name="viewing-the-results"></a>Sonuçları görüntüleme

Aşağıdaki örnek, döndürülen sonuçların kısaltılmış yanıttır. Gruplar halinde ayrılmış sanal makine üzerindeki tüm etkin ve uygulanan güvenlik kuralları sonuçlarını göster **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, ve  **EffectiveSecurityRules**.

```
NetworkInterfaces : [
                      {
                        "NetworkInterfaceSecurityRules": [
                          {
                            "Name": "default-allow-rdp",
                            "Etag": "W/\"d4c411d4-0d62-49dc-8092-3d4b57825740\"",
                            "Id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg2/providers/Microsoft.Network/networkSecurityGroups/testvm2-nsg/securityRules/default-allow-rdp",
                            "Protocol": "TCP",
                            "SourcePortRange": "*",
                            "DestinationPortRange": "3389",
                            "SourceAddressPrefix": "*",
                            "DestinationAddressPrefix": "*",
                            "Access": "Allow",
                            "Priority": 1000,
                            "Direction": "Inbound",
                            "ProvisioningState": "Succeeded"
                          }
                          ...
                        ],
                        "DefaultSecurityRules": [
                          {
                            "Name": "AllowVnetInBound",
                            "Id": "/subscriptions00000000-0000-0000-0000-000000000000/resourceGroups/testrg2/providers/Microsoft.Network/networkSecurityGroups/testvm2-nsg/defaultSecurityRules/",
                            "Description": "Allow inbound traffic from all VMs in VNET",
                            "Protocol": "*",
                            "SourcePortRange": "*",
                            "DestinationPortRange": "*",
                            "SourceAddressPrefix": "VirtualNetwork",
                            "DestinationAddressPrefix": "VirtualNetwork",
                            "Access": "Allow",
                            "Priority": 65000,
                            "Direction": "Inbound",
                            "ProvisioningState": "Succeeded"
                          }
                          ...
                        ],
                        "EffectiveSecurityRules": [
                          {
                            "Name": "DefaultOutboundDenyAll",
                            "Protocol": "All",
                            "SourcePortRange": "0-65535",
                            "DestinationPortRange": "0-65535",
                            "SourceAddressPrefix": "*",
                            "DestinationAddressPrefix": "*",
                            "ExpandedSourceAddressPrefix": [],
                            "ExpandedDestinationAddressPrefix": [],
                            "Access": "Deny",
                            "Priority": 65500,
                            "Direction": "Outbound"
                          },
                          ...
                        ]
                      }
                    ]
```

## <a name="next-steps"></a>Sonraki adımlar

Ziyaret [denetimi ağ güvenlik grupları (NSG) Ağ İzleyicisi ile](network-watcher-nsg-auditing-powershell.md) ağ güvenlik gruplarının doğrulama otomatikleştirme hakkında bilgi edinmek için.


