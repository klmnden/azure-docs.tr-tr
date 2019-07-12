---
title: Verileri olay hub'ları Azure Stream Analytics'i kullanarak işleyebilir | Microsoft Docs
description: Bu makalede Azure Stream Analytics işi Azure olay hub'ınıza gelen verilerin nasıl işleneceğini gösterir.
services: event-hubs
author: spelluru
manager: ''
ms.author: spelluru
ms.date: 07/09/2019
ms.topic: article
ms.service: event-hubs
ms.openlocfilehash: f179687b0983e145244e228a3d3b06b4eabead48
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67723421"
---
# <a name="process-data-from-your-event-hub-using-azure-stream-analytics"></a>Azure Stream Analytics'i kullanarak, olay hub'ından verileri işleme
Azure Stream Analytics hizmeti, işlem, alma ve Azure Event hubs'tan akış verilerini analiz etmek gerçek zamanlı eylemlerden güçlü içgörüler etkinleştirme kolaylaştırır. Bu tümleştirme hot yolu analytics işlem hattı hızlıca oluşturmanıza olanak sağlar. Azure portalı, gelen verileri görselleştirmek ve bir Stream Analytics sorgusunu yazmak için kullanabilirsiniz. Sorgunuzu hazır hale geldikten sonra yalnızca birkaç tıklamayla üretime taşıyabilirsiniz. 

## <a name="key-benefits"></a>Önemli avantajlar
Azure Event Hubs ve Azure Stream Analytics tümleştirmesi öne çıkan avantajları şunlardır: 
- **Verileri Önizle** – Azure portalında bir olay hub'ından gelen verilerin önizlemesini görebilirsiniz.
- **Sorgunuzu test** : bir dönüşüm sorgusu hazırlamak ve doğrudan Azure Portalı'nda test. Sorgu dili söz dizimi için bkz. [Stream Analytics sorgu dili](/stream-analytics-query/built-in-functions-azure-stream-analytics) belgeleri.
- **Sorgunuzu üretime dağıtma** – sorgu oluşturma ve Azure Stream Analytics işi başlatılıyor üretime dağıtabilirsiniz.

## <a name="end-to-end-flow"></a>Uçtan uca akış

1. [Azure Portal](https://portal.azure.com) oturum açın. 
1. Gidin, **Event Hubs ad alanı** gidin **olay hub'ı**, gelen veri vardır. 
1. Seçin **işlem verileri** olay hub'ı sayfasında.  

    ![İşlem veri kutucuğu](./media/process-data-azure-stream-analytics/process-data-tile.png)
1. Seçin **Araştır** üzerinde **etkinleştirme olaylarını gerçek zamanlı içgörüler** Döşe. 

    ![Stream Analytics seçin](./media/process-data-azure-stream-analytics/process-data-page-explore-stream-analytics.png)
1. Aşağıdaki alanlar için önceden ayarlanmış değerlerle sorgu sayfayı görürsünüz:
    1. **Olay hub'ı** sorgu için bir giriş olarak.
    1. Örnek **SQL sorgusu** SELECT deyimi ile. 
    1. Bir **çıkış** diğer test sonuçlarınıza bakın. 

        ![Sorgu Düzenleyicisi](./media/process-data-azure-stream-analytics/query-editor.png)
        
        > [!NOTE]
        >  Bu sayfa, bu özelliği ilk kez kullandığınızda, gelen verilerin önizlemesini görmek için bir tüketici grubu ve olay hub'ınız için bir ilke oluşturmak izninizi isteyen.
1. Seçin **Oluştur** içinde **giriş Önizleme** önceki resimde gösterildiği gibi bölmesi. 
1. Bu sekme en son gelen verilerin bir anlık görüntü hemen görürsünüz.
    - Serileştirme türü verilerinizde otomatik olarak olur (JSON/CSV) algılandı. CSV/JSON/AVRO için de el ile değiştirebilirsiniz.
    - Gelen veriler tablo biçiminde veya ham biçimde önizleyebilirsiniz. 
    - Gösterilen verilerinizi geçerli değilse, seçin **Yenile** en son olayları görmek için. 

        İşte bir örnek veri **tablo biçimi**:   ![Tablo biçiminde sonuçları](./media/process-data-azure-stream-analytics/snapshot-results.png)

        İşte bir örnek veri **ham biçimde**: 

        ![Sonuçları ham biçimde](./media/process-data-azure-stream-analytics/snapshot-results-raw-format.png)
1. Seçin **Test sorgusu** sorgu sonuçları anlık görüntüsünü görmek için **Test sonuçları** sekmesi. Sonuçları da indirebilirsiniz.

    ![Sorgu sonuçları test](./media/process-data-azure-stream-analytics/test-results.png)
1. Verileri dönüştürmek için kendi sorgunuzu yazın. Bkz: [Stream Analytics sorgu dili başvurusu](/stream-analytics-query/stream-analytics-query-language-reference).
1. Sorguyu test ettikten sonra üretime, seçme taşımak istediğiniz **Dağıt sorgu**. Sorgu dağıtmak için burada, bir çıkış işiniz için ayarlayabilir ve işin başlaması Azure Stream Analytics işi oluşturun. Bir Stream Analytics işi oluşturmak için iş için bir ad belirtin ve seçin **Oluştur**.

      ![Azure Stream Analytics işi oluşturma](./media/process-data-azure-stream-analytics/create-stream-analytics-job.png)

      > [!NOTE] 
      >  Bir tüketici grubu ve Event Hubs sayfasından oluşturduğunuz her yeni bir Azure Stream Analytics işi için bir ilke oluşturmanızı öneririz. Her iş için ayrılmış bir tüketici grubu sağlayarak bu sınırını aşmasını ortaya çıkan hatalar sorundan kaçınmak için tüketici grupları yalnızca beş eş zamanlı okuyucu sağlar. Bir ayrılmış ilke anahtarınızı döndürme veya diğer kaynaklara etkilemeden izinlerini iptal etme sağlar. 
1. Stream Analytics işinizi, şimdi burada sorgunuzu test aynıdır ve olay hub'ınıza giriştir oluşturulur. 

9.  İşlem hattı tamamlanacak şekilde ayarlanacağını **çıkış** seçin ve sorgu **Başlat** işi başlatmak için.

    > [!NOTE]
    > İşi başlatmadan önce outputalias Azure Stream Analytics'te oluşturulan çıktı adıyla değiştirmeyi unutmayın.

      ![Çıkış ayarlayın ve işi başlatın](./media/process-data-azure-stream-analytics/set-output-start-job.png)


## <a name="known-limitations"></a>Bilinen sınırlamalar
Sorgunuzu test ederken, test sonuçlarını yüklemek için yaklaşık 6 dakika sürer. Sınama performansı geliştirme konusunda durmaksızın çalışıyoruz. Ancak, üretim ortamında dağıtıldığında, Azure Stream Analytics subsecond gecikme süresine sahiptir.

## <a name="streaming-units"></a>Akış birimleri
Varsayılan olarak üç akış birimleri (su) Azure Stream Analytics işi. Bu ayar ayarlamak için **ölçek** sol menüde **Stream Analytics işi** Azure portalında sayfası. Akış birimleri hakkında daha fazla bilgi için bkz: [anlayın ve akış birimi Ayarla](../stream-analytics/stream-analytics-streaming-unit-consumption.md).

![Akış birimleri ile ölçeklendirme](./media/process-data-azure-stream-analytics/scale.png)

## <a name="next-steps"></a>Sonraki adımlar
Stream Analytics sorguları hakkında daha fazla bilgi için bkz: [Stream Analytics sorgu dili](/stream-analytics-query/built-in-functions-azure-stream-analytics)