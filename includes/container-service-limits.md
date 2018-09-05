---
title: include dosyası
description: include dosyası
services: container-service
author: mmacy
ms.service: container-service
ms.topic: include
ms.date: 08/31/2018
ms.author: marsma
ms.custom: include file
ms.openlocfilehash: 1e8913d31677f3da9b6eb9d2216d8d9ec8b186b4
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2018
ms.locfileid: "43666912"
---
| Kaynak | Varsayılan limit |
| --- | :--- |
| Abonelik başına en fazla küme | 100 |
| Küme başına en fazla düğüm | 100 |
| Düğüm başına en fazla pod: Kubenet ile [temel ağ][basic-networking] | 110 |
| Düğüm başına en fazla pod: Azure CNI ile [gelişmiş ağ][advanced-networking] | Azure CLI dağıtımı: 110<sup>1</sup><br />Resource Manager şablonu: 110<sup>1</sup><br />Portal dağıtımı: 30 |

<sup>1</sup> Bu değer, bir AKS kümesini Azure CLI’si veya Resource Manager şablonu ile dağıtırken küme dağıtımı sırasında yapılandırılabilir.<br />

<!-- LINKS - Internal -->
[basic-networking]: ../articles/aks/networking-overview.md#basic-networking
[advanced-networking]: ../articles/aks/networking-overview.md#advanced-networking

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
