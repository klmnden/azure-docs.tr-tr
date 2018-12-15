---
title: Azure Batch AI neler oluyor? | Microsoft Docs
description: Azure Batch AI ve Azure Machine Learning hizmetinin işlem seçeneği için neler olduğu hakkında bilgi edinin.
services: batch-ai
author: garyericson
ms.service: batch-ai
ms.topic: overview
ms.date: 12/14/2018
ms.author: garye
ms.openlocfilehash: 6803a47ae77c072eff05df65e37156c90aabf3e6
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2018
ms.locfileid: "53436884"
---
# <a name="whats-happening-to-azure-batch-ai"></a>Azure Batch AI neler oluyor?

**Azure Batch AI hizmeti Mart ayında devre dışı bırakılıyor.** Ölçekli eğitim ve puanlama Batch AI yeteneklerini de kullanıma sunulmuştur [Azure Machine Learning hizmeti](../machine-learning/service/overview-what-is-azure-ml.md), hangi genel olarak kullanılabilir hale 4 Aralık 2018'de.

Makine öğrenimi özellikleri birçok diğer yanı sıra, Azure Machine Learning hizmeti, eğitim, dağıtma ve makine öğrenimi modellerini Puanlama için yönetilen bir bulut tabanlı bilgi işlem hedef içerir. Bu işlem hedef adlı [Azure Machine Learning işlem](../machine-learning/service/how-to-set-up-training-targets.md#amlcompute). [Geçiş ve bugün kullanmaya başlayın](#migrate). Azure Machine Learning hizmeti aracılığıyla etkileşim kurabilir, [Python SDK'ları](../machine-learning/service/quickstart-create-workspace-with-python.md), komut satırı arabirimi ve [Azure portalında](../machine-learning/service/quickstart-get-started.md).

## <a name="support-timeline"></a>Destek zaman çizelgesi

| Tarih | Batch AI hizmeti destek ayrıntıları |
| ---- |-----------------|
| Aralık&nbsp;14&#x2c;&nbsp;2018| Önce mevcut Azure Batch AI abonelikler olarak kullanmaya devam edin. Ancak, kaydetme **yeni abonelikler** hiçbir uzun mümkündür ve bu hizmet için yeni hiçbir yatırım yapılacaktır.|
| Mart&nbsp;31&#x2c;&nbsp;2019 | Bu tarihten sonra mevcut Batch AI abonelikleri artık çalışmayacak. |

<a name="migrate"></a>
## <a name="how-do-i-migrate"></a>Nasıl geçiş yaparım?

Uygulamalarınıza kesintilerinden kaçının ve en son özelliklerden yararlanmak için 31 Mart 2019'den önce aşağıdaki adımları uygulayın:

1. Bir Azure Machine Learning hizmeti çalışma alanı oluşturun ve kullanmaya başlayın:
    + [Python tabanlı hızlı başlangıç](../machine-learning/service/quickstart-create-workspace-with-python.md)
    + [Azure portal tabanlı hızlı başlangıç](../machine-learning/service/quickstart-get-started.md)

1. Ayarlanmış bir [Azure Machine Learning işlem](../machine-learning/service/how-to-set-up-training-targets.md#amlcompute) modeli eğitimi için.

1. Azure Machine Learning işlem kullanmak için komut dosyalarını güncelleştirin.

## <a name="support"></a>Destek

Destek, mevcut müşteriler için daha kapsamlı geçirmek isteyen kullanılabilir [Azure Machine Learning hizmeti](https://aka.ms/aml-docs).

Azure Machine Learning hizmeti karşılamıyorsa desteklenen bir işlevsellik Batch AI hizmeti, açık bir Batch AI destek mevcut olduğu gereksiniminizi isteği güvenilir listeye eklenecek destek ekibi ile Batch AI hizmeti devre dışı bırakılmasına kadar kullanmak için aboneliğinizi.

## <a name="next-steps"></a>Sonraki adımlar

+ Okuma [Azure Machine Learning hizmetine genel bakış](../machine-learning/service/overview-what-is-azure-ml.md).

+ [Bir işlem hedefine modeli eğitimi için yapılandırma](../machine-learning/service/how-to-set-up-training-targets.md) Azure Machine Learning hizmeti ile.

+ Gözden geçirme [Azure yol haritası](https://azure.microsoft.com/updates/) diğer Azure hizmet güncelleştirmeleri öğrenin.
