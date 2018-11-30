---
title: Azure İzleyici'de dinamik eşikler ile uyarılar oluşturma
description: Makine öğrenimi tabanlı dinamik eşikler ile uyarılar oluşturun
author: yanivlavi
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 11/29/2018
ms.author: Yaniv.Lavi
ms.reviewer: mbullwin
ms.component: alerts
ms.openlocfilehash: af9f85014ea16dd266c56a71f13b4dce2adccc9a
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2018
ms.locfileid: "52619717"
---
# <a name="alerts-with-dynamic-thresholds-in-azure-monitor-limited-private-preview"></a>Azure İzleyici (sınırlı özel Önizleme) içinde dinamik eşikler ile uyarılar

Dinamik eşikler ile uyarılar Azure ölçüm uyarı Azure İzleyici'de taban çizgileri otomatik olarak hesaplar ve bunları uyarı eşiklerini geçmiş davranışını ölçümleri Gelişmiş Machine Learning (ML) özelliklerinden yararlanmak için bir geliştirme ' dir.

Dinamik eşikler kullanmanın avantajları şunlardır:

- İzleyici otomatik olarak ölçüm geçmiş performansını öğrenir ve ML algoritmaları uyarı eşikleri belirlemek için geçerli olarak önceden tanımlanmış bir katı sınırı ayarlama ile ilişkili bir kolayca kaydedin.
- Bunlar, Dönemsel davranışı ve sezona yönelik beklenen davranışın sapmalar yalnızca uyarısında tanımlayabilirsiniz. Hizmetinizi sonralı düzenli olarak kullanılmadığında ve ardından her Pazartesi ani ölçüm uyarıları dinamik eşikler ile tetiklemez. Şu anda desteklenmiyor: saatlik, günlük ve haftalık mevsimsellik.
- Sürekli olarak Ölçüm performans öğrenir ve ölçüm değişiklikleri uyarlanabilir.

Dinamik Eşik tabanlı uyarılar, tüm Azure İzleyici tabanlı ölçüm kaynakları bu konuda listelenen için kullanılabilir [makale](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts#what-resources-can-i-create-near-real-time-metric-alerts-for).

## <a name="sign-up-to-access-the-preview"></a>Önizleme için kaydolun

Bu özellik için bir dönüş almak için [Önizleme için kaydolun](https://aka.ms/DynamicThresholdMetricAlerts). Her zaman, bize Geri bildirimlerinizi öğrenmekten mutluluk duyarız gibi bu, yakında tutun [azurealertsfeedback@microsoft.com](mailto:azurealertsfeedback@microsoft.com)

## <a name="how-to-configure-alerts-with-dynamic-thresholds"></a>Dinamik eşikler ile uyarıları yapılandırma

Dinamik eşikler ile uyarılar, uyarılar Azure İzleyici aracılığıyla yapılandırılabilir

![Uyarılar Önizleme](./media/monitoring-alerts-dynamic-thresholds/0001.png)

## <a name="creating-an-alert-rule-with-dynamic-thresholds"></a>Dinamik eşikler ile bir uyarı kuralı oluşturma

1. Uyarıları izleme altında bölmeden **yeni uyarı kuralı** Azure'da yeni bir uyarı oluşturmak için.

   ![Yeni Uyarı Kuralı](./media/monitoring-alerts-dynamic-thresholds/002.png)

2. Üç bölümden oluşan ile oluşturma kuralı bölümü gösterilir: _uyarı koşulunu tanımlama_, _uyarı ayrıntılarını tanımlama_, ve _tanımla eylem grubu_. İlk şununla _uyarı koşulunu tanımlama_ bölümdeki **seçin hedef** bir kaynak seçerek hedef belirtmek için bağlantı. Uygun bir kaynak seçilir sonra bitti düğmesine tıklayın.

   ![Hedef seçme](./media/monitoring-alerts-dynamic-thresholds/0003.png)

3. Ardından **Ölçüt Ekle** sinyal seçenekleri için kaynak ve sinyal listesinden kullanılabilir bir listesini görüntülemek için düğmeyi seçin, uygun bir **ölçüm** seçeneği. (Örneğin CPU yüzdesi)

   ![Ölçüt ekle](./media/monitoring-alerts-dynamic-thresholds/004.png)

4. Uyarı mantığı bölümünde yapılandırma sinyal mantığını ekranda ölçüm (mavi çizgi) yanı sıra dinamik eşikler (kırmızı çizgiler) otomatik olarak oluşturur dinamik türüne koşul geçiş seçeneğine sahip.

   ![Dinamik](./media/monitoring-alerts-dynamic-thresholds/005.png)

5. Grafikte görüntülenen eşikleri alınarak hesaplanır son yedi gün geçmiş veri çubuğunda bir uyarı oluşturulduktan sonra dinamik eşikler kullanılabilir ek geçmiş verileri alacağı ve sürekli hale getirmek için yeni verilere dayalı olarak bilgi edineceksiniz eşikleri daha doğru.

6. Ek uyarı mantığı ayarları:
   - Koşul - aşağıdaki üç koşulun birinde tetiklenmesi için uyarıyı seçebilirsiniz:
       - Üst eşik değerinden büyük veya eşiğin daha düşük (varsayılan)
       - Üst eşik değerinden büyük
       - Alt eşik değerinden düşük.
   - Zaman toplama: ortalama (varsayılan), Topla, min, maks.
   - Uyarı duyarlılığı:
       - Yüksek – küçük sapmaya uyarı tetiklenecek şekilde daha fazla uyarı.
       - MED-daha az duyarlı daha yüksek değerinden daha az uyarı ile Yüksek duyarlılık (varsayılan)
       - Düşük – en az hassas eşiği.

    ![Uyarı mantığı ayarları](./media/monitoring-alerts-dynamic-thresholds/00007.png)

7. Değerlendirilen göre:
    -  Hangi süre uyarı için belirtilen koşul seçerek görünmelidir **süresi**.

    ![Değerlendirme şunu temel alır:](./media/monitoring-alerts-dynamic-thresholds/007.png)

   > [!NOTE]
   > Desteklenen dönem değerleri: 5 dakika, 30 dakika ile 1 saat 10 dakika.

   Geçici artışlara tarafından oluşturulan uyarı gürültüsünü azaltmak için "uyarı tetiklenmesi için ihlal sayısı" ayarları kullanmanızı öneririz. Yalnızca eşiği ihlal durumunda uyarı almak bu işlevi sağlayan X art arda kaç kez veya Y saatleri dışında son Z dönem. Örneğin:

    15 dakika boyunca sürekli sorun olduğunda uyarı tetiklemek için 5 dakika, belirli bir süre içinde art arda 3 kez aşağıdaki ayarları kullanın:

   ![Değerlendirme şunu temel alır:](./media/monitoring-alerts-dynamic-thresholds/0008.png)

    Gelen bir Dinamik Eşik ihlalinin dışında son 30 dakika süre 5 dakika ile 15 dakika içinde olduğunda bir uyarı tetiklemek için aşağıdaki ayarları kullanın:

   ![Değerlendirme şunu temel alır:](./media/monitoring-alerts-dynamic-thresholds/0009.png)

8. Şu anda kullanıcılar tek bir ölçüt olarak Dinamik Eşik ölçütleri uyarılarla olabilir.

   ![Kural oluşturma](./media/monitoring-alerts-dynamic-thresholds/010.png)

## <a name="q--a"></a>Soru-Cevap

- S: ölçüm yavaş zamanla değişirse, bu dinamik eşikler ile bir uyarı tetikleyecek?

- C: büyük olasılıkla Hayır. Dinamik eşikler önemli sapmaları algılama yerine yavaş sorunları gelişen iyidir.

- Ben bir API aracılığıyla dinamik eşikler yapılandırabilir miyim?

- Y: üzerinde çalışıyoruz.
