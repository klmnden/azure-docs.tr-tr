---
title: Azure İzleyicisi'nde dinamik eşiklerle uyarıları oluşturma | Microsoft Docs
description: Uyarı dayalı machine learning dinamik eşikleri ile oluşturma
author: antonfrMSFT
manager: carmonm
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ''
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2018
ms.author: antonfr;mbullwin
ms.openlocfilehash: c847052134b1d83cd606e0e2b51b63b580f7917c
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="alerts-with-dynamic-thresholds-in-azure-monitor-limited-public-preview"></a>Dinamik eşikleri (sınırlı genel Önizleme) Azure izleyicisinde uyarıları

Azure ölçüm uyarılar Azure İzleyicisi ' nde otomatik olarak temelleri hesaplamak ve bunları uyarı eşikleri ölçümleri geçmiş davranışını öğrenmek için Gelişmiş makine öğrenimi (ML) özelliklerden yararlanacak yüklenene uyarılar dinamik eşiklerle.

Dinamik eşikleri kullanmanın avantajları şunlardır:

- İzleyici otomatik olarak ölçüm geçmiş performansını öğrenir ve uyarı eşikleri belirlemek için ML algoritmaları uygular olarak önceden tanımlanmış bir katı sınır ayarı ile ilişkili mücadele kaydedin.
- Bunlar Mevsimlik davranışı ve beklenen Mevsimlik davranış sapmaları yalnızca uyarıdaki belirleyebilir. Hizmetinizi hafta sonu düzenli olarak boş ise ve her Pazartesi ani dinamik eşikleri ölçüm uyarılarla tetiklemez. Şu anda desteklenmiyor: saatlik, günlük ve haftalık mevsimselliğin.
- Sürekli olarak Ölçüm performans öğrenir ve ölçüm değişiklikler uyarlanabilir.

Bu konuda listelenen ölçüm kaynakları tabanlı tüm Azure İzleyici için Dinamik Eşik tabanlı uyarılar kullanılabilir [makale](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts#what-resources-can-i-create-near-real-time-metric-alerts-for).

## <a name="sign-up-to-access-the-preview"></a>Önizleme erişmek kaydolun

Bu özellik için bir değer değiştirme yapılacak [Önizleme için kaydolmak](http://aka.ms/DynamicThresholdMetricAlerts). Her zaman, biz görüşlerinizi almak isteriz gibi gelen tutun [azurealertsfeedback@microsoft.com](mailto:azurealertsfeedback@microsoft.com)

## <a name="how-to-configure-alerts-with-dynamic-thresholds"></a>Dinamik Eşik değeriyle uyarılarını yapılandırma

Dinamik eşikleri uyarılarla Azure İzleyicisi'nde uyarıları aracılığıyla yapılandırılabilir

![Uyarıları Önizleme](./media/monitoring-alerts-dynamic-thresholds/0001.png)

## <a name="creating-an-alert-rule-with-dynamic-thresholds"></a>Dinamik Eşik değeriyle bir uyarı kuralı oluşturma

1. İzleyici altında uyarıları bölmesinden seçin **yeni uyarı kuralı** Azure'da yeni bir uyarı oluşturma düğmesi.

   ![Yeni Uyarı Kuralı](./media/monitoring-alerts-dynamic-thresholds/002.png)

2. Üç bölümden oluşan ile Oluştur kural bölümünde gösterilen: _Uyarı koşulu tanımla_, _uyarı ayrıntılarını tanımlayın_, ve _tanımla eylem grubu_. İlk ile başlayan _Uyarı koşulu tanımla_ bölüm kullan **Select Target** hedef kaynak seçerek belirtmek için bağlantı. Uygun bir kaynak seçilen sonra Bitti düğmesini tıklatın.

   ![Hedef seçin](./media/monitoring-alerts-dynamic-thresholds/0003.png)

3. Sonraki kullanmak **ölçüt eklemek** sinyal seçenekleri kaynağın ve sinyal listesinden kullanılabilir bir listesini görüntülemek için düğmesini seçin uygun bir **ölçüm** seçeneği. (Örneğin yüzde CPU)

   ![Ölçüt ekle](./media/monitoring-alerts-dynamic-thresholds/004.png)

4. Yapılandırma sinyal mantığı ekranda uyarı mantığı bölümünde ölçüm (mavi satır) yanı sıra dinamik eşikleri (kırmızı satırları) otomatik olarak oluşturur koşul dinamik türüne geçiş seçeneğiniz vardır.

   ![Dinamik](./media/monitoring-alerts-dynamic-thresholds/005.png)

5. Grafikte görünmesini eşikleri dayalı hesaplanır son yedi gün geçmiş verilerin bir uyarı oluşturulduktan sonra dinamik eşikleri kullanılabilir ek geçmiş verileri alacağı ve sürekli olarak yapmaya göre yeni verileri öğreneceksiniz eşikleri daha doğru.

6. Ek uyarı mantık ayarları:
   - Koşul - aşağıdaki üç koşullardan biri tetiklenmesi için uyarıyı seçebilirsiniz:
       - Üst eşik değerinden büyük veya daha düşük eşik (varsayılan) değerinden daha düşük
       - Üst eşik değerinden büyük
       - Alt eşik değerinden daha düşük.
   - Zaman toplama: ortalama (varsayılan), Topla, min maks.
   - Uyarı duyarlılık:
       - Yüksek – daha fazla uyarıları, en küçük sapmaya uyarı tetiklenecek şekilde.
       - MED – daha az duyarlı daha yüksek, daha az uyarıları daha yüksek duyarlılık (varsayılan)
       - Düşük – az hassas eşiği.

    ![Uyarı mantığı ayarları](./media/monitoring-alerts-dynamic-thresholds/00007.png)

7. Hesaplanan göre:
    -  Hangi süre uyarı için belirtilen koşul seçerek görünmelidir **süresi**.

    ![Değerlendirme şunu temel alır:](./media/monitoring-alerts-dynamic-thresholds/007.png)

   > [!NOTE]
   > Desteklenen dönem değerler: 5 dakika, 10 dakika, 30 dakika ile 1 saat.

   Geçici ani tarafından oluşturulan uyarı gürültü azaltmak için "uyarıyı tetikleyecek ihlalleri sayı" ayarlarını kullanmanızı öneririz. Bu işlevsellik yalnızca eşiği ihlal edildiğini değilse bir uyarı almak sağlar X kez art arda ya da son Z nokta dışında Y kez. Örneğin:

    İçin 15 sürekli sorunu dakika, 5 dakika cinsinden belirtilen süre içinde art arda 3 kez bir uyarıyı tetiklemek için aşağıdaki ayarları kullanın:

   ![Değerlendirme şunu temel alır:](./media/monitoring-alerts-dynamic-thresholds/0008.png)

    Gelen dinamik eşik ihlalinin süre 5 dakika ile son 30 dakika dışında 15 dakika içinde değiştirildiği bir uyarıyı tetiklemek için aşağıdaki ayarları kullanın:

   ![Değerlendirme şunu temel alır:](./media/monitoring-alerts-dynamic-thresholds/0009.png)

8. Şu anda kullanıcıların tek bir ölçüt olarak Dinamik Eşik ölçütleri uyarılarla olabilir.

   ![Kural oluşturma](./media/monitoring-alerts-dynamic-thresholds/010.png)

## <a name="q--a"></a>Soru-Cevap

- S: ölçüm yavaş zamanla değişirse, bu dinamik eşiklerle bir uyarı tetikleyecek?

- A: büyük olasılıkla yok. Dinamik eşikleri önemli sapmaları algılama yerine yavaş sorunları gelişen iyidir.

- S: bir API aracılığıyla dinamik eşikleri yapılandırmak?

- Y: üzerinde çalışıyoruz.
