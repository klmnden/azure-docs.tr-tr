---
title: Gözcü Azure önizlemede Azure Information Protection veri toplama | Microsoft Docs
description: Gözcü Azure, Azure Information Protection verilerini nasıl toplayacağınızı öğrenin.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: bfa2eca4-abdc-49ce-b11a-0ee229770cdd
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/6/2019
ms.author: rkarlin
ms.openlocfilehash: 7c5866d3096823f91a70b28c7c5dd1790e1b3bf8
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57537176"
---
# <a name="collect-data-from-azure-information-protection"></a>Azure Information Protection tarafından sağlanan veri toplama

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Günlüklerinden akışını [Azure Information Protection](https://docs.microsoft.com/azure/information-protection/reports-aip) Azure Gözcü tek bir tıklamayla içine. Azure Information Protection, bulutta veya şirket içi altyapılarda ve denetim ve Yardım güvenli e-posta, belgeler ve şirketinizin dışındaki kişilerle paylaştığınız hassas verileri saklanıyor olsun, verilerinizin korunmasına yardımcı olur. Kolay sınıflandırmadan tümleşik etiket ve izinlere, Azure Information Protection ile veri koruması her zaman artırın. Azure Information Protection Azure Gözcü, akış için tüm uyarılar Azure Information Protection'dan Azure Gözcü bağladığınızda.


## <a name="prerequisites"></a>Önkoşullar

- Genel yönetici, güvenlik yöneticisi veya bilgi koruma izinleri olan kullanıcı


## <a name="connect-to-azure-information-protection"></a>Azure Information Protection'a bağlanmak

Azure Information Protection'ı zaten varsa, olduğundan emin olun [ağınızda etkin](https://docs.microsoft.com/azure/information-protection/activate-service).
Azure Information Protection dağıtılan ve veri alma, uyarı verileri kolayca Azure Gözcü aktarılabilir.


1. Azure Gözcü içinde seçin **veri toplama** ve ardından **Azure Information Protection** Döşe.

2. Git [Azure Information Protection portalı](https://portal.azure.com/?ScannerConfiguration=true&EndpointDiscovery=true#blade/Microsoft_Azure_InformationProtection/DataClassGroupEditBlade/quickstartBlade) 

3. Altında **bağlantı**, günlüklerin Azure Information Protection için Azure Gözcü tıklayarak akış yukarı ayarlanan [yapılandırın](https://portal.azure.com/#blade/Microsoft_Azure_InformationProtection/DataClassGroupEditBlade/analyticsOnboardBlade)

4. Hangi Azure Gözcü dağıttıysanız çalışma alanı seçin. 

5. **Tamam** düğmesine tıklayın.

6. İlgili şema Log Analytics'te Azure Information Protection uyarılarını kullanmak için arama **InformationProtectionLogs_CL**.




## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Information Protection için Azure Gözcü bağlanmak öğrendiniz. Azure Gözcü hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- Bilgi nasıl [görünürlük almak, veri ve olası tehditleri](quickstart-get-visibility.md).
- Başlama [Azure Gözcü kullanarak tehditleri algılama](tutorial-detect-threats.md).
