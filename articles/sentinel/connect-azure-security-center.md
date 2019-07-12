---
title: Gözcü Azure Önizleme için Azure Güvenlik Merkezi verilere bağlanma | Microsoft Docs
description: Azure Gözcü için Azure Güvenlik Merkezi veri bağlanmayı öğreneceksiniz.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: d28c2264-2dce-42e1-b096-b5a234ff858a
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: 697983000f84f9e4efe7e2c8ef9dbb2f636e0735
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67620571"
---
# <a name="connect-data-from-azure-security-center"></a>Azure Güvenlik Merkezi'nden veri bağlama

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).



Azure Sentinel uyarılardan bağlamanıza olanak sağlar [Azure Güvenlik Merkezi](../security-center/security-center-intro.md) ve bunları Azure Gözcü akışa. 

## <a name="prerequisites"></a>Önkoşullar

- Azure Güvenlik Merkezi'nden uyarı dışarı aktarmak istiyorsanız, günlükleri, akış abonelik üzerinde katkıda bulunan olması gerekir.

- Olmalıdır [Azure Güvenlik Merkezi standart katmanı](../security-center/security-center-pricing.md) abonelik üzerinde çalışıyor. Aksi halde [aboneliğinizi standart sürümüne yükseltebilir](https://azure.microsoft.com/pricing/details/security-center/).

- Genel yönetici veya güvenlik yöneticisi izinleri bağlamak istediğiniz her abonelikte bulunan bir kullanıcı oturum gerekir.


## <a name="connect-to-azure-security-center"></a>Azure Güvenlik Merkezi'ne bağlama

1. Azure Gözcü içinde seçin **veri bağlayıcıları** ve ardından **Azure Güvenlik Merkezi** Döşe.
1. Sağ, **Connect** uyarılar Azure Gözcü akışı yapmak istediğiniz her abonelik yanında. Her abonelik Azure Gözcü stream uyarıların Azure Güvenlik Merkezi standart katmanına yükseltin emin olun.

3. **Bağlan**'a tıklayın.

4. İlgili şema Log Analytics'te Azure Güvenlik Merkezi uyarılarını kullanmak için arama **SecurityEvent**.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Güvenlik Merkezi, Azure Gözcü için bağlama öğrendiniz. Azure Gözcü hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- Bilgi nasıl [görünürlük almak, veri ve olası tehditleri](quickstart-get-visibility.md).
- Başlama [Azure Gözcü kullanarak tehditleri algılama](tutorial-detect-threats.md).
