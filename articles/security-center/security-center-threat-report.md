---
title: Azure Güvenlik Merkezi tehdit zekası raporu | Microsoft Docs
description: Bu belge, bir araştırma sırasında güvenlik uyarıları hakkında daha fazla bilgiye ulaşmak için Azure Güvenlik Merkezi Tehdit Zekası Raporlarını kullanmanıza yardımcı olur.
services: security-center
documentationcenter: na
author: terrylan
manager: mbaldwin
editor: ''
ms.assetid: 5662e312-e8c2-4736-974e-576eeb333484
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: yurid
ms.openlocfilehash: f0c1588633f548b740e6b9f6a7a3121ef791500a
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51234264"
---
# <a name="azure-security-center-threat-intelligence-report"></a>Azure Güvenlik Merkezi Tehdit Zekası Raporu
Bu belge, Azure Güvenlik Merkezi Tehdit Zekası Raporlarının, güvenlik uyarılarını oluşturan tehditler hakkında daha fazla bilgi edinmenize nasıl yardımcı olabileceğini açıklamaktadır.

## <a name="what-is-a-threat-intelligence-report"></a>Tehdit zekası rapor nedir?
Güvenlik Merkezi tehdit algılaması Azure kaynaklarınızdan, ağınızdan ve bağlı iş ortağı çözümlerinden güvenlik verilerini izleyerek çalışır. Tehditleri belirlemek amacıyla bu bilgileri genellikle birden fazla kaynaktan bilgileri ilişkilendirerek analiz eder. Bu işlem, Güvenlik Merkezi [tanılama özelliklerinin](security-center-detection-capabilities.md) bir parçasıdır.

Güvenlik Merkezi tarafından bir tehdit algılandığında, belirli bir olay için düzeltme önerileri de dahil olmak üzere ayrıntılı bilgiler içeren bir [güvenlik uyarısı](security-center-managing-and-responding-alerts.md) tetikler. Güvenlik Merkezi, olay yanıt ekiplerine tehdit araştırma ve düzeltme süreçlerinde yardımcı olmak üzere aşağıdaki bilgiler de dahil olmak üzere algılanan tehdit hakkında bilgiler içeren bir tehdit zekası raporu sunar:

* Saldırganların kimliği veya bağlantıları (bu konuda bilgi varsa)
* Saldırganların hedefleri
* Güncel ve geçmiş saldırı kampanyaları (bu konuda bilgi varsa)
* Saldırganların taktikleri, araçları ve yordamları
* URL’ler ve dosya karmaları gibi ilgili tehlike göstergeleri (IoC)
* Azure kaynaklarınızın risk altında olup olmadığını belirlemenize yardımcı olmak için sektörel ve coğrafi yaygınlık bilgisi olan mağdur bilimi
* Azaltma ve düzeltme bilgileri

> [!NOTE]
> Raporlardaki bilgi düzeyi değişiklik gösterir. Ayrıntı düzeyi kötü amaçlı yazılımın etkinliğine ve yaygınlığına bağlıdır.
>
>

Güvenlik Merkezi’nde, saldırıya göre değişiklik gösteren üç tehdit raporu türü vardır. Raporlar şunlardır:

* **Etkinlik Grubu Raporu**: Saldırganlar, amaçları ve taktikleri hakkında ayrıntılı bilgi sunar.
* **Kampanya Raporu**: Belirli saldırı kampanyaların ayrıntılarına odaklanır.
* **Tehdit Özeti Raporu**: Yukarıdaki iki raporun tüm maddelerini kapsar.

Bu tür bilgiler saldırının kaynağını, saldırganın niyetini ve ilerleyen dönemlerde bu riski azaltmak için yapılması gerekenlerin incelendiği bir soruşturmanın yapıldığı [olay yanıt](security-center-incident-response.md) sürecinde çok faydalı olacaktır.

## <a name="how-to-access-the-threat-intelligence-report"></a>Tehdit zekası raporuna nasıl erişebilirim?
**Güvenlik uyarıları** kutucuğuna bakarak mevcut uyarılarınızı gözden geçirebilirsiniz. Azure Portal’ı açın ve her bir uyarı hakkında daha fazla ayrıntıya ulaşmak için aşağıdaki adımları izleyin:

1. Güvenlik Merkezi panosunda **Güvenlik uyarıları** kutucuğunu görürsünüz.
2. Kutucuğa tıklayarak uyarılar hakkında daha fazla bilginin yer aldığı **Güvenlik uyarıları** dikey penceresini açın ve daha fazla bilgi almak istediğiniz güvenlik uyarısını seçin.

    ![Güvenlik uyarıları](./media/security-center-threat-report/security-center-threat-report-fig1.png)
3. Bu durumda **Yürütülen şüpheli işlem** dikey penceresinde yer alan uyarı bilgileri aşağıdaki şekilde gösterilmiştir:

    ![Güvenlik uyarısı ayrıntıları](./media/security-center-threat-report/security-center-threat-report-fig2.png)
4. Güvenlik uyarılarındaki bilgi miktarı, uyarının türüne göre değişiklik gösterir. **RAPORLAR** alanında tehdit zekası raporunun bağlantısını bulabilirsiniz. Bağlantıya tıkladığınızda PDF dosyası yeni bir tarayıcı penceresinde açılacaktır.

   ![Storage seçimi](./media/security-center-threat-report/security-center-threat-report-fig3.png)

Buradan raporun PDF dosyasını indirebilir, algılanan güvenlik sorunu hakkında daha fazla bilgi edinebilir ve verilen bilgilere göre işlem yapabilirsiniz.

## <a name="see-also"></a>Ayrıca bkz.
Bu belgede Azure Güvenlik Merkezi Tehdit Zekası Raporlarının güvenlik uyarısı araştırmaları sırasında nasıl yardımcı olabileceğini öğrendiniz. Azure Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi SSS](security-center-faq.md). Hizmet kullanımı ile ilgili sık sorulan soruları bulun.
* [Olay Yanıtı için Azure Güvenlik Merkezi’nden Yararlanma](security-center-incident-response.md)
* [Azure Güvenlik Merkezi algılama özellikleri](security-center-detection-capabilities.md)
* [Azure Güvenlik Merkezi planlama ve işlemler kılavuzu](security-center-planning-and-operations-guide.md). Azure Güvenlik Merkezi'ni benimsemek için tasarım ile ilgili dikkat edilmesi gerekenleri planlama ve anlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md). Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde Güvenlik Olayını İşleme](security-center-incident.md)
* [Azure Güvenlik Blogu](https://blogs.msdn.com/b/azuresecurity/). Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun.
