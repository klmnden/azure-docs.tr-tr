---
title: Gözcü Azure Önizleme için Azure Information Protection verilere bağlanma | Microsoft Docs
description: Azure Information Protection verileri Azure Gözcü bağlanmayı öğreneceksiniz.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: bfa2eca4-abdc-49ce-b11a-0ee229770cdd
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: 4b73edd10521b23fb4befbe4fe7d9f0c7b496de3
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65204311"
---
# <a name="connect-data-from-azure-information-protection"></a>Azure Information Protection'dan veri bağlama

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Günlüklerinden akışını [Azure Information Protection](https://docs.microsoft.com/azure/information-protection/reports-aip) Azure Gözcü tek bir tıklamayla içine. Azure Information Protection, bulutta veya şirket içi altyapılarda ve denetim ve Yardım güvenli e-posta, belgeler ve şirketinizin dışındaki kişilerle paylaştığınız hassas verileri saklanıyor olsun, verilerinizin korunmasına yardımcı olur. Kolay sınıflandırmadan tümleşik etiket ve izinlere, Azure Information Protection ile veri koruması her zaman artırın. Azure Information Protection Azure Gözcü, akış için tüm uyarılar Azure Information Protection'dan Azure Gözcü bağladığınızda.


## <a name="prerequisites"></a>Önkoşullar

- Genel yönetici, güvenlik yöneticisi veya bilgi koruma izinleri olan kullanıcı


## <a name="connect-to-azure-information-protection"></a>Azure Information Protection'a bağlanmak

Azure Information Protection'ı zaten varsa, olduğundan emin olun [ağınızda etkin](https://docs.microsoft.com/azure/information-protection/activate-service).
Azure Information Protection dağıtılan ve veri alma, uyarı verileri kolayca Azure Gözcü aktarılabilir.


1. Azure Gözcü içinde seçin **veri bağlayıcıları** ve ardından **Azure Information Protection** Döşe.

2. Git [Azure Information Protection portalı](https://portal.azure.com/?ScannerConfiguration=true&EndpointDiscovery=true#blade/Microsoft_Azure_InformationProtection/DataClassGroupEditBlade/quickstartBlade) 

3. Altında **bağlantı**, günlüklerin Azure Information Protection için Azure Gözcü tıklayarak akış yukarı ayarlanan [yapılandırın](https://portal.azure.com/#blade/Microsoft_Azure_InformationProtection/DataClassGroupEditBlade/analyticsOnboardBlade)

4. Hangi Azure Gözcü dağıttıysanız çalışma alanı seçin. 

5. **Tamam** düğmesine tıklayın.

6. İlgili şema Log Analytics'te Azure Information Protection uyarılarını kullanmak için arama **InformationProtectionLogs_CL**.




## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Information Protection için Azure Gözcü bağlanmak öğrendiniz. Azure Gözcü hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- Bilgi nasıl [görünürlük almak, veri ve olası tehditleri](quickstart-get-visibility.md).
- Başlama [Azure Gözcü kullanarak tehditleri algılama](tutorial-detect-threats.md).
