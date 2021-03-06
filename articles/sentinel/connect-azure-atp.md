---
title: Azure ATP verilere Azure Önizleme Gözcü | Microsoft Docs
description: Azure ATP veri Azure Gözcü için bağlanmayı öğreneceksiniz.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 5bf3cc44-ecda-4c78-8a63-31ab42f43605
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: 77f745f92133f4f43cd2a65f2b69ded1eff9e8ed
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67620624"
---
# <a name="connect-data-from-azure-advanced-threat-protection-atp"></a>Azure Gelişmiş tehdit Koruması (ATP gelen) veri bağlama

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


Günlüklerinden akışını [Azure Gelişmiş tehdit koruması](https://docs.microsoft.com/azure-advanced-threat-protection/what-is-atp) Azure Gözcü tek bir tıklamayla içine.

## <a name="prerequisites"></a>Önkoşullar

- Genel yönetici veya güvenlik yöneticisi izinleri ile kullanıcı
- Azure ATP özel Önizleme müşterisi olmalıdır

## <a name="connect-to-azure-atp"></a>Azure ATP için Bağlan

Azure ATP özel Önizleme sürümü olduğundan emin olun [ağınızda etkin](https://docs.microsoft.com/azure-advanced-threat-protection/install-atp-step1).
Varsa Azure ATP dağıtılan ve verileriniz başlayan kümeniz, şüpheli uyarıları kolayca Azure Gözcü aktarılabilir. Bu, Azure Gözcü akışını başlatmak uyarılar için 24 saate kadar sürebilir.



1. Azure Gözcü içinde seçin **veri bağlayıcıları** ve ardından **Azure ATP** Döşe.

2. **Bağlan**'a tıklayın.

6. İlgili şema Log Analytics'te Azure ATP uyarılarını kullanmak için arama **SecurityAlert**.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Gözcü için Azure Gelişmiş tehdit koruması bağlanma öğrendiniz. Azure Gözcü hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- Bilgi nasıl [görünürlük almak, veri ve olası tehditleri](quickstart-get-visibility.md).
- Başlama [Azure Gözcü kullanarak tehditleri algılama](tutorial-detect-threats.md).

