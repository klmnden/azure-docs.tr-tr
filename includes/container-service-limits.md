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
ms.openlocfilehash: 23c25953d2f493d2dd799bfd11dbbb69db002d1b
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2019
ms.locfileid: "55736030"
---
| Kaynak | Varsayılan limit |
| --- | :--- |
| Abonelik başına en fazla küme | 100 |
| Küme başına en fazla düğüm | 100 |
| En fazla düğüm başına pod'ları: [Temel ağ] [ basic-networking] ile Kubernetes | 110 |
| En fazla düğüm başına pod'ları: [Gelişmiş Ağ] [ advanced-networking] Azure CNI ile | Azure CLI dağıtım: 30<sup>1</sup><br />Resource Manager şablonu: 30<sup>1</sup><br />Portal Dağıtım: 30 |

<sup>1</sup> Azure CLI veya Resource Manager şablonuyla bir AKS kümesi dağıttığınızda bu değer **düğüm başına 110 pod'a** kadar yapılandırılabilir. Bir AKS kümesini dağıttıktan sonra veya Azure portal ile küme dağıttığınızda düğüm başına maksimum pod sayısını yapılandıramazsınız.<br />

<!-- LINKS - Internal -->
[basic-networking]: ../articles/aks/concepts-network.md#kubenet-basic-networking
[advanced-networking]: ../articles/aks/concepts-network.md#azure-cni-advanced-networking

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
