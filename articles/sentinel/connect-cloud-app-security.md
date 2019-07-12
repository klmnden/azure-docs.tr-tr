---
title: Gözcü Azure Önizleme için cloud App Security veri bağlayın | Microsoft Docs
description: Azure Gözcü için cloud App Security veri bağlanmayı öğreneceksiniz.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: cd9e5e27-fdd4-4717-8924-be4c1c430f23
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: 4e75c9003103e267e864a98c7ee5c1bef2176bae
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67612398"
---
# <a name="connect-data-from-microsoft-cloud-app-security"></a>Microsoft Cloud App Security'den veri bağlama 

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Günlüklerinden akışını [Cloud App Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security) Azure Gözcü tek bir tıklamayla içine. Bu bağlantı, Cloud App Security uyarılardan Azure Gözcü akışını sağlar. 

## <a name="prerequisites"></a>Önkoşullar

- Genel yönetici veya güvenlik yöneticisi izinleri ile kullanıcı

## <a name="connect-to-cloud-app-security"></a>Cloud App Security'ye bağlama

Cloud App Security zaten varsa, olduğundan emin olun [ağınızda etkin](https://docs.microsoft.com/cloud-app-security/getting-started-with-cloud-app-security).
Cloud App Security dağıtılan ve verileriniz başlayan kümeniz, uyarı verileri kolayca Azure Gözcü aktarılabilir.


1. Azure Gözcü içinde seçin **veri bağlayıcıları** ve ardından **Cloud App Security** Döşe.

2. **Bağlan**'a tıklayın.

3. İlgili şema için Cloud App Security uyarıları Log Analytics'te kullanmak için arama **SecurityAlert**.


## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Gözcü için Microsoft Cloud App Security bağlanma öğrendiniz. Azure Gözcü hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- Bilgi nasıl [görünürlük almak, veri ve olası tehditleri](quickstart-get-visibility.md).
- Başlama [Azure Gözcü kullanarak tehditleri algılama](tutorial-detect-threats.md).
