---
title: "Azure maliyeti yönetim maliyeti yönetim raporları anlama | Microsoft Docs"
description: "Bu makalede, Cloudyn maliyet yönetim raporları temel yapısını ve işlevlerini anlamanıza yardımcı olur."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 03/07/2018
ms.topic: article
ms.service: cost-management
manager: carmonm
ms.custom: 
ms.openlocfilehash: bc2c696dceb3ed4741c10a5c611bd2d438b71bd5
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="understanding-cost-management-reports"></a>Anlama maliyet yönetim raporları

Bu makalede, Cloudyn maliyet yönetim raporları temel yapısını ve işlevlerini anlamanıza yardımcı olur. Çoğu Cloudyn raporlar sezgisel ve Tekdüzen bir görünümüne sahip. Bu makaleyi okuduktan sonra tüm maliyet yönetim raporları kullanabilmek hazır olursunuz. Birçok standart raporları kolayca gidin olanak tanıyan çeşitli raporlar genelinde kullanılabilir özelliklerdir. Raporları özelleştirilebilir ve hesaplamak ve sonuçları görüntülemek için çeşitli seçenekler arasından seçim yapabilirsiniz.

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

    Raporlarda kullanılabilir yerleşik gruplar listesi aşağıdadır:

    - **Maliyet türü**
      - Maliyet türü veya birden çok maliyet türü seçin veya tümünü seçin. Maliyet türleri şunlardır:
        - Tek seferlik bir ücret
        - Destek
        - Kullanım Maliyeti
    - **Müşteri**
        - Belirli bir müşteri, birden çok müşteriyi veya tüm müşteriler seçin.
    - **Hesap adı**
        - Hesabın veya aboneliğin adı. Azure'da, Azure aboneliğinin adıdır.
    - **Hesap yok**
        - Bir hesap, birden fazla hesap ya da tüm hesapları seçin. Azure'da, Azure aboneliğinin GUID değeridir.
    - **Üst hesabı**
        - Üst hesabı, birden fazla hesap veya Seç seçin.
    - **Hizmet**
        - Birden fazla hizmet, bir hizmet seçin veya tüm hizmetleri seçin.
    - **Sağlayıcı**
        - Varlıklar ve giderler ilişkili olduğu bulut sağlayıcısı.
    - **Bölge**
        - Kaynak barındırıldığı bölgesi.
    - **Kullanılabilirlik bölge**
        - Bir bölge içinde yalıtılmış AWS konumlar.
    - **Kaynak Türü**
        - Kullanımdaki kaynak türü.
    - **Alt türü**
        - Alt türü seçin.
    - **İşlem**
        - İşlemi seçin veya **Tümünü Göster**.
    - **Fiyat modeli**
        - Tüm önceden
        - Önceden yok
        - Kısmi önceden
        - İsteğe Bağlı
        - Rezervasyon
        - Spot
    - **Ücret türü**
        - Negatif veya pozitif Ücret türü veya her ikisini de seçin.
    - **Kiralama**
        - Bir makine adanmış bir makine olarak kullanılıp kullanılmadığını.
    -   **Kullanım türü**
          - Kullanım türü tek seferlik ücretlerinin ya da yinelenen ücretleri olabilir.

5. **filtreleri**

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

12.   **görüntü biçimleri**

    Görüntü biçimleri çeşitli grafik veya tablo görünümleri seçmek için kullanın.

    ![görüntü biçimleri](./media/understanding-cost-reports/display-formats.png)

13. **Çok rengi**

    Raporunuzda grafikleri rengini ayarlamak için çok renk kullanın.

14. **Eylemler**

    Kaydet, dışarı veya raporu zamanlama için eylemlerini kullanın.

## <a name="save-and-schedule-reports"></a>Kaydet ve raporları zamanlama

Bir raporu oluşturduktan sonra gelecekte kullanılmak üzere kaydedebilirsiniz. Kaydedilen raporlar kullanılabilir **My Araçları** > **raporlarım**. Var olan bir raporu değişiklikleri yapın ve dosyayı kaydedin, raporun yeni bir sürüm olarak kaydedilir. Veya yeni bir rapor olarak kaydedin.

### <a name="save-a-report-to-the-cloudyn-portal"></a>Rapor Cloudyn Portalı'na kaydetme

Herhangi bir raporu görüntülerken tıklatın **Eylemler** ve ardından **raporlarım için Kaydet**. Rapor adı ve ya da eklemek bir kendi URL veya otomatik olarak oluşturulan URL'yi kullan. İsteğe bağlı olarak yapabileceğiniz **paylaşmak** herkese açık şekilde başkalarıyla kuruluşunuzun veya rapor için varlığınız paylaşabilirsiniz. Raporun paylaşmıyorsa kişisel bir rapor kalır ve yalnızca görüntüleyebilirsiniz. Raporu kaydedin.


### <a name="save-a-report-to-cloud-provider-storage"></a>Sağlayıcı depolama bulut için bir rapor Kaydet

Bulut hizmeti sağlayıcısı için bir raporu kaydetmek için zaten bir depolama hesabı yapılandırmış olmanız gerekir. Herhangi bir raporu görüntülerken tıklatın **Eylemler** ve ardından **zamanlama rapor**. Rapor adı ve ya da eklemek bir kendi URL veya otomatik olarak oluşturulan URL'yi kullan. Seçin **depolamasına kaydetmek** ve ardından depolama hesabını seçin veya yeni bir tane ekleyin. Raporu dosya adına eklenmiş bir önek girin. Bir CSV veya JSON dosyası biçiminde seçin ve ardından raporu kaydedin.

### <a name="schedule-a-report"></a>Bir raporu zamanlama

Zamanlanan aralıklarla raporları çalıştırabilirsiniz ve gönderdiğiniz bunları alıcı listesi veya Bulut için bir hizmet sağlayıcısı depolama hesabı. Herhangi bir raporu görüntülerken tıklatın **Eylemler** ve ardından **zamanlama rapor**. Raporu e-posta ile göndermek ve bir depolama hesabına kaydedin. Altında **zamanlama**, aralığı (günlük, haftalık veya aylık) seçin. Haftalık ve aylık için gün veya teslim etmek ve zamanı seçmek için tarihleri seçin. Zamanlanmış raporu kaydedin. Excel raporu biçimini seçerseniz rapor ek olarak gönderilir. E-posta içeriği biçimini seçtiğinizde, grafik biçiminde gösterilen rapor sonuçlarını bir grafik gönderilir.

### <a name="export-a-report-as-a-csv-file"></a>Bir raporu CSV dosyası olarak dışarı aktarın

Herhangi bir raporu görüntülerken tıklatın **Eylemler** ve ardından **tüm rapor verilerini dışa**. Açılır pencere görünür ve bir CSV dosyası indirilir.

## <a name="next-steps"></a>Sonraki adımlar

- İlk öğreticide maliyet yönetimi için zaten tamamlanmış yapmadıysanız, hem okuma [gözden kullanım ve maliyetleri](tutorial-review-usage.md).
