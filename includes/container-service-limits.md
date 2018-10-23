---
title: include dosyası
description: include dosyası
services: container-service
author: dlepow
ms.service: container-service
ms.topic: include
ms.date: 10/11/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 4251f379c517d5ccfd0430987e3d5280208590ff
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49400150"
---
| Kaynak | Varsayılan limit |
| --- | :--- |
| Abonelik başına en fazla küme | 100 |
| Küme başına en fazla düğüm | 100 |
| Düğüm başına en fazla pod: Kubenet ile [temel ağ][basic-networking] | 110 |
| Düğüm başına en fazla pod: Azure CNI ile [gelişmiş ağ][advanced-networking] | Azure CLI dağıtımı: 30<sup>1</sup><br />Resource Manager şablonu: 30<sup>1</sup><br />Portal dağıtımı: 30 |

<sup>1</sup> Azure CLI veya Resource Manager şablonuyla bir AKS kümesi dağıttığınızda bu değer **düğüm başına 110 pod'a** kadar yapılandırılabilir. Bir AKS kümesini dağıttıktan sonra veya Azure portal ile küme dağıttığınızda düğüm başına maksimum pod sayısını yapılandıramazsınız.<br />

<!-- LINKS - Internal -->
[basic-networking]: ../articles/aks/concepts-network.md#basic-networking
[advanced-networking]: ../articles/aks/concepts-network.md#advanced-networking

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
