---
title: "Azure Application Insights işlem Diagnostics | Microsoft Docs"
description: "Uygulama Öngörüler uçtan uca işlem tanılama"
services: application-insights
documentationcenter: .net
author: SoubhagyaDash
manager: victormu
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 01/19/2018
ms.author: sdash
ms.openlocfilehash: da945257a7a2548fe68498e5c908bd5487dad782
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
---
# <a name="unified-cross-component-transaction-diagnostics"></a>Birleşik arası bileşeni işlem tanılama

*Bu deneyim, şu anda önizlemede değil ve varolan sunucu tarafı tanılama Kanatlar değiştirir.*

Önizleme otomatik olarak sunucu tarafı telemetri tüm Application Insights izlenen bileşenler genelinde tek bir görünümde karşılık gelen yeni bir birleşik tanılama deneyimi sunar. Ayrı izleme anahtarı ile birden çok kaynaklarınız varsa önemli değildir; Application Insights temel ilişki algılar ve uygulama bileşeni, bağımlılık veya bir işlem yavaşlama veya hata nedeniyle özel durum kolayca tanılamanızı sağlar.

## <a name="what-does-component-mean-in-the-context-of-application-insights"></a>Bileşen Application Insights bağlamında anlamı nedir?

Dağıtılmış mikro uygulamanızın bağımsız olarak dağıtılabilir parçalarını bileşenleridir. Geliştiriciler ve işlemleri ekipleri kodu düzeyinde görünürlüğe veya bu uygulama bileşenleri tarafından oluşturulan telemetri erişimine sahip. 

* Bileşenleri "gözlemlenen" dış bağımlılıkları SQL gibi farklı takım/kuruluşunuz olmayabilir EventHub vb. erişim (kod veya telemetri için).
* Bileşenleri rol/sunucu/kapsayıcı örnekleri herhangi bir sayı üzerinde çalıştırın.
* Bileşenleri (abonelikler farklı olsa bile) ayrı Application Insights izleme anahtarı olabilir veya tek bir Application Insights izleme anahtarı için raporlama farklı roller oluşturabilirsiniz. Yeni deneyimi ayrıntıları nasıl bunlar ayarlanan bakılmaksızın, tüm bileşenler genelinde gösterir.

> [!Tip]
> En iyi sonuçlar için tüm bileşenler en son Application Insights kararlı SDK'ları ile işaretlendiğine emin olun. Farklı Application Insights kaynaklar varsa, bunların telemetri görüntülemek için uygun haklara sahip olun.

## <a name="enable-and-access"></a>Etkinleştirme ve erişim
Etkinleştir "Ayrıntıları birleşik: E2E işlem Tanılama" den [önizlemeleri listesi](app-insights-previews.md)

![Önizlemeyi etkinleştir](media/app-insights-e2eTxn-diagnostics/previews.png)

Bu önizleme, sunucu tarafı istekler, bağımlılıklar ve özel durumlar için şu anda kullanılabilir. Yeni deneyimi erişebilirsiniz **arama sonuçlarında**, **performans**, veya **hatası** önceliklendirme deneyimleri. Önizleme karşılık gelen Klasik Ayrıntılar dikey pencereleri değiştirir. 

![Performans örnekleri](media/app-insights-e2eTxn-diagnostics/performanceSamplesClickThrough.png)

## <a name="transaction-diagnostics-experience"></a>İşlem tanılama deneyimi 
Bu görünüm anahtar üç bölümden oluşur: arası bileşeni işlem grafiği, belirli bileşeni işlemi ve herhangi bir seçili telemetri öğeyi soldaki için Ayrıntılar bölmesinde tüm telemetri zaman serisi listesi.

![Anahtar bölümleri](media/app-insights-e2eTxn-diagnostics/3partsCrossComponent.png)

### <a name="cross-component-transaction-chart"></a>Çapraz bileşeni işlem grafik

Bu grafik yatay çubukları olan bir zaman çizelgesi istekleri ve bağımlılıkları süresince bileşenlerinde sağlar. Toplanan tüm özel durumlar da zaman çizelgesinde işaretlenir.

* Bu grafik ilk satırındaki giriş noktası temsil eden ilk bileşen gelen istek bu işlemde çağrılır. İşlemin tamamlanması geçen toplam süre aralıktır.
* Dış bağımlılıkları yapılan her çağrı bağımlılık türünü temsil eden simgelerle basit daraltılabilir olmayan satırları edilir.
* Diğer bileşenleri çağrıları daraltılabilir satırları aynıdır. Her satır, bileşenin çağrılan belirli bir işlemi karşılık gelir.
* Varsayılan olarak, istek, bağımlılık veya ilk başta seçtiğiniz özel durum grafikte görüntülenir.
* Sağ tarafta ayrıntılarını görmek için herhangi bir satır seçin. Profil Oluşturucu simgesi isteği satır üzerindeki veya bir özel durum satır bir hata ayıklama anlık görüntü simgesine tıklayarak ilgili ayrıntıları bölmesi açılır.

> [!NOTE]
Diğer bileşenleri çağrıları sahip iki satır: bir satır çağıran bileşen giden çağrısından (bağımlılık) temsil eder ve diğer satır çağrılan bileşen gelen isteğiyle karşılık gelir. İkinci aralarında birbirinden ayırmaya yardımcı olması için localhost olarak adlandırılır. Bize bildirin nasıl hakkında güncelleştirilmiş sunu düşündüğünüz sağ üst köşesinde geri bildirim kanalı kullanın.

### <a name="time-sequenced-telemetry-of-the-selected-component-operation"></a>Seçilen bileşenin işleminin zaman sıralı telemetri

Çapraz bileşeni işlem grafikte seçili herhangi bir satır, belirli bir bileşenin çağrılan bir işlemin ilgilidir. Bu seçilen bileşenin işlem alt bölüm başlığında yansıtılır. Belirli bir işlemle ilişkili tüm telemetri düz zaman dizisini görmek için bu bölümü açın. Sağ tarafta ilgili ayrıntıları görmek için bu listedeki herhangi bir telemetri öğesini seçin.

![Tüm telemetri zaman serisi](media/app-insights-e2eTxn-diagnostics/allTelemetryDrawerOpened.png)

### <a name="details-pane"></a>Ayrıntılar bölmesi

Bu bölmesi sol tarafta iki bölümlerden birine seçili öğelerden ayrıntılarını gösterir. "Tümünü Göster" tüm toplanan standart özniteliklerini listeler. Tüm özel öznitelikleri ayrı olarak standart kümesi altında listelenir.

![Özel durum ayrıntısı](media/app-insights-e2eTxn-diagnostics/exceptiondetail.png)

## <a name="profiler-and-snapshot-debugger"></a>Profil Oluşturucu ve anlık görüntü hata ayıklayıcı

[Uygulama Öngörüler profil oluşturucu](app-insights-profiler.md) veya [anlık görüntü hata ayıklayıcı](app-insights-snapshot-debugger.md) yardımcı kod düzey performans ve arıza sorunları tanılama ile. Bu deneyim profil oluşturucu izlemeleri görebilirsiniz veya herhangi bir bileşenin tek bir anlık görüntüler'i tıklatın.

![Hata ayıklayıcı tümleştirme](media/app-insights-e2eTxn-diagnostics/debugSnapshot.png)

## <a name="faq"></a>SSS

*Grafikte tek bir bileşen görüyorum ve diğerleri yalnızca bu bileşenleri içinde ne, herhangi bir ayrıntı olmadan dış bağımlılıklar olarak gösterildiğinden.*

Olası nedenler:

* Diğer bileşenleri, Application Insights ile işaretlendiğine?
* Bunlar, en son kararlı Application Insights SDK'sı kullanıyor musunuz?
* Bu bileşenleri ayrı Application Insights kaynakları varsa, bunları gerekli erişim hakkınız?

Erişiminiz ve bileşenlerinin en son uygulama Insights SDK'ları ile işaretlendiğine, üst sağ geri bildirim kanalı bize bildirin.

*Yalnızca dış bağımlılıkları sahibim. Bu önizleme hakkında dikkat etmelisiniz?*

Evet. Yeni deneyimi, tek bir görünümde tüm ilgili sunucu tarafı telemetri birleştirir. Eski ayrıntı Kanatlar bu deneyimi gelecekte değiştirilecek, böylelikle deneyin ve bize bildirin. İşte bu şekilde bir tek bileşen işlem için görünür:

![Tek bileşen deneyimi](media/app-insights-e2eTxn-diagnostics/singleComponent.png)

*Bağımlılıklar için yinelenen satırları görüyorum Bu beklenen bir durumdur?*

Şu anda biz yerine gelen istek ayrı giden bağımlılık çağrısı gösteriliyor. Genellikle, iki çağrıları yalnızca gidiş dönüş ağ nedeniyle farklı olduğu süre değeri ile aynı görünür. Bunları ayırt etmenize yardımcı olmak için bir sunucu simgesiyle birlikte "localhost" isteği alındığında bileşen aradığınız. Bu satır hemen bağımlılık satır izler. Bu verilerin sunumunu karmaşıktır? Bize geri bildirim verin!

*Ne hakkında saati farklı bileşeni örneklerinde Eğer?*

Zaman çizelgelerini saati eğriltir işlem grafikte için ayarlanır. Tam zaman damgalarını analizi kullanarak veya ayrıntılar bölmesinde görebilirsiniz.

*Yeni deneyimi çoğu ilgili öğeler sorgu neden eksik?*

Bu tasarım gereğidir. Tüm ilişkili öğeleri tüm bileşenleri arasında zaten sol taraftaki (üst ve alt bölümleri) kullanılabilir. Sol tarafta kapsamıyordur iki ilgili öğeler yeni deneyime sahip: beş dakika önce ve sonra bu olay ve kullanıcı zaman çizelgesi tüm telemetri.

*Neden yeni deneyimi eski Kanatlar severek özelliği yok?*

Bize geri bildirimde bulunun! Bu deneyim eski görünümleri kullanım aynı zamanda genellikle, kullanılabilir olmadan önce sorunları çözmek istiyoruz. Şimdilik, eski Kanatlar geri dönmek için Önizleme'yi devre dışı bırakabilirsiniz.

## <a name="known-issues"></a>Bilinen Sorunlar

* Uygulama eşlemesi hatası örneklerinden eski ayrıntı Kanatlar bağlayın.
* Arama sonuçlarında autocluster dayalı Öngörüler eski ayrıntı Kanatlar bağlayın.
* Çalışma öğesi tümleştirmeyi yeni deneyimi kullanılabilir değil.