---
title: Gözcü Azure önizlemede Azure Güvenlik Merkezi veri toplama | Microsoft Docs
description: Gözcü Azure, Azure Güvenlik Merkezi verilerini nasıl toplayacağınızı öğrenin.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: d28c2264-2dce-42e1-b096-b5a234ff858a
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: f59c5f6a9f497a6420172996f9f327f16ffd26f9
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2019
ms.locfileid: "57242284"
---
# <a name="collect-data-from-azure-security-center"></a>Azure Güvenlik Merkezi veri toplama

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).



Azure Sentinel uyarılardan toplamanızı sağlar [Azure Güvenlik Merkezi](../security-center/security-center-intro.md) ve bunları Azure Gözcü akışa. 

## <a name="prerequisites"></a>Önkoşullar

- Azure Güvenlik Merkezi'nden uyarı dışarı aktarmak istiyorsanız, günlükleri, akış abonelik üzerinde katkıda bulunan olması gerekir.

- Olmalıdır [Azure Güvenlik Merkezi standart katmanı](../security-center/security-center-pricing.md) abonelik üzerinde çalışıyor. Aksi halde [aboneliğinizi standart sürümüne yükseltebilir](https://azure.microsoft.com/pricing/details/security-center/).

- Genel yönetici veya güvenlik yöneticisi izinleri bağlamak istediğiniz her abonelikte bulunan bir kullanıcı oturum gerekir.


## <a name="connect-to-azure-security-center"></a>Azure Güvenlik Merkezi'ne bağlama

1. Azure Gözcü içinde seçin **veri toplama** ve ardından **Azure Güvenlik Merkezi** Döşe.
1. Sağ, **Connect** uyarılar Azure Gözcü akışı yapmak istediğiniz her abonelik yanında. Her abonelik Azure Gözcü stream uyarıların Azure Güvenlik Merkezi standart katmanına yükseltin emin olun.

3. **Bağlan**'a tıklayın.

4. İlgili şema Log Analytics'te Azure Güvenlik Merkezi uyarılarını kullanmak için arama **SecurityEvent**.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Güvenlik Merkezi, Azure Gözcü için bağlama öğrendiniz. Azure Gözcü hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- Bilgi nasıl [görünürlük almak, veri ve olası tehditleri](quickstart-get-visibility.md).
- Başlama [Azure Gözcü kullanarak tehditleri algılama](tutorial-detect-threats.md).
