---
title: Azure Ağ İzleyicisi özellikleri kullanmak için gerekli izinleri | Microsoft Docs
description: Ağ İzleyicisi özellikleri ile çalışmak için gereken hangi Azure rol tabanlı erişim denetimi izinleri hakkında bilgi edinin.
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
ms.openlocfilehash: 7d0f0367a4126e7cecd34b39e6e5065e7d4fd90a
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43287116"
---
# <a name="role-based-access-control-permissions-required-to-use-network-watcher-capabilities"></a>Ağ İzleyicisi özellikleri kullanmak için gereken rol tabanlı erişim denetimi izinleri

Azure rol tabanlı erişim denetimi (RBAC), yalnızca belirli eylemler atanan sorumluluklarını tamamlamak için gereksinim duydukları kuruluşunuzun üyelerine atamanızı sağlar. Ağ İzleyicisi özellikleri kullanmak için oturum, azure'da kullandığınız hesabın atanmalıdır için [sahibi](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#owner), [katkıda bulunan](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#contributor), veya [ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#network-contributor) yerleşik roller veya atanmış bir [özel rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) aşağıdaki bölümlerde her Ağ İzleyicisi özelliği için listelenen eylemleri atanır. Ağ İzleyicisi'nin özellikleri hakkında daha fazla bilgi edinmek için [Ağ İzleyicisi nedir?](network-watcher-monitoring-overview.md).

## <a name="network-watcher"></a>Ağ İzleyicisi

| Eylem                                                              | Ad                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/read                              | Ağ İzleyicisi Al                                          |
| Microsoft.Network/networkWatchers/write                             | Ağ İzleyicisi güncelle                             |
| Microsoft.Network/networkWatchers/delete                            | Ağ İzleyicisini Sil                                       |

## <a name="nsg-flow-logs"></a>NSG akış günlükleri

| Eylem                                                              | Ad                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/configureFlowLog/action           | Günlük akışını yapılandırın                                           |
| Microsoft.Network/networkWatchers/queryFlowLogStatus/action         | Akış günlüğü için sorgu durumu                                    |

## <a name="connection-troubleshoot"></a>Bağlantı sorunlarını giderme

| Eylem                                                              | Ad                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/queryTroubleshootResult/action    | Test sorgu sonuçlarının bir bağlantı sorunlarını giderme                |
| Microsoft.Network/networkWatchers/troubleshoot/action               | Çalıştırma bağlantı testi sorunlarını giderme                             |

## <a name="connection-monitor"></a>Bağlantı İzleyicisi

| Eylem                                                              | Ad                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/connectionMonitors/start/action   | Bağlantı izleyicisini Başlat                                     |
| Microsoft.Network/networkWatchers/connectionMonitors/stop/action    | Bağlantı izleyicisini durdurmak                                      |
| Microsoft.Network/networkWatchers/connectionMonitors/query/action   | Sorgu bir bağlantı İzleyicisi                                     |
| Microsoft.Network/networkWatchers/connectionMonitors/read           | Bağlantı İzleyicisi Al                                       |
| Microsoft.Network/networkWatchers/connectionMonitors/write          | Bağlantı izleyicisi oluşturma                                    |
| Microsoft.Network/networkWatchers/connectionMonitors/delete         | Bağlantı izleyicisini Sil                                    |

## <a name="packet-capture"></a>Paket yakalama

| Eylem                                                              | Ad                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/packetCaptures/queryStatus/action | Paket yakalaması durumu sorgu                           |
| Microsoft.Network/networkWatchers/packetCaptures/stop/action        | Paket Yakalamayı Durdur                                          |
| Microsoft.Network/networkWatchers/packetCaptures/read               | Paket yakalaması Al                                           |
| Microsoft.Network/networkWatchers/packetCaptures/write              | Paket Yakalaması oluşturma                                        |
| Microsoft.Network/networkWatchers/packetCaptures/delete             | bir paket yakalamasını Sil                                        |

## <a name="ip-flow-verify"></a>IP akışı doğrulama

| Eylem                                                              | Ad                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/ipFlowVerify/action               | IP akışı doğrulama                                              |

## <a name="next-hop"></a>Sonraki atlama

| Eylem                                                              | Ad                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/nextHop/action                    | Sonraki atlama bir VM'den Al                                     |

## <a name="network-security-group-view"></a>Ağ güvenlik grubu görünümü

| Eylem                                                              | Ad                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/securityGroupView/action          | Güvenlik grupları görüntüleyin                                           |

## <a name="topology"></a>Topoloji

| Eylem                                                              | Ad                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/topology/action                   | Topoloji Al                                                   |

## <a name="reachability-report"></a>Erişilebilirlik rapor

| Eylem                                                              | Ad                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/azureReachabilityReport/action    | Bir Azure ulaşılabilirlik rapor alma                               |

## <a name="additional-actions"></a>Ek Eylemler

Ağ İzleyicisi özellikleri, aşağıdaki eylemleri de gerektirir:

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