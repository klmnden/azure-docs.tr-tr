---
title: Azure Ağ İzleyicisi özelliklerini kullanmak için gereken izinler | Microsoft Docs
description: Ağ İzleyicisi özellikleriyle çalışması için hangi Azure rol tabanlı erişim denetimi izinleri gerekli öğrenin.
services: network-watcher
documentationcenter: ''
author: jimdial
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: network-watcher
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2018
ms.author: jdial
ms.openlocfilehash: 09f3a1e1d9c6796cb55ae8f0ab18bf8e1b3fa198
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
ms.locfileid: "34077881"
---
# <a name="role-based-access-control-permissions-required-to-use-network-watcher-capabilities"></a>Ağ İzleyicisi özellikleri kullanmak için gerekli rol tabanlı erişim denetimi izinleri

Azure rol tabanlı erişim denetimi (RBAC), yalnızca belirli eylemleri onlara, atanan sorumluluklarını tamamlamak için gereken, kuruluşunuzun üyelerine atamanıza olanak sağlar. Ağ İzleyicisi özellikleri kullanmak için oturum açmanız, Azure hesap atanmalıdır [sahibi](/role-based-access-control/built-in-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#owner), [katkıda bulunan](/role-based-access-control/built-in-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#contributor), veya [ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#network-contributor) yerleşik roller veya atanmış bir [özel rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) bölümlerde her Ağ İzleyicisi özelliği için listelenen eylemleri atanır. Ağ İzleyicisi'nin özellikleri hakkında daha fazla bilgi edinmek için [Ağ İzleyicisi nedir?](network-watcher-monitoring-overview.md).

## <a name="network-watcher"></a>Ağ İzleyicisi

| Eylem                                                              | Ad                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/read                              | Ağ İzleyicisi Al                                          |
| Microsoft.Network/networkWatchers/write                             | Ağ İzleyicisi güncelle                             |
| Microsoft.Network/networkWatchers/delete                            | Ağ İzleyicisi Sil                                       |

## <a name="nsg-flow-logs"></a>NSG akış günlükleri

| Eylem                                                              | Ad                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/configureFlowLog/action           | Bir günlük akışı Yapılandır                                           |
| Microsoft.Network/networkWatchers/queryFlowLogStatus/action         | Akış günlüğü için sorgu durumu                                    |

## <a name="connection-troubleshoot"></a>Bağlantı sorunlarını giderme

| Eylem                                                              | Ad                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/queryTroubleshootResult/action    | Sorgu sonuçları bağlantısının test sorun giderme                |
| Microsoft.Network/networkWatchers/troubleshoot/action               | Çalışma bağlantı sınama sorunlarını giderme                             |

## <a name="connection-monitor"></a>Bağlantı İzleyicisi

| Eylem                                                              | Ad                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/connectionMonitors/start/action   | Bir bağlantı İzleyicisi'ni başlatın                                     |
| Microsoft.Network/networkWatchers/connectionMonitors/stop/action    | Bir bağlantı izleyicisini durdurun                                      |
| Microsoft.Network/networkWatchers/connectionMonitors/query/action   | Sorgu Bağlantı İzleyicisi                                     |
| Microsoft.Network/networkWatchers/connectionMonitors/read           | Bağlantı İzleyici Al                                       |
| Microsoft.Network/networkWatchers/connectionMonitors/write          | Bağlantı izleyici oluşturmak                                    |
| Microsoft.Network/networkWatchers/connectionMonitors/delete         | Bağlantı İzleyici Sil                                    |

## <a name="packet-capture"></a>Paket yakalama

| Eylem                                                              | Ad                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/packetCaptures/queryStatus/action | Paket yakalama durumunu sorgulama                           |
| Microsoft.Network/networkWatchers/packetCaptures/stop/action        | Paket yakalama işlemini durdurun                                          |
| Microsoft.Network/networkWatchers/packetCaptures/read               | Paket yakalama Al                                           |
| Microsoft.Network/networkWatchers/packetCaptures/write              | Paket yakalama oluşturma                                        |
| Microsoft.Network/networkWatchers/packetCaptures/delete             | Paket yakalama Sil                                        |

## <a name="ip-flow-verify"></a>IP akış doğrulayın

| Eylem                                                              | Ad                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/ipFlowVerify/action               | Bir IP akışı doğrulayın                                              |

## <a name="next-hop"></a>Sonraki atlama

| Eylem                                                              | Ad                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/nextHop/action                    | Bir sanal makineden sonraki atlama Al                                     |

## <a name="network-security-group-view"></a>Ağ güvenlik grubu görünümü

| Eylem                                                              | Ad                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/securityGroupView/action          | Güvenlik grupları görüntüle                                           |

## <a name="topology"></a>Topoloji

| Eylem                                                              | Ad                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/topology/action                   | Topoloji Al                                                   |

## <a name="reachability-report"></a>Ulaşılabilirlik raporu

| Eylem                                                              | Ad                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/azureReachabilityReport/action    | Bir Azure ulaşılabilirlik rapor alma                               |

## <a name="additional-actions"></a>Ek Eylemler

Ağ İzleyicisi özelliklerini de aşağıdaki eylemleri gerektirir:

- Microsoft.Storage/Read
- Microsoft.Authorization/Read
- Microsoft.Resources/subscriptions/resourceGroups/Read
- Microsoft.Storage/storageAccounts/listServiceSas/Action
- Microsoft.Storage/storageAccounts/listAccountSas/Action
- Microsoft.Storage/storageAccounts/listKeys/Action
- Microsoft.Compute/virtualMachines/Read
- Microsoft.Compute/virtualMachines/Write
- Microsoft.Compute/virtualMachineScaleSets/Read
- Microsoft.Compute/virtualMachineScaleSets/Write
- Microsoft.Insights/alertRules/*
- Microsoft.Support/*