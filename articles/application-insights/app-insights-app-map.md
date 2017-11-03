---
title: "Azure Application Insights uygulama eşlemesinde | Microsoft Docs"
description: "Uygulama bileşenleri arasındaki bağımlılıkları görsel sunumu KPI'ları ve uyarılarla etiketli."
services: application-insights
documentationcenter: 
author: SoubhagyaDash
manager: carmonm
ms.assetid: 3bf37fe9-70d7-4229-98d6-4f624d256c36
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: mbullwin
ms.openlocfilehash: e1eb2177d6032142781e6e31af6c7f6313d38f4d
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="application-map-in-application-insights"></a>Application ınsights'ta uygulama eşlemesi
İçinde [Azure Application Insights](app-insights-overview.md), uygulama eşlemesi olan uygulama bileşenlerinizin bağımlılık ilişkilerini visual düzeni. Her bileşen yük, performans, hataları ve Uyarıları gibi bir performans sorunu veya hatası neden herhangi bir bileşeni keşfetmenize yardımcı olmak için KPI'ları gösterir. Aracılığıyla herhangi bir bileşeni Application Insights olaylarını gibi daha ayrıntılı tanılama tıklayabilirsiniz. Uygulamanızı Azure hizmetlerini kullanıyorsa, üzerinden SQL Database Advisor önerileri gibi Azure tanılama tıklatabilirsiniz.

Diğer grafikler gibi bir uygulama eşlemesi tam olarak işlevsel olduğu Azure panoya sabitleyebilirsiniz. 

## <a name="open-the-application-map"></a>Uygulama eşlemesi açın
Uygulamanız için genel bakış dikey penceresinden harita açın:

![Uygulama Eşlem'i açın](./media/app-insights-app-map/01.png)

![Uygulama eşleme](./media/app-insights-app-map/02.png)

Harita gösterir:

* Kullanılabilirlik testleri
* İstemci tarafı bileşen (JavaScript SDK'sı ile izlenen)
* Sunucu tarafı bileşeni
* İstemci ve sunucu bileşenleri bağımlılıkları

Genişletme ve bağımlılık bağlantı gruplarına daraltma:

![Daralt](./media/app-insights-app-map/03.png)

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

## <a name="save-filters"></a>Filtreleri Kaydet
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

## <a name="end-to-end-system-app-maps"></a>Uçtan uca sistem uygulama eşlemeleri

*SDK'sı sürüm 2.3 veya üstü gerektirir*

Uygulamanız çeşitli bileşenleri - Örneğin, bir arka uç hizmeti Ayrıca web uygulaması'na - sahip sonra bunları gösterebilir tüm bir tümleşik uygulama harita üzerinde.

![Filtrelerini ayarlama](./media/app-insights-app-map/multi-component-app-map.png)

Uygulama harita yüklü Application Insights SDK'sı ile sunucu arasında yapılan tüm HTTP bağımlılık çağrıları izleyerek sunucu düğümleri bulur. Her bir Application Insights kaynağı, bir sunucu içeren varsayılır.

### <a name="multi-role-app-map-preview"></a>Birden çok rol uygulama eşleme (Önizleme)

Önizleme birden çok rol uygulama eşleme özelliğini uygulama eşlemesi aynı Application Insights kaynağına veri gönderilirken birden fazla sunucuyla sayesinde / izleme anahtarı. Harita sunucuları, telemetri öğeler üzerinde cloud_RoleName özelliği tarafından ayrılmış. Ayarlama *birden çok rol uygulama eşlemesi* için *üzerinde* bu yapılandırmayı etkinleştirmek için önizlemeleri dikey penceresinden.

Bu yaklaşım, bir mikro hizmetler uygulamasındaki ya da tek bir Application Insights kaynağı içinde birden çok sunucudaki olayları ilişkilendirmek istediğiniz diğer senaryolarda istenebilir.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player] 

## <a name="feedback"></a>Geri Bildirim
Portal geri bildirimi seçeneği aracılığıyla geri bildirim sağlayın.

![MapLink-1 görüntüsü](./media/app-insights-app-map/13.png)


## <a name="next-steps"></a>Sonraki adımlar

* [Azure portal](https://portal.azure.com)
