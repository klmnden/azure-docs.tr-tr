---
title: "Bir Azure uygulamasında ölçeklendirin | Microsoft Docs"
description: "Kapasite ve özellikleri eklemek için Azure App Service'te bir uygulama ölçeklendirin öğrenin."
services: app-service
documentationcenter: 
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
ms.openlocfilehash: 248b96cc97367ca2cb3fd82c9824d43dfee43c0a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="scale-up-an-app-in-azure"></a>Bir Azure uygulamasında ölçeklendirin

> [!NOTE]
> Yeni **PremiumV2** katmanı daha hızlı CPU'lar, SSD depolama sağlar ve varolan fiyatlandırma katmanlarını daha bellek çekirdek oranı çift. En fazla ölçeklendirmek için **PremiumV2** katmanı için bkz: [uygulama hizmetin yapılandırma PremiumV2 katmanı](app-service-configure-premium-tier.md).
>

Bu makalede, uygulamanızı Azure App Service'te ölçeklendirme gösterilmektedir. Yukarı ölçeklendirme, ölçeklendirme için iki iş akışlarını vardır ve ölçek genişletme ve bu makalede ölçek büyütme iş akışını açıklar.

* [Ölçeği artırma](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): daha fazla CPU, bellek, disk alanı ve ayrılmış sanal makine (VM), özel etki alanları ve sertifikalar, hazırlama yuvaları, otomatik ölçeklendirme ve daha fazla özellikten alın. Uygulamanızın ait olduğu App Service planının fiyatlandırma katmanını değiştirerek ölçeği.
* [Ölçeği genişletme](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): uygulamanızı çalıştıran VM örneği sayısını artırın.
  Out kadar 20 örneklerine fiyatlandırma katmanınıza bağlı ölçeklendirebilirsiniz. [Uygulama hizmeti ortamları](environment/intro.md) içinde **Isolated** daha fazla katmanı genişletme sayınız 100 örneklerine artırır. Genişletme hakkında daha fazla bilgi için bkz: [örnek sayısı el ile veya otomatik olarak ölçeklendirme](../monitoring-and-diagnostics/insights-how-to-scale.md). Vardır, örnek sayısı otomatik olarak önceden tanımlanmış kurallar ve zamanlamaları göre ölçeklendirme olduğu otomatik ölçeklendirmeyi kullanmayı öğrenin.

Ölçek ayarları uygulamak ve tüm uygulamaları etkiler yalnızca saniye sürebilir, [uygulama hizmeti planı](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
Bunlar, uygulamanızın yeniden dağıtın veya kodunuzu değiştirmek gerektirmez.

Fiyatlandırma ve tek tek uygulama hizmeti planları özellikleri hakkında daha fazla bilgi için bkz: [App Service fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/web-sites/).  

> [!NOTE]
> Bir uygulama hizmeti planında geçiş yapmadan önce **serbest** katmanı, ilk kaldırmalısınız [harcama limitlerini](https://azure.microsoft.com/pricing/spending-limits/) Azure aboneliğiniz için yerinde. Görüntülemek veya Microsoft Azure uygulama hizmeti aboneliğinizi seçeneklerini değiştirmek için bkz: [Microsoft Azure abonelikleri][azuresubscriptions].
> 
> 

<a name="scalingsharedorbasic"></a>
<a name="scalingstandard"></a>

## <a name="scale-up-your-pricing-tier"></a>Fiyatlandırma katmanınızı ölçeklendirme
1. Tarayıcınızda açın [Azure portal][portal].
2. Uygulama hizmeti uygulaması sayfasında tıklatın **tüm ayarları**ve ardından **ölçeği Artır**.
   
    ![Azure uygulamanızı ölçeklendirmek için gidin.][ChooseWHP]
3. Katmanınızı seçin ve ardından **seçin**.
   
    **Bildirimleri** flash yeşil sekmesi **başarı** işlemi tamamlandıktan sonra.

<a name="ScalingSQLServer"></a>

## <a name="scale-related-resources"></a>İlgili kaynaklar ölçeklendirme
Uygulamanızı Azure SQL veritabanı ya da Azure depolama gibi diğer hizmetler yapılandırmasanız bu kaynakları ayrı olarak ölçeklendirebilirsiniz. Uygulama hizmeti planı tarafından yönetilen bu kaynakları değil.

1. İçinde **Essentials**, tıklatın **kaynak grubu** bağlantı.
   
    ![Azure, uygulamanızın ilgili kaynakları ölçeklendirme](./media/web-sites-scale/RGEssentialsLink.png)
2. İçinde **Özet** parçası **kaynak grubu** sayfasında, ölçeklendirmek istediğiniz bir kaynağa tıklayın. Aşağıdaki ekran görüntüsü ve bir Azure depolama kaynağı bir SQL veritabanı kaynak gösterir.
   
    ![Azure uygulamanızı ölçeklendirmek için kaynak grubu sayfasına gidin](./media/web-sites-scale/ResourceGroup.png)
3. Bir SQL veritabanı kaynağı için tıklatın **ayarları** > **fiyatlandırma katmanı** fiyatlandırma katmanı için.
   
    ![SQL veritabanı arka ucu Azure uygulamanız için ölçeklendirin](./media/web-sites-scale/ScaleDatabase.png)
   
    Ayrıca açabilirsiniz [coğrafi çoğaltma](../sql-database/sql-database-geo-replication-overview.md) SQL veritabanı Örneğiniz için.
   
    Bir Azure depolama kaynağı için tıklatın **ayarları** > **yapılandırma** depolama seçeneklerinizi ölçeklendirmek için.
   
    ![Azure uygulamanız tarafından kullanılan Azure Storage hesabı ölçeklendirin](./media/web-sites-scale/ScaleStorage.png)

<a name="OtherFeatures"></a>
<a name="devfeatures"></a>

## <a name="compare-pricing-tiers"></a>Fiyatlandırma katmanlarını karşılaştırmak
Her fiyatlandırma katmanının VM boyutları gibi ayrıntılı bilgi için bkz: [App Service fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/web-sites/).
Hizmet sınırları, kotaları ve kısıtlamaları ve her katmanındaki desteklenen özellikler tablosu için bkz: [App Service sınırları](../azure-subscription-service-limits.md#app-service-limits).

> [!NOTE]
> Azure hesabı için kaydolmadan önce Azure App Service'i kullanmaya başlamak istiyorsanız, Git [App Service'i deneyin](https://azure.microsoft.com/try/app-service/) hemen oluşturabileceğiniz bir kısa süreli başlangıç web uygulaması App Service içinde. Kredi kartı gerekmez ve hiçbir taahhüt yoktur.
> 
> 

<a name="Next Steps"></a>

## <a name="next-steps"></a>Sonraki adımlar
* Fiyatlandırma, destek ve SLA hakkında daha fazla bilgi için aşağıdaki bağlantıları ziyaret edin:
  
    [Veri aktarımları fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/data-transfers/)
  
    [Microsoft Azure destek planları](https://azure.microsoft.com/support/plans/)
  
    [Hizmet Düzeyi Sözleşmeleri](https://azure.microsoft.com/support/legal/sla/)
  
    [SQL veritabanı fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/sql-database/)
  
    [Sanal makine ve bulut hizmeti boyutları Microsoft Azure][vmsizes]
  
* En iyi yöntemler, ölçeklenebilir ve esnek bir mimari oluşturmak gibi Azure App Service hakkında bilgi için bkz: [en iyi uygulamalar: Azure App Service Web Apps](http://blogs.msdn.com/b/windowsazure/archive/2014/02/10/best-practices-windows-azure-websites-waws.aspx).
* Uygulama hizmeti uygulamaları ölçeklendirme hakkında daha fazla videolar için aşağıdaki kaynaklara bakın:
  
  * [Azure ile Web siteleri - Stefan Schackow ölçeklendirmek ne zaman](https://azure.microsoft.com/resources/videos/azure-web-sites-free-vs-standard-scaling/)
  * [Azure Web siteleri, CPU ölçeklendirme otomatik veya zamanlanmış - Stefan Schackow](https://azure.microsoft.com/resources/videos/auto-scaling-azure-web-sites/)
  * [-Stefan Schackow ile nasıl Azure Web siteleri ölçek](https://azure.microsoft.com/resources/videos/how-azure-web-sites-scale/)

<!-- LINKS -->
[vmsizes]:/pricing/details/app-service/
[SQLaccountsbilling]:http://go.microsoft.com/fwlink/?LinkId=234930
[azuresubscriptions]:http://go.microsoft.com/fwlink/?LinkID=235288
[portal]: https://portal.azure.com/

<!-- IMAGES -->
[ChooseWHP]: ./media/web-sites-scale/scale1ChooseWHP.png
[ResourceGroup]: ./media/web-sites-scale/scale10ResourceGroup.png
[ScaleDatabase]: ./media/web-sites-scale/scale11SQLScale.png
[GeoReplication]: ./media/web-sites-scale/scale12SQLGeoReplication.png
