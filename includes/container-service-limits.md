---
title: include dosyası
description: include dosyası
services: container-service
author: dlepow
ms.service: container-service
ms.topic: include
ms.date: 08/31/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 71294824bd3dd5215c388cfcd44382c7eee123ad
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2018
ms.locfileid: "48874083"
---
| Kaynak | Varsayılan limit |
| --- | :--- |
| Abonelik başına en fazla küme | 100 |
| Küme başına en fazla düğüm | 100 |
| Düğüm başına en fazla pod: Kubenet ile [temel ağ][basic-networking] | 110 |
| Düğüm başına en fazla pod: Azure CNI ile [gelişmiş ağ][advanced-networking] | Azure CLI dağıtımı: 30<sup>1</sup><br />Resource Manager şablonu: 30<sup>1</sup><br />Portal dağıtımı: 30 |

<sup>1</sup> Bu değer, bir AKS kümesini Azure CLI’si veya Resource Manager şablonu ile dağıtırken küme dağıtımı sırasında yapılandırılabilir.<br />

<!-- LINKS - Internal -->
[basic-networking]: ../articles/aks/networking-overview.md#basic-networking
[advanced-networking]: ../articles/aks/networking-overview.md#advanced-networking

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
