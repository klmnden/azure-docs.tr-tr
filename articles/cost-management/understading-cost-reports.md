---
title: "Azure maliyeti yönetim maliyeti raporlarda anlama | Microsoft Docs"
description: "Bu makalede, Cloudyn raporları temel yapısını ve işlevlerini anlamanıza yardımcı olur."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 01/30/2018
ms.topic: article
ms.service: cost-management
manager: carmonm
ms.custom: 
ms.openlocfilehash: 38c1313f42a58403e158cad9c2930b6541da5adc
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="understanding-cost-reports"></a>Maliyet raporlarını anlama

Bu makalede, Cloudyn raporları temel yapısını ve işlevlerini anlamanıza yardımcı olur. Çoğu Cloudyn raporlar sezgisel ve Tekdüzen bir görünümüne sahip. Bu makaleyi okuduktan sonra tüm raporları kullanabilmek hazır olursunuz. Birçok standart raporları kolayca gidin olanak tanıyan çeşitli raporlar genelinde kullanılabilir özelliklerdir. Raporları özelleştirilebilir ve hesaplamak ve sonuçları görüntülemek için çeşitli seçenekler arasından seçim yapabilirsiniz.

## <a name="report-fields-and-options"></a>Rapor alanları ve seçenekleri

Burada, zaman içinde maliyeti raporu örneği göz verilmiştir. Çoğu Cloudyn raporları benzer bir düzeni sahiptir.

![Örnek rapor](./media/understanding-cost-reports/sample-report.png)

Önceki görüntünün numaralı her alanına aşağıdaki bilgileri ayrıntılı açıklanmıştır:

1. **Tarih aralığı**

    Hazır veya özel kullanarak bir rapor zaman aralığını tanımlamak için tarih aralığını listesini kullanın.
2. **Kaydedilen filtresi**

    Kaydedilen filtresi listesi rapora uygulanan filtreler ve geçerli gruplar kaydetmek için kullanın. Kaydedilmiş filtreler gibi maliyet ve performans raporları kullanılabilir:

      - Maliyet çözümleme
      - Ayırma
      - Varlık Yönetimi
      - İyileştirme

  Bir filtre adı ve ardından yazın **kaydetmek**.

3. **Etiketler**

    Grup etiketleri alana etiketi kategorilere göre kullanın. Menüde listelenen etiketleri Azure departmanı ya da maliyet merkezi etiketler veya Cloudyn'ın maliyet varlık ve üyelik etiketler. Sonuçları filtrelemek için etiketler seçin. Sonuçları filtrelemek için bir etiket adı (anahtar) de yazabilirsiniz.

    ![seçenekleri seçin](./media/understanding-cost-reports/select-options.png)

    Tıklatın **Ekle** yeni bir filtre eklemek için.

    ![Filtre ekleme](./media/understanding-cost-reports/add-filter.png)

    Gruplandırma veya filtreleme etiketi Azure kaynakları veya kaynak grubu etiketleri ilgili değildir.

    Maliyet ayırma etiketi gruplandırma ve filtreleme bulunan **grupları** menü seçeneği.

4. **Raporlarda grupları**

    Grupları kullanma maliyet çözümleme, standart, göstermek için raporları dökümü Raporunuzdaki verileri faturalama gelen kategoriler.  Ancak, maliyet ayırma raporları göster gruplarında etiket tabanlı kategorileri görüntüleyin. Etiket tabanlı kategorileri maliyet ayırma modeli ve faturalama verisi standart dökümü kategorilerden tanımlanır.

    ![grupları etiketleri](./media/understanding-cost-reports/groups-tags01.png)

    ![grupları etiketleri](./media/understanding-cost-reports/groups-tags02.png)

    Maliyet ayırma raporlarda etiketi tabanlı Grup kategorileri gruplarında şunlar olabilir:
      - Etiketler
      - kaynak grubu etiketleri
      - Varlık etiketleri maliyet Cloudyn
      - Maliyet ayırmayı amaçlar için abonelik etiketi kategorileri

  Örnekleri içerebilir:
     - Maliyet merkezi
     - Bölüm
     - Uygulama
     - Ortam
     - Maliyet kodu

5. **Filtreleri**

    Aralıkları seçili değerlere ayarlamak için tek veya çoklu seçim filtreleri kullanın. Bir filtre ayarlamak için tıklatın **Ekle** ve ardından filtre kategorisi ve değerleri seçin.

6. **Maliyet modeli**

    Maliyet modeli maliyet ayırma 360 ile önceden oluşturduğunuz bir maliyet modeli seçmek için kullanın. Maliyet ayırma gereksinimlerinize bağlı olarak birden fazla Cloudyn maliyet modeli olabilir. Bazı kuruluş ekipleriniz diğerlerinden farklı ayırma gereksinimleri maliyet yoluna sahip. Her takım kendi adanmış maliyet modeline sahip olabilir.

    Maliyet ayırma modeli tanımı oluşturma hakkında daha fazla bilgi için bkz: [maliyetleri ayırmak üzere özel etiketleri kullanın](tutorial-manage-costs.md#use-custom-tags-to-allocate-costs).

7. **İtfası**

    Kullanım dışı görüntülemek için kullanım İtfası maliyet ayırma raporlarında hizmet ücretlerinin ya da tek seferlik borç maliyetleri dayalı ve kendi maliyet süre kendi kullanım ömrü boyunca eşit olarak üzerinden yayılabilir. Tek seferlik ücretleri örnekleri içerebilir:
    - Yıllık destek ücretleri
    - Yıllık güvenlik bileşenleri ücretleri
    - Ayrılmış örnekler ücretleri satın alma
    - Bazı Azure Market öğesi.

  İtfası altında seçin **maliyet Amortized** veya **gerçek maliyet**.

8. **Çözümleme**

    Seçilen tarih aralığı içinde saat çözümleme seçmek üzere çözümü kullanın. Zaman çözünürlüğünüz nasıl birimleri raporda görüntülenen ve olabilir belirler:
    - Günlük
    - Haftalık
    - Aylık
    - Üç aylık
    - Yıllık

9. **Ayırma kuralları**

    Uygulama veya maliyet ayırma maliyet yeniden hesaplama devre dışı bırakmak için ayırma kurallarını kullanın. Etkinleştirmek veya faturalama verisi için maliyet ayırma yeniden hesaplama devre dışı bırakabilirsiniz. Rapor seçili kategorilerde yeniden hesaplama uygular. Ham faturalama verisi karşı maliyet ayırma yeniden hesaplama etkisini değerlendirmenize olanak tanır.

10. **Kategorilere ayrılmamış**

    Kategorilere ayrılmamış dahil etmek veya hariç rapordaki Kategorilere ayrılmamış maliyetleri kullanın.

11. **Alanları Göster/Gizle**

    Göster/Gizle seçeneği herhangi bir etkisi raporlarda sahip değil.

12.   **Görüntü biçimleri**

    Görüntü biçimleri çeşitli grafik veya tablo görünümleri seçmek için kullanın.

    ![görüntü biçimleri](./media/understanding-cost-reports/display-formats.png)

13. **Çok rengi**

    Raporunuzda grafikleri rengini ayarlamak için çok renk kullanın.

14. **Eylemler**

    Kaydet, dışarı veya raporu zamanlama için eylemlerini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

- İlk öğreticide maliyet yönetimi için zaten tamamlanmış yapmadıysanız, hem okuma [gözden kullanım ve maliyetleri](tutorial-review-usage.md).
