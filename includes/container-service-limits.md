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
ms.openlocfilehash: 62eb75ef18d3ac81be65783e57c21c0aefd7a429
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57554734"
---
| Kaynak | Varsayılan limit |
| --- | :--- |
| Abonelik başına en fazla kümeleri | 100 |
| Küme başına en fazla düğüm | 100 |
| Düğüm başına en fazla pod: [Temel ağ] [ basic-networking] ile Kubernetes | 110 |
| Düğüm başına en fazla pod: [Gelişmiş Ağ] [ advanced-networking] Azure kapsayıcı ağ arabirimi | Azure CLI dağıtım: 30<sup>1</sup><br />Azure Resource Manager şablonu: 30<sup>1</sup><br />Portal Dağıtım: 30 |

<sup>1</sup>, Azure CLI veya Resource Manager şablonu ile bir Azure Kubernetes Service (AKS) kümesini dağıtırken, bu değer 110 pod'ların düğüm başına en fazla yapılandırılabilir. Zaten bir AKS kümesi dağıttıktan sonra veya Azure portalını kullanarak bir kümesi dağıtıyorsanız, düğüm başına en fazla pod'ların yapılandıramazsınız.<br />

<!-- LINKS - Internal -->
[basic-networking]: ../articles/aks/concepts-network.md#kubenet-basic-networking
[advanced-networking]: ../articles/aks/concepts-network.md#azure-cni-advanced-networking

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
