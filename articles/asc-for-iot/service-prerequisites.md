---
title: IOT önkoşulları Önizleme için Azure Güvenlik Merkezi | Microsoft Docs
description: Azure Güvenlik Merkezi ile IOT hizmet önkoşulları kullanmaya başlamak için gereken her şeyi ayrıntıları.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 790cbcb7-1340-4cc1-9509-7b262e7c3181
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: 198459887ff19b16e897b2a8dde55bca1217c8ac
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67616445"
---
# <a name="azure-security-center-for-iot-prerequisites"></a>Azure Güvenlik Merkezi IOT önkoşulları

> [!IMPORTANT]
> IOT için Azure Güvenlik Merkezi şu anda genel Önizleme aşamasındadır.
> Bu önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanmaktadır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu makalede, farklı yapı taşları, Azure Güvenlik Merkezi (hizmet anlamanıza yardımcı olması için ASC) IOT hizmeti, başlamak için ihtiyacınız olanlar ve temel kavramlar için bir açıklama sağlar. 

## <a name="minimum-requirements"></a>En düşük gereksinimleri

- IOT hub'ı standart katman
    - RBAC rolü **sahibi** düzeyi ayrıcalıklar 
- [Log Analytics çalışma alanı](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace) 
- Azure Güvenlik Merkezi (önerilir)
    - Azure Güvenlik Merkezi kullanımı yalnızca bir öneri ve bu olmadan, bir gereksinim değil olsa diğer Azure kaynaklarınızı IOT hub'ının içinden görüntülemeniz mümkün olmayacaktır. 
 
## <a name="working-with-asc-for-iot-service"></a>IOT hizmeti için ASC ile çalışma

ASC IOT öngörüleri ve raporlama için Azure IOT Hub ve Azure Güvenlik Merkezi kullanılarak kullanılabilir. ASC bir hesapla Azure IOT Hub, IOT için etkinleştirmek için **sahibi** düzeyinde ayrıcalıkları gereklidir. IOT hub'ınızdaki IOT için ASC etkinleştirdikten sonra IOT öngörüleri için ASC olarak görüntülenir **güvenlik** özellik olarak ve Azure IOT hub'ında **IOT** Azure Güvenlik Merkezi'nde. 

## <a name="supported-service-regions"></a>Desteklenen hizmet bölgeleri 

ASC IOT için şu anda aşağıdaki bölgelerde Azure IOT hub'ları için desteklenir:
  - Orta ABD
  - Kuzey Avrupa
  - Güneydoğu Asya

## <a name="wheres-my-iot-hub"></a>IOT hub'ı nerede?

Başlamadan önce hizmet kullanılabilirliğini doğrulamak için IOT hub'ı konumu kontrol edin. 

1. IOT Hub'ınızı açın. 
2. **Genel Bakış**'a tıklayın. 
3. Listelenen konumuyla eşleşen birini doğrulayın [hizmet bölgeleri desteklenen](#supported-service-regions). 


## <a name="supported-platforms-for-agents"></a>Aracılar için desteklenen platformlar 

ASC IOT aracılar için artan cihaz ve platformları destekler. Bkz: [platform listesine desteklediği](how-to-deploy-agent.md) mevcut veya planlanan cihaz kitaplığınızı denetlemek için.  

## <a name="next-steps"></a>Sonraki adımlar
- [Genel bakış](overview.md)
- [Hizmeti etkinleştirme](quickstart-onboard-iot-hub.md)
- [ASC için IOT hakkında SSS](resources-frequently-asked-questions.md)
- [ASC IOT uyarılarını anlama](concept-security-alerts.md)
