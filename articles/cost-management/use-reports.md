---
title: "Azure maliyeti Management maliyet yönetim raporları kullanma | Microsoft Docs"
description: "Bu makalede, çeşitli maliyet yönetim raporları Cloudyn Portalı'nda kullanmayı açıklar."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 01/30/2018
ms.topic: article
ms.service: cost-management
manager: carmonm
ms.custom: 
ms.openlocfilehash: 8078591b1e2ad120190a23dd29800bd0f1ae33ea
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="use-cost-management-reports"></a>Maliyet Yönetimi raporlarını kullanma

Bu makalede, çeşitli maliyet yönetim raporları Cloudyn Portalı'nda kullanmayı açıklar. Çoğu Cloudyn raporlar sezgisel ve Tekdüzen bir görünümüne sahip. Cloudyn raporlar hakkında bir genel bakış için bkz: [anlama maliyet raporları](understading-cost-reports.md). Makale ayrıca çeşitli seçenekler ve çoğu raporlarda kullanılan alanları açıklar.

## <a name="cost-analysis-reports"></a>Maliyet analiz raporları

Maliyet analiz raporları, bulut sağlayıcılardan faturalama verileri görüntüler. Raporları kullanarak, Grup ve fatura dosyasında listelenen çeşitli veri bölümü ayrıntılarına. Raporları bulut satıcıların ham faturalama verisi arasında ayrıntılı maliyeti Gezinti etkinleştirin.

Maliyet analiz raporları maliyetleri etiketlere göre grup değil. Etiket tabanlı raporlama yalnızca maliyet ayırma 360 kullanarak bir maliyet modeli oluşturduktan sonra ayarlamak maliyet ayırma raporlarında kullanılabilir.

### <a name="actual-cost-analysis"></a>Gerçek Maliyet Analizi

Gerçek maliyet analiz raporunu devam eden maliyetler ve tek seferlik ücretleri dahil olmak üzere, ana maliyet katkıda bulunanlar gösterir.

 Gerçek maliyet analizi raporu kullanın:

- Analiz etmek ve belirtilen bir zaman çerçevesinde harcanan fiili maliyetleri izleme
- Zamanlama bir eşik uyarısı
- Giderleri ve geri ödeme maliyetleri Çözümle

#### <a name="to-use-the-actual-cost-analysis-report"></a>Gerçek maliyet analizi raporu kullanmak için

En azından, aşağıdaki adımları gerçekleştirin. Diğer seçenekler ve alanlar de kullanabilirsiniz.

1. Bir tarih aralığı seçin.
2. Bir filtre seçin.

Bunlara incelemek ve daha ayrıntılı bilgileri görüntülemek için rapor sonuçlarını sağ tıklayabilirsiniz.

![Gerçek maliyet analizi raporu örneği](./media/use-reports/actual-cost-analysis.png)

### <a name="actual-cost-over-time"></a>Gerçek maliyet zamanla

Tanımlanan zaman çözünürlüğü üzerinden maliyet dağıtma standart maliyet analiz raporu gerçek zamanlı maliyet rapordur. Rapor eğilimler gözlemleyip harcama sıradışı algılamak olanak tanımak için zaman içinde harcama görüntüler. Bu rapor, devam eden maliyetler ve seçilen bir zaman çerçevesinde harcanan tek seferlik ayrılmış örnek ücretleri dahil olmak üzere, ana maliyet katkıda bulunanlar gösterir.

Gerçek zamanlı maliyet raporun kullanın:

- Maliyeti eğilimlerini zamanla bakın.
- Sıradışı maliyeti bulun.
- Amazon Web Hizmetleri ile ilgili tüm ilgili maliyeti soruları bulun.

#### <a name="to-use-the-actual-cost-over-time-report"></a>Gerçek zamanlı maliyeti raporu kullanmak için:

En azından, aşağıdaki adımları gerçekleştirin. Diğer seçenekler ve alanlar de kullanabilirsiniz.

- Bir tarih aralığı seçin.

Örneğin, kendi maliyet zaman içinde görüntülemek için grupları seçebilirsiniz. Ve ardından sonuçlarınızı daraltmak için filtreleri ekleyin.

![Gerçek zamanlı maliyeti raporu örneği](./media/use-reports/actual-cost-over-time.png)



### <a name="amortized-cost-reports"></a>Amortized maliyet raporları

Bu amortized maliyet raporları doğrusal duruma gösterir olmayan kullanım kümesini hizmet ücretlerinin ya da tek seferlik borç maliyetleri dayalı ve süre kendi kullanım ömrü boyunca eşit olarak üzerinden kendi maliyet yayılabilir.

Örneğin, bir kerelik ücretleri şunlar olabilir:

- Yıllık destek ücretleri
- Yıllık güvenlik bileşen ücretleri
- Ayrılmış örnekler ücretleri satın alma
- Bazı Azure Market öğesi

Fatura dosyasında tek seferlik ücretleri belirlenir hizmet tüketimini başlangıç ve bitiş tarihleri veya zaman damgası olduğunda eşit değerler. Cloudyn sonra bunları amortized gibi tek seferlik ücretleri tanır. İsteğe bağlı kullanım maliyetleri tüketim tabanlı diğer hizmetlerle amortized olamaz.

Amortized maliyetleri göstermek için gerçek maliyet üzerinden süresi raporu aşağıdaki örnek görüntüsünü gözden geçirin. Örnekte, bir maliyet ani 23 Ağustos gösterir. Normal günlük maliyet eğilimi karşılaştırıldığında anomali görünebilir. Kök neden analizi ve veri gezintisi olarak satın alınan ve o gün fatura tek seferlik bir ücret olan bir yıllık AWS hizmet APN ayırma, bu maliyet tanımlanmış. Bu maliyet sonraki bölümde nasıl amortized görebilirsiniz.

![Tek seferlik maliyetini gösteren gerçek zamanlı maliyeti raporu örneği](./media/use-reports/actual-amort-example.png)

#### <a name="to-use-the-amortized-cost-over-time-report"></a>Zaman içinde Amortized maliyeti raporu kullanmak için:

En azından, aşağıdaki adımları gerçekleştirin. Diğer seçenekler ve alanlar de kullanabilirsiniz.

1. Bir tarih aralığı seçin.
2. Bir hizmet ve bir sağlayıcı seçin.

Önceki örnekte taşıyan-İleri, tek seferlik maliyet şimdi aşağıdaki görüntüde amortized görebilirsiniz:

![Amortized zaman içinde maliyeti raporu örneği](./media/use-reports/amort-cost-over-time.png)

Önceki resimde APN ayırma maliyet amortized maliyetini zamanla gösterilir. Bu rapor, yıllık bir ayırma satın alma'olarak tek seferlik bir ücret İtfası ve APN maliyet gösterir. Her gün 1 olarak APN maliyet eşit olarak yayılır/ayırma eylemli maliyetinin 365th.

## <a name="cost-allocation-analysis-reports"></a>Ayırma analiz raporları maliyet

Maliyet ayırma analiz raporları, maliyet ayırma 360 kullanarak bir maliyet modeli oluşturduktan sonra kullanılabilir. Cloudyn maliyet/faturalama verilerini işler ve bulut hesaplarınız kullanım ve etiket verilerini verileri eşleştirir. Verileri eşleştirmek için kullanım verilerinizi erişim Cloudyn gerektirir. Kimlik bilgileri eksik, Kategorilere ayrılmamış kaynaklar olarak etiketlenir hesaplar.

### <a name="cost-analysis-report"></a>Maliyet Analizi raporu

Maliyet analiz raporu, bulut tüketim ve seçilen bir zaman çerçevesinde harcama bir anlayış sağlar. Maliyet ayırma Yöneticisi'nde ayarlanan ilkelerle maliyet analiz raporunda kullanılır.

Cloudyn Bu rapor nasıl hesaplar?

Hesap benzeşim uygulayarak ayırma her bağlantılı hesap bütünlüğünü korur Cloudyn sağlar. Belirli bir hizmet kullanmaz bir hesabının kendisi için ayrılan bu hizmetin maliyetlerin yok benzeşimi sağlar. Hesap bu hesabın kalır ve ayırma ilkeleri tarafından hesaplanmaz maliyetleri tahakkuk. Örneğin, beş bağlantılı hesapları olabilir. Yalnızca depolama hizmetleri üç tanesi kullanın, ardından maliyeti depolama hizmetleri, yalnızca üç hesaplarındaki etiketleri arasında tahsis edilir.

 Maliyet analiz raporu kullanın:

- Tüm dağıtımınız belirli bir zaman dilimi için birleşik bir görünümünü görüntüler.
- Maliyet modelinde oluşturulmuş ilkelerine bağlı olarak etiketi kategorilere göre maliyetlerini görüntüleyin.

#### <a name="to-use-the-cost-analysis-report"></a>Maliyet analiz raporu kullanmak için:

1. Bir tarih aralığı seçin.
2. Etiketler, gerektiği şekilde ekleyin.
3. Grupları ekleyin.
4. Daha önce oluşturduğunuz bir maliyet modeli seçin.

Aşağıdaki resimde bir örnek maliyet analiz raporu sunburst biçiminde gösterir. Çalma grupları göster. İç daire gösterilir ve dış halkası hizmet gösterir.

![Maliyet analiz raporu örneği](./media/use-reports/cost-analysis01.png)



Bir Tablo görünümünde aynı bilgileri örneği burada verilmiştir.

![Maliyet analiz raporu örneği](./media/use-reports/cost-analysis02.png)



### <a name="cost-over-time-report"></a>Zaman İçinde Maliyet raporu

Zaman içinde maliyet rapor zamanla eğilimler gözlemleyip sıradışı dağıtımınızda algılamak için harcama görüntüler. Aslında, tanımlanan bir süre içinde dağıtılmış maliyetleri gösterir. Rapor devam eden maliyetler ve seçilen bir zaman çerçevesinde harcanan tek seferlik ayrılmış örnek ücretleri dahil olmak üzere, ana maliyet katkıda bulunanlar gibi bilgileri içerir. Bu rapor kümesini maliyet Yöneticisi'nde 360 derecelik ilkeleri kullanılabilir.

Zaman içinde maliyet raporun kullanın:

- Zaman ve hangi etkiler sonraki bir gün (veya tarih aralığı) değiştirme üzerinden değişiklikleri görebilirsiniz.
- Belirli bir örnek için zamanla maliyetleri çözümleyin.
- Anlamak oluştu neden belirli bir örneği için bir maliyeti artışı.

#### <a name="to-use-the-cost-over-time-report"></a>Zaman içinde maliyeti raporu kullanmak için:

1. Bir tarih aralığı seçin.
2. Etiketler, gerektiği şekilde ekleyin.
3. Grupları ekleyin.
4. Daha önce oluşturduğunuz bir maliyet modeli seçin.
5. Gerçek maliyet veya amortized maliyetleri seçin.
6. İçin maliyet görünümü yeniden hesaplanması veya veri görünümü faturalama ham görünümüne ayırma kuralları uygulanıp uygulanmayacağını seçin.

Rapor bir örneği burada verilmiştir.

![Zaman içinde maliyet örneği](./media/use-reports/cost-over-time.png)



## <a name="next-steps"></a>Sonraki adımlar

- İlk öğreticide maliyet yönetimi için zaten tamamlanmış yapmadıysanız, hem okuma [gözden kullanım ve maliyetleri](tutorial-review-usage.md).
