---
title: Azure Güvenlik Merkezi'nde güvenilirlik puanı | Microsoft Docs
description: " Azure Güvenlik Merkezi güvenilirlik puanı ile çalışmayı öğrenin. "
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: e88198f8-2e16-409d-a0b0-a62e68c2f999
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/23/2018
ms.author: rkarlin
ms.openlocfilehash: 64bab5c1b99720eecb189834d7a11802cf919ca3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60916601"
---
# <a name="alert-confidence-score"></a>Güvenilirlik puanı uyarısı 

Azure Güvenlik Merkezi, size Azure'da çalıştırdığınız kaynaklar genelinde görünürlük sağlar ve olası sorunları algıladığında sizi uyarır. Hacimli uyarılar bir güvenlik operasyon ekibinin adreslemek zor olabilir ve araştırmak için hangi uyarıları önceliklendirme gerekli hale gelir. Uyarıları araştırırken karmaşık ve zaman alıcı olabilir ve sonuç olarak, bazı uyarıları göz ardı edilir.

Güvenlik Merkezi'nde güvenilirlik puanı, takım önceliklendirme yardımcı olmak ve Uyarıları önceliklendirme kullanabilirsiniz. Güvenlik Merkezi, sektördeki en iyi uygulamalar, akıllı algoritmalar ve bir tehdit yasal ve anlamlı bilgiler bir güven puanı biçiminde sağlar belirlemek için analist tarafından kullanılan işlemleri otomatik olarak uygular.

## <a name="how-the-confidence-score-is-triggered"></a>Güvenilirlik puanı nasıl tetiklenir

Şüpheli işlemlerin sanal makinelerinizde çalışan algılandığında uyarı oluşturulur. Güvenlik Merkezi inceler ve bu uyarılar Azure'da çalışan bir Windows sanal makinelerinde analiz eder. Otomatik denetimler ve gelişmiş algoritmalar kuruluş ve tüm Azure kaynaklarınız arasında birden çok varlıkları ve veri kaynakları arasında kullanarak bağıntılar gerçekleştirir ve Güvenlik Merkezi'nin nasıl başarılara bir ölçü olan puanı güvenle sunar Uyarı gerçek olduğunu ve araştırılması gerekir.

## <a name="understanding-the-confidence-score"></a>Güvenilirlik puanı anlama

Güvenilirlik puanı, 1 ile 100 arasında aralıkları ve Güvenlik Merkezi uyarısı araştırılması gerektiğini sahip güvenle temsil eder. Yüksek puanı, daha rahat hareket edebiliyor Güvenlik Merkezi uyarısı orijinal kötü amaçlı etkinliği gösterir. Güvenilirlik puanı, neden kendi güvenilirlik puanı uyarı alınan başlıca nedenlerdendir listesini içerir. Güvenilirlik puanı yanıtını uyarılara öncelik sırasına sokmanıza ve saldırıları ilk tuşuna basarak, sonuçta saldırıları ve ihlal yanıtlamak için gereken süre miktarını azaltarak en adres güvenlik analistleri için kolaylaştırır.

Güvenilirlik puanı görüntülemek için:
- Güvenlik Merkezi portalında güvenlik uyarıları dikey penceresi açın.
-  Uyarıları ve olaylarının en yüksekten en düşük, daha rahat hareket edebiliyor Güvenlik Merkezi bir uyarı için sayfanın üstündeki olduğu bir tehdit olan, daha yakın temsil ettiğini yani düzenlenir. 


 ![Güvenilirlik puanı][1]

Bir uyarı Güvenlik Merkezi'nin ellerde katkıda verileri görüntülemek için:
- Güvenlik altında dikey penceresinde, uyarı **güvenle**görüntülemek için güvenilirlik puanı katkıda gözlemleri ve uyarıyla ilgili Öngörüler edinin. Bu uyarıya neden etkinlikleri yapısını daha fazla öngörü sağlar.

  ![Şüpheli güvenilirlik puanı][2]

Uyarı önceliklendirme ortamınızdaki önceliğini belirlemek için kullanım Güvenlik Merkezi'nin güvenilirlik puanı. Güvenilirlik puanı otomatik olarak uyarıları araştırırken, sektördeki en iyi yöntemler ve akıllı algoritmaları uygulayarak ve hangi tehditleri gerçek ve ilgilenmeniz odaklanmanıza gerek duyduğunuz senaryolara belirlemek için cevap sanal analistini davranan zaman ve çaba kaydeder.


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, güvenilirlik puanı uyarı araştırma önceliğini belirlemek için nasıl kullanılacağı açıklanmıştır. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.



<!--Image references-->
[1]: ./media/security-center-confidence-score/confidence-score.png
[2]: ./media/security-center-confidence-score/suspicious-confidence-score.png
