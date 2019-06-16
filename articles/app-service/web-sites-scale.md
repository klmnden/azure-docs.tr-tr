---
title: Özellikleri ve yetenekleriyle - Azure App Service ' ölçeklendirme | Microsoft Docs
description: Kapasite ve özellikler eklemek için Azure App Service'te bir uygulama ölçeğini öğrenin.
services: app-service
documentationcenter: ''
author: cephalin
manager: erikre
editor: mollybos
ms.assetid: f7091b25-b2b6-48da-8d4a-dcf9b7baccab
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2016
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 98d3d1f6fc0f2f30196f360811808579dfbab312
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60832151"
---
# <a name="scale-up-an-app-in-azure"></a>Azure'da uygulamanın ölçeğini

> [!NOTE]
> Yeni **PremiumV2** katmanı daha hızlı CPU'lar, SSD depolama sağlar ve mevcut fiyatlandırma katmanları bellek / çekirdek oranı iki katına çıkar. Performans avantajı ile uygulamalarınızı daha az örneklerinde çalıştırarak paradan tasarruf sağlayabilirsiniz. Kadar ölçeklendirme yapmanızı **PremiumV2** katmanı için bkz: [App Service için yapılandırma PremiumV2 katmanını](app-service-configure-premium-tier.md).
>

Bu makalede, uygulamanızı Azure App Service'te ölçeklendirme işlemini göstermektedir. Yukarı iki iş akışları için ölçeklendirme, Ölçek vardır ve ölçek genişletme ve bu makalede, iş akışı ölçeği açıklar.

* [Ölçeği artırma](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): Daha fazla CPU, bellek, disk alanı ve adanmış sanal makinelerde (VM), özel etki alanları ve sertifikalar, hazırlama yuvaları, otomatik ölçeklendirme ve daha fazla özellikten alın. Uygulamanızın ait olduğu App Service planının fiyatlandırma katmanını değiştirerek ölçeği artırmanıza.
* [Ölçeği genişletme](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): Uygulamanızı çalıştıran VM örneği sayısını artırır.
  En çok 20 örneklerine fiyatlandırma katmanınıza bağlı ölçeği genişletebilirsiniz. [App Service ortamları](environment/intro.md) içinde **yalıtılmış** daha fazla katmanı ölçek genişletme sayınız 100 örneğe kadar artırır. Ölçek genişletme hakkında daha fazla bilgi için bkz. [örnek sayısını elle veya otomatik olarak ölçeklendirme](../monitoring-and-diagnostics/insights-how-to-scale.md). Burada, örnek sayısını otomatik olarak önceden tanımlanmış kurallar ve zamanlamaları göre ölçeklendirme olan otomatik ölçeklendirme, kullanmayı öğrenin.

Ölçek ayarları uygulamak ve tüm uygulamaları etkiler. yalnızca saniye Süren, [App Service planı](../app-service/overview-hosting-plans.md).
Bunlar, kodunuzu değiştirin veya uygulamanızı yeniden dağıtmanız gerekmez.

Fiyatlandırma ve Özellikler ayrı App Service planları hakkında daha fazla bilgi için bkz: [App Service fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/web-sites/).  

> [!NOTE]
> Bir App Service planından geçiş yapmadan önce **ücretsiz** katmanı, önce kaldırmanız gerekir [harcama limitleri](https://azure.microsoft.com/pricing/spending-limits/) Azure aboneliğiniz için bir yerde. Görüntülemek veya Microsoft Azure App Service aboneliğiniz için seçeneklerini değiştirmek için bkz: [Microsoft Azure abonelikleri][azuresubscriptions].
> 
> 

<a name="scalingsharedorbasic"></a>
<a name="scalingstandard"></a>

## <a name="scale-up-your-pricing-tier"></a>Fiyatlandırma katmanınızı ölçeklendirin
1. Tarayıcınızda açın [Azure portalında][portal].
2. App Service uygulaması sayfanızın tıklayın **tüm ayarlar**ve ardından **ölçeği Artır**.
   
    ![Azure uygulamanızı ölçeklendirme gidin.][ChooseWHP]
3. Katmanınızı seçin ve ardından **Uygula**.
   
    **Bildirimleri** flash yeşil sekmesi **başarı** işlemi tamamlandıktan sonra.

<a name="ScalingSQLServer"></a>

## <a name="scale-related-resources"></a>İlgili kaynakları ölçeklendirme
Uygulamanızı Azure SQL veritabanı veya Azure depolama gibi diğer sistemlerde bağlıysa, bu kaynakları ayrı olarak ölçeklendirebilirsiniz. Bu kaynaklar, App Service planı tarafından yönetilmiyor.

1. İçinde **Essentials**, tıklayın **kaynak grubu** bağlantı.
   
    ![Ölçeği artırma Azure uygulamanızın ilgili kaynakları](./media/web-sites-scale/RGEssentialsLink.png)
2. İçinde **özeti** parçası **kaynak grubu** sayfasında, ölçeklendirmek istediğiniz bir kaynağa tıklayın. Aşağıdaki ekran görüntüsünde, SQL veritabanı kaynağı ve Azure depolama kaynağı gösterir.
   
    ![Azure uygulamanızı ölçeklendirmek için kaynak grubu sayfasına gidin](./media/web-sites-scale/ResourceGroup.png)
3. Bir SQL veritabanı kaynağı için tıklatın **ayarları** > **fiyatlandırma katmanı** fiyatlandırma katmanını ölçeklendirmek için.
   
    ![SQL veritabanı arka ucu Azure uygulamanızın ölçeğini](./media/web-sites-scale/ScaleDatabase.png)
   
    Ayrıca etkinleştirebilirsiniz [coğrafi çoğaltma](../sql-database/sql-database-geo-replication-overview.md) SQL veritabanı Örneğiniz için.
   
    Bir Azure depolama kaynağı için **ayarları** > **yapılandırma** depolama seçenekleriniz ölçeklendirmek için.
   
    ![Azure, uygulamanız tarafından kullanılan Azure depolama hesabı ölçeğini](./media/web-sites-scale/ScaleStorage.png)

<a name="OtherFeatures"></a>
<a name="devfeatures"></a>

## <a name="compare-pricing-tiers"></a>Fiyatlandırma katmanlarını karşılaştırın
Her fiyatlandırma katmanının VM boyutları gibi ayrıntılı bilgi için bkz. [App Service fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/web-sites/).
Hizmet sınırları, kotalar ve kısıtlamalar ve desteklenen özellikler her katmanında tablo için bkz: [App Service limitleri](../azure-subscription-service-limits.md#app-service-limits).

<a name="Next Steps"></a>

## <a name="next-steps"></a>Sonraki adımlar
* Fiyatlandırma, destek ve SLA hakkında daha fazla bilgi için aşağıdaki bağlantıları ziyaret edin:
  
    [Veri aktarımları fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/data-transfers/)
  
    [Microsoft Azure destek planları](https://azure.microsoft.com/support/plans/)
  
    [Hizmet Düzeyi Sözleşmeleri](https://azure.microsoft.com/support/legal/sla/)
  
    [SQL veritabanı fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/sql-database/)
  
    [Sanal makine ve bulut hizmeti boyutları için Microsoft Azure][vmsizes]
  
* Azure App Service hakkında daha fazla bilgi için en iyi uygulamalar, ölçeklenebilir ve dayanıklı bir mimari oluşturmak gibi bkz [en iyi uygulamalar: Azure App Service Web Apps'e](https://azure.microsoft.com/blog/best-practices-windows-azure-websites-waws/).
* App Service uygulamalarını ölçeklendirme hakkında daha fazla video için aşağıdaki kaynaklara bakın:
  
  * [Ne zaman Azure Web siteleri - Stefan Schackow ile ölçeklendirme](https://azure.microsoft.com/resources/videos/azure-web-sites-free-vs-standard-scaling/)
  * [Otomatik ölçeklendirme Azure Web siteleri, CPU veya zamanlanmış - Stefan Schackow](https://azure.microsoft.com/resources/videos/auto-scaling-azure-web-sites/)
  * [-Stefan Schackow ile Azure Web siteleri ölçeklendirme](https://azure.microsoft.com/resources/videos/how-azure-web-sites-scale/)

<!-- LINKS -->
[vmsizes]:https://azure.microsoft.com/pricing/details/app-service/
[SQLaccountsbilling]:https://go.microsoft.com/fwlink/?LinkId=234930
[azuresubscriptions]:https://account.windowsazure.com/subscriptions
[portal]: https://portal.azure.com/

<!-- IMAGES -->
[ChooseWHP]: ./media/web-sites-scale/scale1ChooseWHP.png
[ResourceGroup]: ./media/web-sites-scale/scale10ResourceGroup.png
[ScaleDatabase]: ./media/web-sites-scale/scale11SQLScale.png
[GeoReplication]: ./media/web-sites-scale/scale12SQLGeoReplication.png
