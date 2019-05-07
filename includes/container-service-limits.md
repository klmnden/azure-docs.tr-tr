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
ms.openlocfilehash: a2729af6a689daa551fc01f585324d53a8770a9b
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65072872"
---
| Resource | Varsayılan limit |
| --- | :--- |
| Abonelik başına en fazla kümeleri | 100 |
| Küme başına en fazla düğüm | 100 |
| Düğüm başına en fazla pod: [Temel ağ] [ basic-networking] ile Kubernetes | 110 |
| Düğüm başına en fazla pod: [Gelişmiş Ağ] [ advanced-networking] Azure kapsayıcı ağ arabirimi | Azure CLI dağıtım: 30<sup>1</sup><br />Azure Resource Manager şablonu: 30<sup>1</sup><br />Portal Dağıtım: 30 |

<sup>1</sup>, Azure CLI veya Resource Manager şablonu ile bir Azure Kubernetes Service (AKS) kümesini dağıtırken, bu değer 250 pod'ların düğüm başına en fazla yapılandırılabilir. Zaten bir AKS kümesi dağıttıktan sonra veya Azure portalını kullanarak bir kümesi dağıtıyorsanız, düğüm başına en fazla pod'ların yapılandıramazsınız.<br />

<!-- LINKS - Internal -->
[basic-networking]: ../articles/aks/concepts-network.md#kubenet-basic-networking
[advanced-networking]: ../articles/aks/concepts-network.md#azure-cni-advanced-networking

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
