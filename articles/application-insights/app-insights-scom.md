---
title: "Application Insights ile SCOM tümleştirme | Microsoft Docs"
description: "SCOM kullanıcı değilseniz, Application Insights ile sorunlarını tanılamak ve performansını izleyebilir. Kapsamlı panoları, akıllı uyarıları, güçlü tanılama araçları ve analiz sorguları."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: 606e9d03-c0e6-4a77-80e8-61b75efacde0
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 08/12/2016
ms.author: mbullwin
ms.openlocfilehash: 35ea37b751909e14e616a965462b832e4e51bae0
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="application-performance-monitoring-using-application-insights-for-scom"></a>SCOM için Application Insights kullanarak uygulama performansı izleme
Sunucularınızı yönetmek için System Center Operations Manager (SCOM) kullanıyorsanız, performansı izleyerek ve yardımıyla performans sorunlarını tanılamanıza [Azure Application Insights](app-insights-asp-net.md). Application Insights REST ve SQL çağrıları, özel durumlar ve günlük izlemelerini giden web uygulamanızın gelen istekleri izler. Panolar ölçüm grafikler ve akıllı uyarıları yanı sıra güçlü tanılama arama ve analitik sorguları ile bu telemetri sağlar. 

SCOM Yönetim Paketi kullanarak Application Insights izleme geçiş yapabilirsiniz.

## <a name="before-you-start"></a>Başlamadan önce
Biz varsayın:

* Web sunucuları ile SCOM tanıdık ve IIS yönetmek için SCOM 2012 R2 veya 2016 kullanın.
* Application Insights ile izlemek istediğiniz bir web uygulaması sunucularınıza zaten yüklediniz.
* Uygulama framework .NET 4.5 veya üzeri sürümüdür.
* Bir abonelikte erişimi [Microsoft Azure](https://azure.com) ve oturum açarak [Azure portal](https://portal.azure.com). Kuruluşunuz bir aboneliğe sahip ve Microsoft hesabınızı ekleyebilirsiniz.

(Geliştirme ekibi oluşturabilir [Application Insights SDK'sı](app-insights-asp-net.md) web uygulamada. Bu derleme araçları bunları özel telemetri yazma büyük esneklik sağlar. Ancak, önemli değildir: ile ya da SDK'sı yerleşiktir olmadan burada açıklanan adımları izleyebilirsiniz.)

## <a name="one-time-install-application-insights-management-pack"></a>(Bir saat) Application Insights Yönetimi paketini yükleyin
Operations Manager çalıştırdığı makinede:

1. Yönetim Paketi herhangi bir eski sürümü kaldırın:
   1. Operations Manager'da yönetim, yönetim paketleri açın. 
   2. Eski sürümünü silin.
2. Karşıdan yükleme ve Yönetim Paketi Kataloğu'ndan yükleyin.
3. Operations Manager'ı yeniden başlatın.

## <a name="create-a-management-pack"></a>Bir yönetim paketi oluşturun
1. Operations Manager'da açmak **yazma**, **.NET Application Insights ile...**, **izleme Ekleme Sihirbazı'nı**, yeniden seçin **.NET Application Insights ile...**.
   
    ![](./media/app-insights-scom/020.png)
2. Yapılandırmadan sonra uygulamanızı adlandırın. (Bir kerede bir uygulamayı izlemek vardır.)
   
    ![](./media/app-insights-scom/030.png)
3. Aynı sihirbaz sayfasında, yeni bir yönetim paketi oluşturun veya Application Insights için daha önce oluşturduğunuz bir paketi seçin.
   
     (Application Insights [Yönetim Paketi](https://technet.microsoft.com/library/cc974491.aspx) örneği oluşturduğunuz bir şablonu. Aynı örnek daha sonra yeniden kullanabilirsiniz.)

    ![Genel Özellikler sekmesinde, uygulama adını yazın. Yeni'yi tıklatın ve bir Yönetim Paketi için bir ad yazın. Tamam'ı tıklatın, sonra İleri'yi tıklatın.](./media/app-insights-scom/040.png)

1. İzlemek istediğiniz bir uygulama seçin. Sunucularınızda yüklü uygulamalar arasında arama özelliğini arar.
   
    ![İzlenecekler sekmesinde üzerinde Ekle'yi tıklatın, uygulama adı kısmını yazın, Ara'yı tıklatın, uygulama ve ardından Ekle, Tamam'ı seçin.](./media/app-insights-scom/050.png)
   
    Tüm sunucularda uygulamayı izlemek istemiyorsanız, isteğe bağlı bir izleme kapsamı alan, sunucularınızın bir alt belirtmek için kullanılabilir.
2. Sonraki sihirbaz sayfasında ilk Microsoft Azure'da oturum açmak için kimlik bilgilerinizi sağlamanız gerekir.
   
    Bu sayfada telemetri verileri analiz ve görüntülenmesini istediğiniz Application Insights kaynağı seçin. 
   
   * Uygulama geliştirme sırasında Application Insights için yapılandırıldıysa, var olan kaynağı seçin.
   * Aksi takdirde, uygulama için adlı yeni bir kaynak oluşturun. Aynı sistem bileşenlerinin başka uygulamalar varsa, bunları erişim telemetri yönetmeyi kolaylaştırmak için aynı kaynak grubunda yerleştirin.
     
     Bu ayarları daha sonra değiştirebilirsiniz.
     
     ![Application Insights Ayarlar sekmesinde, 'oturum aç' tıklayın ve Azure için Microsoft hesabı kimlik bilgilerinizi sağlayın. Ardından bir abonelik, kaynak grubu ve kaynak seçin.](./media/app-insights-scom/060.png)
3. Sihirbazı tamamlayın.
   
    ![Oluştur’a tıklayın](./media/app-insights-scom/070.png)

İzlemek istediğiniz her uygulama için bu yordamı yineleyin.

Ayarları daha sonra değiştirmeniz gerekirse, yeniden yazma penceresinden İzleyici özelliklerini açın.

![Yazma, .NET uygulama performansı izleme Application Insights ile seçin, sizin İzleyicisi'ni seçin ve Özellikler'i tıklatın.](./media/app-insights-scom/080.png)

## <a name="verify-monitoring"></a>İzleme doğrulayın
İzleyici her sunucuda yüklü uygulamanızı arar. Burada, app bulur uygulamayı izlemek için Application Insights Durum İzleyicisi yapılandırır. Gerekiyorsa, sunucuda ilk Durum İzleyicisi'ni yükler.

Uygulamayı hangi örneklerini bulduğu doğrulayabilirsiniz:

![İzleme, Application Insights açın](./media/app-insights-scom/100.png)

## <a name="view-telemetry-in-application-insights"></a>Application Insights telemetrisi görünümü
İçinde [Azure portal](https://portal.azure.com), uygulamanız için kaynak konumuna göz atın. [Telemetri gösteren grafikleri görmek](app-insights-dashboards.md) uygulamanızdan. (Ana sayfasında henüz gösterilen kurmadı, Canlı ölçümleri akış'ı tıklatın.)

## <a name="next-steps"></a>Sonraki adımlar
* [Bir Pano ayarlamak](app-insights-dashboards.md) bu ve diğer uygulamalar izleme en önemli grafikleri araya getirmek için.
* [Ölçümler hakkında bilgi edinin](app-insights-metrics-explorer.md)
* [Uyarıları ayarlayın](app-insights-alerts.md)
* [Performans sorunlarını tanılama](app-insights-detect-triage-diagnose.md)
* [Güçlü Analytics sorgular](app-insights-analytics.md)
* [Kullanılabilirlik web testleri](app-insights-monitor-web-app-availability.md)

