---
title: Azure Application Insights uygulama eşlemesinde | Microsoft Docs
description: Karmaşık bir uygulama topolojileri uygulama eşlemesi ile izleme
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 3bf37fe9-70d7-4229-98d6-4f624d256c36
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 03/14/2017
ms.reviewer: Soubhagya.Dash
ms.author: mbullwin
ms.openlocfilehash: 539becf272194a116355c6a0491042d40e1e7494
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35293971"
---
# <a name="application-map-triage-distributed-applications"></a>Uygulama eşlemesi: Dağıtılmış uygulamalar Önceliklendirme
Uygulama eşlemesi nokta performans sorunları veya hata etkin, dağıtılmış uygulamanın tüm bileşenleri arasında yardımcı olur. Harita her bir düğümde bir uygulama bileşeni veya bağımlılıklarını temsil eder; durumu KPI sahip ve durum uyarır. Aracılığıyla herhangi bir bileşeni Application Insights olaylarını gibi daha ayrıntılı tanılama tıklayabilirsiniz. Uygulamanızı Azure hizmetlerini kullanıyorsa, üzerinden SQL Database Advisor önerileri gibi Azure tanılama tıklatabilirsiniz.

## <a name="what-is-a-component"></a>Bir bileşeni nedir?

Dağıtılmış mikro uygulamanızın bağımsız olarak dağıtılabilir parçalarını bileşenleridir. Geliştiriciler ve işlemleri ekipleri kodu düzeyinde görünürlüğe veya bu uygulama bileşenleri tarafından oluşturulan telemetri erişimine sahip. 

* Bileşenleri "gözlemlenen" dış bağımlılıkları SQL gibi farklı takım/kuruluşunuz olmayabilir EventHub vb. erişim (kod veya telemetri için).
* Bileşenleri rol/sunucu/kapsayıcı örnekleri herhangi bir sayı üzerinde çalıştırın.
* Bileşenleri (abonelikler farklı olsa bile) ayrı Application Insights izleme anahtarı olabilir veya tek bir Application Insights izleme anahtarı için raporlama farklı roller oluşturabilirsiniz. Önizleme harita deneyimi bileşenlerini nasıl ayarladıktan bakmaksızın gösterir.

## <a name="composite-application-map-preview"></a>Bileşik uygulama eşlemesi (Önizleme)
*Bu ilk önizleme sürümü ve size daha fazla özellik bu harita ekleme. Yeni deneyimi görüşlerinizi almak memnuniyet duyarız. Önizleme ve klasik deneyimlerini arasında kolayca geçiş yapabilirsiniz.*

"Bileşik uygulama eşlemesi" etkinleştirme başlangıç [önizlemeleri listesi](app-insights-previews.md), veya "Önizleme haritada" sağ üst köşesinde Değiştir'i tıklatın. Bu geçiş, Klasik deneyimine geçiş yapmak için kullanabilirsiniz.
![Önizleme harita etkinleştir](media/app-insights-app-map/preview-from-classic.png)

>[!Note]
Bu önizleme önceki "Birden çok rol uygulama eşlemesi" Önizleme değiştirir. Şu anda bu uygulama bileşeni bağımlılıkları birden çok düzeyi arasında tüm topolojisini görüntülemek için kullanın. Bize geri bildirim verin, biz Klasik harita destekler ne benzer daha fazla yetenekleri ekleme.

Birden çok düzeyde ilgili uygulama bileşenleri arasında tam uygulama topolojisi görebilirsiniz. Bileşenleri farklı Application Insights kaynaklar ya da farklı rollerdeki tek bir kaynak olabilir. Uygulama eşleme bileşenleri yüklü Application Insights SDK'sı ile sunucu arasında yapılan aşağıdaki HTTP bağımlılık çağrıları tarafından bulur. 

Bu deneyim bileşenleri aşamalı bulma ile başlar. Önizleme ilk yüklediğinizde, sorguları bir dizi bu bileşenle ilgili bileşenler bulmak için tetiklenir. Bulundukları bir düğme sol üst köşesinde, uygulamanızda bileşenleri sayısı ile güncelleştirir. 
![Önizleme eşleme](media/app-insights-app-map/preview.png)

"Güncelleştirme eşleme bileşenleri" tıklatıldığında harita noktasındaki kadar bulunan tüm bileşenlerle yenilenir.
![Önizleme yüklenen eşleme](media/app-insights-app-map/components-loaded-hierarchical.png)

Tüm bileşenleri tek bir Application Insights kaynağı içindeki roller varsa, bu bulma adım gerekli değildir. Bu tür bir uygulama için ilk yükleme tüm bileşenlerini sahip olur.

Yeni bir deneyim anahtar hedefleri bileşenleri yüzlerce karmaşık topolojiler görselleştirmek için biridir. Yeni deneyimi yakınlaştırma destekler ve, yakınlaştırma bileşenini gibi ayrıntısı ekler. Daha fazla bir bakışta bileşenleri ve daha yüksek başarısızlık oranları hala nokta bileşenleriyle görüntülemek için uzaklaştırma. 

![Yakınlaştırma düzeyleri](media/app-insights-app-map/zoom-levels.png)

Performans ve bu bileşen için hata değerlendirme deneyimi gidin ve ilgili Öngörüler görmek için herhangi bir bileşeni tıklayın.

![Çıkma](media/app-insights-app-map/preview-flyout.png)


## <a name="classic-application-map"></a>Klasik uygulama eşlemesi

Harita gösterir:

* Kullanılabilirlik testleri
* İstemci tarafı bileşen (JavaScript SDK'sı ile izlenen)
* Sunucu tarafı bileşeni
* İstemci ve sunucu bileşenleri bağımlılıkları

![Uygulama eşleme](./media/app-insights-app-map/02.png)

Genişletme ve bağımlılık bağlantı gruplarına daraltma:

![daralt](./media/app-insights-app-map/03.png)

Bir tür (SQL, HTTP vb.) pek çok bağımlılık varsa, bunlar gruplandırılmış görünebilir. 

![Gruplandırılmış bağımlılıkları](./media/app-insights-app-map/03-2.png)

## <a name="spot-problems"></a>Nokta sorunları
Her düğüm bu bileşen için yükleme, performans ve hata hızlarını gibi ilgili performans göstergelerini içerir. 

Uyarı simgeleri olası sorunlar vurgulayın. Turuncu bir uyarı isteklerinde sayfa görünümleri veya bağımlılık çağrıları hataları olduğu anlamına gelir. Kırmızı bir hata oranı %5 yukarıda anlamına gelir. Bu eşikler ayarlamak istiyorsanız, Seçenekleri'ni açın.

![hata simgeleri](./media/app-insights-app-map/04.png)

Etkin Göster yukarı da uyarır: 

![Etkin uyarılar](./media/app-insights-app-map/05.png)

SQL Azure kullanırsanız, ne zaman gösteren bir simge yoktur nasıl performansını iyileştirebilir ilişkin öneriler vardır. 

![Azure önerisi](./media/app-insights-app-map/06.png)

Daha fazla bilgi almak için herhangi bir simgesini tıklatın:

![Azure önerisi](./media/app-insights-app-map/07.png)

## <a name="diagnostic-click-through"></a>Tanılama geçişli tıklatma
Harita üzerinde düğümlerinin her biri için tanılama aracılığıyla hedeflenen tıklatın sunar. Seçenekler düğüm türüne bağlı olarak değişir.

![Sunucu seçenekleri](./media/app-insights-app-map/09.png)

Azure üzerinde barındırılan bileşenler için bunları doğrudan bağlantıların seçenekleri içerir.

## <a name="filters-and-time-range"></a>Filtreler ve zaman aralığı
Varsayılan olarak, seçilen zaman aralığı için kullanılabilir tüm verileri harita özetler. Ancak, yalnızca belirli işlem adları veya bağımlılıkları içerecek şekilde filtre uygulayabilirsiniz.

* İşlem adı: Bu sayfa görünümleri ve sunucu tarafı istek türleri içerir. Bu seçenek, yalnızca seçili işlemler için istemci/sunucu-tarafı düğümde KPI eşlemesini gösterir. Bu belirli işlemler bağlamında adlı bağımlılıkları gösterir.
* Bağımlılık temel name: Bu, sunucu tarafı bağımlılıkları ve AJAX tarayıcı bağımlılıklar içerir. Özel bağımlılık telemetrisi TrackDependency API ile rapor ise, bunlar ayrıca burada görünür. Haritada göstermek için bağımlılıklar seçebilirsiniz. Şu anda bu seçimi sunucu tarafı istekleri ya da istemci-tarafı sayfa görünümleri filtre uygulamaz.

![Filtrelerini ayarlama](./media/app-insights-app-map/11.png)

## <a name="save-filters"></a>Filtreleri kaydet
Filtre uygulanmış bir görünüm üzerine uyguladığınız filtreleri Kaydet sabitlemek bir [Pano](app-insights-dashboards.md).

![Panoya sabitle](./media/app-insights-app-map/12.png)

## <a name="error-pane"></a>Hata bölmesi
Harita düğümünde tıkladığınızda bir hata bölmesi sağ taraftaki hataları bu düğüm için özetleme görüntülenir. Hataları ilk işlem Kimliğine göre gruplandırılmış ve sorun kimliğine göre gruplandırılmış

![Hata bölmesi](./media/app-insights-app-map/error-pane.png)

Bir arıza tıklatarak bu hatanın en son örneğine alır.

## <a name="resource-health"></a>Kaynak durumu
Bazı kaynak türleri için kaynak durumu hata bölmesinin üst kısmında görüntülenir. Örneğin, bir SQL düğümü tıklatarak veritabanı sistem durumu ve puanlı herhangi bir uyarı gösterir.

![Kaynak durumu](./media/app-insights-app-map/resource-health.png)

Bu kaynak için standart genel bakış ölçümlerini görüntülemek için kaynak adı tıklatabilirsiniz.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player] 

## <a name="feedback"></a>Geri Bildirim
Portal geri bildirimi seçeneği aracılığıyla geri bildirim sağlayın.

![MapLink-1 görüntüsü](./media/app-insights-app-map/13.png)


## <a name="next-steps"></a>Sonraki adımlar

* [Azure portal](https://portal.azure.com)
