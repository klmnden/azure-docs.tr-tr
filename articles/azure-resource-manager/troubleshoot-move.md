---
title: Azure kaynakları için yeni abonelik veya kaynak grubu taşırken hatalarını giderme
description: Kaynakları yeni kaynak grubuna veya aboneliğe taşıma için Azure Resource Manager'ı kullanın.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: tomfitz
ms.openlocfilehash: e23d7c571a010e5bfb42e5f15368e0194272ed53
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67723473"
---
# <a name="troubleshoot-moving-azure-resources-to-new-resource-group-or-subscription"></a>Azure kaynakları yeni kaynak grubuna veya aboneliğe taşıma sorunlarını giderme

Bu makalede kaynakları taşırken sorunları çözmenize yardımcı olacak öneriler sunar.

## <a name="upgrade-a-subscription"></a>Bir aboneliği yükseltme

Gerçekte Azure aboneliğiniz (örneğin, boş, Kullandıkça Öde aboneliğine geçiş) yükseltmek istiyorsanız, aboneliğinizin dönüştürmeniz gerekir.

* Ücretsiz deneme sürümü yükseltmek için bkz: [ücretsiz deneme sürümü ya da Microsoft Imagine Azure aboneliğinizi Kullandıkça Öde aboneliğine yükseltme](../billing/billing-upgrade-azure-subscription.md).
* Bir Kullandıkça Öde hesabına değiştirmek için bkz [Azure Kullandıkça Öde aboneliğinizi değiştirmek için farklı bir teklif](../billing/billing-how-to-switch-azure-offer.md).

Abonelik dönüştüremezse [bir Azure destek isteği oluşturma](../azure-supportability/how-to-create-azure-support-request.md). Seçin **abonelik yönetimi** sorun türü için.

## <a name="service-limitations"></a>Hizmet sınırlamaları

Kaynakları taşırken bazı hizmetler için başka konular gerekli. Aşağıdaki Hizmetleri taşıyorsanız, sınırlamaları ve yönergeleri denetlediğinizden emin olun.

* [Uygulama Hizmetleri](./move-limitations/app-service-move-limitations.md)
* [Azure DevOps Hizmetleri](/azure/devops/organizations/billing/change-azure-subscription?toc=/azure/azure-resource-manager/toc.json)
* [Klasik dağıtım modeli](./move-limitations/classic-model-move-limitations.md)
* [Kurtarma Hizmetleri](../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json)
* [Sanal Makineler](./move-limitations/virtual-machines-move-limitations.md)
* [Sanal Ağlar](./move-limitations/virtual-network-move-limitations.md)

## <a name="large-requests"></a>Büyük istekleri

Mümkün olduğunda, kesme büyük ayrı taşıma işlemlerini taşır. Tek bir işlemde kaynakları 800'den fazla olduğunda, Kaynak Yöneticisi'ni hemen bir hata döndürür. Ancak, 800'den daha az kaynağı taşımadan da zaman aşımına göre başarısız olabilir.

## <a name="next-steps"></a>Sonraki adımlar

Kaynakları taşıma komutlar için bkz [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](resource-group-move-resources.md).
