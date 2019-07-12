---
title: Azure Güvenlik Merkezi'nde güvenlik uyarıları | Microsoft Docs
description: Bu konu, güvenlik uyarıları ve Azure Güvenlik Merkezi'nde kullanılabilen farklı türlerin ne olduğunu açıklar.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: 1b71e8ad-3bd8-4475-b735-79ca9963b823
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: monhaber
ms.openlocfilehash: 06f41bbfd97d5deb59e7bfd08615b2f28e256070
ms.sourcegitcommit: 1e347ed89854dca2a6180106228bfafadc07c6e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67571679"
---
# <a name="security-alerts-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde güvenlik uyarıları

Bu makalede, Azure Güvenlik Merkezi (ASC) mevcut olan güvenlik uyarı farklı türleri gösterir. Uyarılar birçok farklı kaynak türleri için çeşitli vardır. ASC, Azure'da dağıtılan iki kaynaklar için uyarılar oluşturur ve şirket içi ve karma bulut ortamlarına dağıtılan kaynakları. 

## <a name="what-are-security-alerts"></a>Güvenlik uyarıları nedir?

Güvenlik Merkezi, tehditleri kaynaklarınızın algıladığında oluşturduğu bildirimler uyarılardır. Bu öncelik veren ve uyarılar hızlı bir şekilde sorunu araştırmak için gereken bilgilerle birlikte listeler. Güvenlik Merkezi, ayrıca nasıl bir saldırıyı düzeltmek öneriler sağlar.

## <a name="how-does-security-center-detect-threats"></a>Güvenlik Merkezi'nin nasıl tehditleri algılar?

Gerçek tehditleri algılamak ve hatalı pozitif sonuçları azaltmak için Güvenlik Merkezi toplar, çözümler ve günlük verilerini Azure kaynaklarınızdan, ağ ve güvenlik duvarı gibi bağlı iş ortağı çözümleri ve uç nokta koruma çözümleri tümleştirir. Güvenlik Merkezi, bu bilgiler genellikle tehditleri tanımlamak için birden fazla kaynaktan bilgileri ilişkilendirerek analiz eder.

ASC, Azure'da dağıtılan veya diğer şirket içi ve karma bulut ortamlarına dağıtılan kaynakları izler. Algılama ve tehditlerine yanıt verme hakkında daha fazla bilgi için bkz. [nasıl Güvenlik Merkezi algılar ve bu tehditlere yanıt](security-center-detection-capabilities.md#asc-detects).

## <a name="security-alert-types"></a>Güvenlik Uyarısı türleri

Aşağıdaki konular kaynak türleri göre farklı ASC uyarılar size kılavuzluk eder:

* [Iaas Vm'leri ve sunucular uyarıları](security-center-alerts-iaas.md)
* [Yerel işlem uyarıları](security-center-alerts-compute.md)
* [Veri Hizmetleri uyarıları](security-center-alerts-data-services.md)

Aşağıdaki konularda, Güvenlik Merkezi, Azure'da dağıtılan kaynaklar için ek koruma katmanları uygulamak için Azure altyapı ile tümleştirme toplayan farklı telemetri nasıl yararlanır açıklanır:

* [Hizmet katmanı uyarıları](security-center-alerts-service-layer.md)
* [Azure güvenlik ürünleri ile tümleştirme](security-center-alerts-integration.md)

## <a name="what-are-alert-incidents"></a>Uyarı olayları nelerdir?

Bir güvenlik olayı, her bir uyarı ayrı ayrı listelemek yerine ilgili uyarıların koleksiyonudur. Güvenlik Merkezi, güvenlik olaylara farklı uyarıların ve düşük güvenilirliğe sahip sinyalleri ilişkilendirmek için fusion kullanır.

Olayları kullanarak, Güvenlik Merkezi ile bir saldırı kampanyasını ve tüm ilgili uyarıları tek bir görünüm sağlar. Bu görünüm, saldırganın aldığı hangi işlemleri yaptığını ve hangi kaynakların etkilendiğini hızlıca anlamak sağlar. Daha fazla bilgi için [bulut akıllı uyarı bağıntısı](security-center-alerts-cloud-smart.md).

## <a name="get-started-with-alerts"></a>Uyarıları ile çalışmaya başlama

ASC tarafından sunulan uyarılar yanıtlamayla ilgili yönergeler yanı sıra, ASC tarafından izlenen kaynakları hakkında daha fazla anlamak için aşağıdaki konulara bakın.

* ASC tarafından hangi platformlar ve Özellikler korumalı olduğunu görmek için bkz: [platformları ve Azure Güvenlik Merkezi tarafından desteklenen özellikler](security-center-os-coverage.md).  
* Güvenlik olaylarını nelerdir ve ASC onlara nasıl yanıt vereceğini anlamak için bkz: [Azure Güvenlik Merkezi'nde güvenlik olaylarını işlemek nasıl](security-center-incident.md). 
* Aldığınız uyarıların nasıl yönetileceği bilgi edinmek için [yönetme ve Azure Güvenlik Merkezi'nde güvenlik uyarılarını yanıtlama](security-center-managing-and-responding-alerts.md).
* Güvenlik Merkezi düzgün şekilde yapılandırıldığını nasıl bilgilerinin doğrulamak ve bir test uyarısı arttırın için bkz [Azure Güvenlik Merkezi'nde uyarıları doğrulama](security-center-alert-validation.md).  


## <a name="upgrade-to-standard-for-advanced-detections"></a>Gelişmiş algılamaları için standart sürümüne yükseltme

Gelişmiş algılamaları ayarlamak için Azure Güvenlik Merkezi Standart sürümüne yükseltme yapın. 

1. Güvenlik Merkezi menüden **Güvenlik İlkesi**.
2. Standart katmana taşımak istediğiniz abonelikleri için tıklayın **ayarlarını Düzenle**. 
3. Ayarları sayfasından seçin **fiyatlandırma katmanı**. 
   Ücretsiz deneme sürümü bir ay için kullanılabilir. Daha fazla bilgi edinmek için [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/). 

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, güvenlik uyarılarını ve Güvenlik Merkezi'nde uyarıları farklı türde ne olduğunu öğrendiniz. Daha fazla bilgi için aşağıdaki konulara bakın:

* [Azure Güvenlik Merkezi planlama ve işlemler kılavuzu](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide)
* [Azure Güvenlik Merkezi SSS](https://docs.microsoft.com/azure/security-center/security-center-faq): Hizmet kullanımı ile ilgili sık sorulan soruları bulun.

