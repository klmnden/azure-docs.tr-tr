---
title: SCOM tümleştirmesiyle Application Insights | Microsoft Docs
description: SCOM kullanıcısıysanız, performansı izleyebilir ve Application Insights ile ilgili sorunları tanılayın. Kapsamlı panolar, uyarılar akıllı, güçlü tanılama araçları ve analiz sorguları.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 606e9d03-c0e6-4a77-80e8-61b75efacde0
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 08/20/2018
ms.author: mbullwin
ms.openlocfilehash: a08a67a78362325e29b1002b44f390a94e78888a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60784886"
---
# <a name="application-performance-monitoring-using-application-insights-for-scom"></a>SCOM için Application Insights kullanarak uygulama performansı izleme
Sunucularınızı yönetmek için System Center Operations Manager (SCOM) kullanırsanız, performans izleme ve Yardım performans sorunlarını tanılayın [Azure Application Insights](../../azure-monitor/app/asp-net.md). Application Insights, web uygulamanızın gelen istekler, REST ve SQL çağrıları, özel durumlar ve günlük izlemelerini giden izler. Bu panolar ölçüm grafikleri ve akıllı uyarılar yanı sıra güçlü tanılama araması ve analitik sorguları bu telemetrisi üzerinde sağlar. 

Application Insights izleme temel bir SCOM Yönetim paketini kullanarak geçiş yapabilirsiniz.

> [!IMPORTANT]
> Bu System Center Operations Manager Yönetim Paketi sunulmuştur **kullanım dışı**. Bu, en son Application Insights SDK'ları desteklemez ve artık önerilir.

## <a name="before-you-start"></a>Başlamadan önce
Varsayılır:

* Web sunucuları SCOM ile ilgili bilgi sahibi olduğunuz ve IIS yönetmek için SCOM 2012 R2 veya 2016 kullanın.
* Application Insights ile izlemek istediğiniz bir web uygulaması sunucularınıza zaten yüklediniz.
* Uygulama framework sürümü, .NET 4.5 veya sonrası.
* Bir abonelikte erişiminiz [Microsoft Azure](https://azure.com) ve oturum açarak [Azure portalında](https://portal.azure.com). Kuruluşunuz bir aboneliğe sahip ve Microsoft hesabınızı ekleyebilirsiniz.

(Geliştirme ekibi oluşturabilir [Application Insights SDK'sı](../../azure-monitor/app/asp-net.md) web uygulamasına. Bu derleme araçları, bunları özel telemetri yazma daha fazla esneklik sağlar. Ancak, önemli değildir: veya SDK'sı yerleşiktir belirtmeden burada açıklanan adımları takip edebilirsiniz.)

## <a name="one-time-install-application-insights-management-pack"></a>(Bir kez) Application Insights Yönetimi paketini yükleyin
Operations Manager çalıştırıldığı makinede:

1. Yönetim paketinin herhangi bir eski sürümü kaldırın:
   1. Operations Manager'da, yönetim, yönetim paketleri açın. 
   2. Eski sürümü silin.
2. İndirin ve Yönetim Paketi Kataloğu'ndan yükleyin.
3. Operations Manager'ı yeniden başlatın.

## <a name="create-a-management-pack"></a>Bir yönetim paketi oluşturma
1. Operations Manager'da açın **yazma**, **.NET Application Insights ile...**, **izleme Ekleme Sihirbazı'nı**, yeniden seçin **.NET uygulaması ile... Insights**.
   
    ![](./media/scom/020.png)
2. Yapılandırmadan sonra uygulamanızı adlandırın. (Bir kerede tek bir uygulama izleme gerekir.)
   
    ![](./media/scom/030.png)
3. Aynı sihirbaz sayfasında, yeni bir Yönetim Paketi oluşturabilir veya daha önce oluşturduğunuz Application Insights için bir paket seçin.
   
     (Application Insights [Yönetim Paketi](https://technet.microsoft.com/library/cc974491.aspx) kendisinden oluşturduğunuz bir örneği bir şablonudur. Aynı örnek daha sonra yeniden kullanabilirsiniz.)

    ![Genel Özellikler sekmesinde, bir uygulama adı yazın. Yeni'ye tıklayın ve bir Yönetim Paketi için bir ad yazın. Tamam'a tıklayın ve ardından İleri'ye tıklayın.](./media/scom/040.png)

1. İzlemek istediğiniz bir uygulama seçin. Sunucularınızda yüklü uygulamalar arasında arama özelliğini arar.
   
    ![Hangi İzleyici sekmesi için Ekle'yi tıklatın, uygulama adının bir bölümünü yazın, Ara'yı tıklatın, uygulama ve ardından Ekle, Tamam'ı seçin.](./media/scom/050.png)
   
    Tüm sunucularda uygulamayı izlemek istemiyorsanız, isteğe bağlı izleme kapsamı alan sunucularınızın bir alt kümesi belirtmek için kullanılabilir.
2. Sonraki sihirbaz sayfasında ilk oturum açmak için Microsoft Azure için kimlik bilgilerinizi sağlamanız gerekir.
   
    Bu sayfada görüntülenir ve analiz edilmesi telemetri verilerini istediğiniz Application Insights kaynağını seçin. 
   
   * Uygulama geliştirme sırasında Application Insights için yapılandırıldıysa, mevcut kaynağı seçin.
   * Aksi takdirde, uygulama için adlandırılan yeni bir kaynak oluşturun. Aynı sistem bileşenlerinin diğer uygulamalarını varsa, bunları telemetriyi yönetilmesi daha kolay erişim sağlamak için aynı kaynak grubunda yerleştirin.
     
     Bu ayarları daha sonra değiştirebilirsiniz.
     
     ![Application Insights Ayarlar sekmesinde, 'oturum' tıklayın ve Azure için Microsoft hesabı kimlik bilgilerinizi sağlayın. Abonelik, kaynak grubu ve kaynak seçin.](./media/scom/060.png)
3. Sihirbazı tamamlayın.
   
    ![Oluştur’a tıklayın](./media/scom/070.png)

İzlemek istediğiniz her uygulama için bu yordamı yineleyin.

Ayarları daha sonra değiştirmeniz gerekiyorsa, İzleyici yazma penceresinden özelliklerini yeniden açın.

![Yazma, .NET uygulama performansı izleme ile Application Insights'ı seçin, İzleyicisi'ni seçin ve Özellikler'e tıklayın.](./media/scom/080.png)

## <a name="verify-monitoring"></a>İzleme doğrulayın
İzleyici her sunucuda yüklü uygulamanızı arar. Uygulamayı nerede bulduğu uygulamayı izlemek için Application Insights Durum İzleyicisi yapılandırır. Gerekirse, sunucuya ilk Durum İzleyicisi'ni yükler.

Uygulamanın hangi örneklerinin bulduğu doğrulayabilirsiniz:

![İzleme, Application ınsights'ı açın](./media/scom/100.png)

## <a name="view-telemetry-in-application-insights"></a>Application ınsights telemetrisini görüntüleme
İçinde [Azure portalında](https://portal.azure.com), uygulamanız için kaynak göz atın. [Telemetri gösteren grafikleri görmek](../../azure-monitor/app/app-insights-dashboards.md) uygulamanızdan. (Ana sayfasında göze henüz gösterilen taşınmadığından, Canlı ölçümleri Stream'ı tıklatın.)

## <a name="next-steps"></a>Sonraki adımlar
* [Bir Pano Ayarla](../../azure-monitor/app/app-insights-dashboards.md) bu ve diğer uygulamaları izleme en önemli grafikleri bir araya getirmek için.
* [Ölçümler hakkında bilgi edinin](../../azure-monitor/app/metrics-explorer.md)
* [Uyarılar ayarlayın](../../azure-monitor/app/alerts.md)
* [Performans sorunlarını tanılama](../../azure-monitor/app/detect-triage-diagnose.md)
* [Güçlü analiz sorguları](../../azure-monitor/app/analytics.md)
* [Kullanılabilirlik web testleri](../../azure-monitor/app/monitor-web-app-availability.md)

