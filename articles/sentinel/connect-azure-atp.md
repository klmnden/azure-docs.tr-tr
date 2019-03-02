---
title: Gözcü Azure önizlemede Azure ATP veri toplama | Microsoft Docs
description: Azure Gözcü içinde Azure ATP verilerini nasıl toplayacağınızı öğrenin.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 5bf3cc44-ecda-4c78-8a63-31ab42f43605
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/30/2019
ms.author: rkarlin
ms.openlocfilehash: 154af3988084792331db082b99cae0ae06126f1b
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2019
ms.locfileid: "57242182"
---
# <a name="collect-data-from-azure-advanced-threat-protection-atp"></a>Gelen Azure Gelişmiş tehdit Koruması (ATP) veri toplama

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



1. Azure Gözcü içinde seçin **veri toplama** ve ardından **Azure ATP** Döşe.

2. **Bağlan**'a tıklayın.


## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Gözcü için Azure Gelişmiş tehdit koruması bağlanma öğrendiniz. Azure Gözcü hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- Bilgi nasıl [görünürlük almak, veri ve olası tehditleri](quickstart-get-visibility.md).
- Başlama [Azure Gözcü kullanarak tehditleri algılama](tutorial-detect-threats.md).

