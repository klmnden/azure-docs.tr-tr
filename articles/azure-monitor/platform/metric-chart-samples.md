---
title: Azure İzleyici ölçüm grafiğini örnekleri
description: Azure İzleyici, veri görselleştirme hakkında bilgi edinin.
author: vgorbenko
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: vitalyg
ms.subservice: metrics
ms.openlocfilehash: bbfeb428d38c23955df4497242184499349aecf9
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60256693"
---
# <a name="metric-chart-samples"></a>Ölçüm grafiğini örnekleri

Azure platformu teklifleri [binlerce ölçüm üzerinden](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported)boyutları olan çoğu. Kullanarak [boyut filtreleri](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-charts), uygulama [bölme](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-charts), grafik türü denetleme ve güçlü tanılama görünümleri ve sistem durumunu öngörü sağlayan panolar oluşturabilir grafiği ayarları ayarlama Altyapı ve uygulamalar. Bu makale bazı örnekler kullanarak oluşturabileceğiniz grafiklerin [ölçüm Gezgini](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-charts) ve bu grafiklerin her yapılandırmak için gereken adımları açıklar.

Harika grafikler örnekleri dünya ile paylaşmak istiyorsunuz? Bu sayfaya github'da katkıda ve kendi grafik örneklerinizi paylaşabilirsiniz!

## <a name="website-cpu-utilization-by-server-instances"></a>Sunucu örnekleri Web sitesinin CPU kullanımı

Bu grafik, bir App Service için CPU kabul edilebilir aralık içinde olan ve yükü doğru dağıtılmış olup olmadığını belirlemek için örneği tarafından böler gösterir. Uygulama bir tek sunucu örneğinde, 6 AM önce çalışıyordu ve ardından başka bir örneği ekleyerek yukarı ölçeklendirilemez grafikten görebilirsiniz.

![Çizgi grafik sunucu örneği tarafından ortalama cpu yüzdesi](./media/metric-chart-samples/cpu-by-instance.png)

### <a name="how-to-configure-this-chart"></a>Bu grafiği yapılandırmak nasıl?

App Service kaynağınızı seçin ve bulma **CPU yüzdesi** ölçümü. Ardından **uygulamak bölme** seçip **örneği** boyut.

## <a name="application-availability-by-region"></a>Bölgelere göre uygulama kullanılabilirliği

Hangi coğrafi konumlar sorunlarınız uygulamanızın kullanılabilirliğini belirlemek için bölgeye göre görüntüleyin. Bu grafik, Application Insights kullanılabilirlik ölçümü gösterir. Doğu ABD veri merkezinde kullanılabilirliği bir sorun olmadığını izlenen uygulama var, ancak Batı ABD ve Doğu Asya kısmi kullanılabilirlik sorunu yaşıyor görebilirsiniz.

![Konuma göre ortalama kullanılabilirlik grafiği](./media/metric-chart-samples/availability-run-location.png)

### <a name="how-to-configure-this-chart"></a>Bu grafiği yapılandırmak nasıl?

İlk açmak gerekir [Application Insights kullanılabilirlik](https://docs.microsoft.com/azure/azure-monitor/app/monitor-web-app-availability) Web siteniz için izleme. Bundan sonra Application Insights kaynağınızı seçin ve kullanılabilirlik ölçümü seçin. Temel bölme uygulamak **çalıştırma yeri** boyut.

## <a name="volume-of-storage-account-transactions-by-api-name"></a>Depolama hesabı işlemlerinin API adı tarafından

Depolama hesabı kaynak fazla hacimli işlemler yaşıyor. İşlem ölçüm API'si için aşırı yük sorumlu olan tanımlamak için kullanabilirsiniz. Aşağıdaki grafikte filtreleme ve yalnızca ilgilendiğiniz API çağrıları görünümüne daraltmak için bölme (API adı) aynı boyutta ile yapılandırılmış olduğuna dikkat edin:

![API işlemlerinin çubuk grafik](./media/metric-chart-samples/transactions-by-api.png)

### <a name="how-to-configure-this-chart"></a>Bu grafiği yapılandırmak nasıl?

Ölçüm Seçici'de, depolama hesabınızı seçin ve **işlemleri** ölçümü. Geçiş için grafik türü **çubuk grafik**. Tıklayın **uygulamak bölme** ve select boyut **API adı**. Ardından **Filtre Ekle** ve çekme **API adı** yine boyut. Filtre iletişim kutusunda vykreslit v grafu istediğiniz API'ları seçin.

## <a name="next-steps"></a>Sonraki adımlar

* Azure İzleyici hakkında bilgi edinin [çalışma kitapları](../../azure-monitor/app/usage-workbooks.md)
* Daha fazla bilgi edinin [ölçüm Gezgini](metrics-charts.md)