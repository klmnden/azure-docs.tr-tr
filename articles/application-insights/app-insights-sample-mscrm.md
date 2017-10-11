---
title: Microsoft Dynamics CRM ve Azure Application Insights | Microsoft Docs
description: "Telemetri Microsoft Dynamics CRM Application Insights kullanarak Online'dan alın. Kurulum, verileri, Görselleştirme ve dışarı aktarma alma kılavuz."
services: application-insights
documentationcenter: 
author: mazharmicrosoft
manager: carmonm
ms.assetid: 04c66338-687e-49e5-9975-be935f98f156
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/16/2017
ms.author: bwren
ms.openlocfilehash: a9593d5f198e05db80451a599428a296ed02e781
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="walkthrough-enabling-telemetry-for-microsoft-dynamics-crm-online-using-application-insights"></a>İzlenecek yol: Telemetri Microsoft Dynamics CRM Online Application Insights kullanarak etkinleştirme
Bu makalede, telemetri verilerini alma gösterilmektedir [Microsoft Dynamics CRM Online](https://www.dynamics.com/) kullanarak [Azure Application Insights](https://azure.microsoft.com/services/application-insights/). Biz uygulamanıza Application Insights komut dosyası ekleme tamamlandı işlemini veri ve veri görselleştirme yakalama yol.

> [!NOTE]
> [Örnek çözümü Gözat](https://dynamicsandappinsights.codeplex.com/).
> 
> 

## <a name="add-application-insights-to-new-or-existing-crm-online-instance"></a>Yeni veya mevcut CRM Online örneğine Application Insights ekleyin
Uygulamanızı izlemek üzere uygulamanıza Application Insights SDK ekleyin. SDK'sı telemetri gönderir [Application Insights portalındaki](https://portal.azure.com), burada bizim güçlü analizi ve tanılama araçları kullanabilir, veya verileri depolama alanına verin.

### <a name="create-an-application-insights-resource-in-azure"></a>Application Insights kaynağı oluşturma
1. Alma [Microsoft Azure hesap](http://azure.com/pricing). 
2. Oturum [Azure portal](https://portal.azure.com) ve yeni bir Application Insights kaynağı ekleyin. Verilerinizi nereye işlenen ve görüntülenen budur.
   
    ![Tıklatın +, geliştirici Hizmetleri, Application Insights.](./media/app-insights-sample-mscrm/01.png)
   
    Uygulama türü olarak ASP.NET’i seçin.
3. Başlarken sayfasını açın ve Aç "İzleyici ve istemci tarafı tanılamak".
   
    ![Kod parçacığı web sayfanıza eklemek için](./media/app-insights-sample-mscrm/03.png)

**Kod sayfası açık tutmak** başka bir tarayıcı penceresi sonraki adımda yaparken. Kod yakında gerekir. 

### <a name="create-a-javascript-web-resource-in-microsoft-dynamics-crm"></a>Microsoft Dynamics CRM JavaScript web kaynak oluşturma
1. Oturum açma ve CRM Online örneği yönetici ayrıcalıklarıyla açın.
2. Açık Microsoft Dynamics CRM ayarları, özelleştirmeleri, sistem özelleştirme
   
    ![Microsoft Dynamics CRM ayarları](./media/app-insights-sample-mscrm/04.png)
   
    ![Ayarlar > özelleştirmeleri](./media/app-insights-sample-mscrm/05.png)

    ![Sistem seçeneği özelleştirme](./media/app-insights-sample-mscrm/06.png)

1. Bir JavaScript kaynağı oluşturun.
   
    ![Yeni Web kaynağı iletişim kutusu](./media/app-insights-sample-mscrm/07.png)
   
    Select bir ad verin **komut dosyası (JScript)** ve metin düzenleyicisini açın.
   
    ![Metin Düzenleyicisi'ni açın](./media/app-insights-sample-mscrm/08.png)
2. Kodu Application Insights kopyalayın. Kopyalanırken komut dosyası etiketlerini yoksay emin olun. Ekran görüntüsünün altında bakın:
   
    ![İzleme anahtarı ayarlayın](./media/app-insights-sample-mscrm/09.png)
   
    Kodu uygulama ınsights kaynağınızı tanımlayan izleme anahtarını içerir.
3. Kaydedin ve yayımlayın.
   
    ![Kaydedin ve yayımlayın](./media/app-insights-sample-mscrm/10.png)

### <a name="instrument-forms"></a>Gereç formlar
1. Microsoft CRM Online içinde hesap formunu açın
   
    ![Hesap formu](./media/app-insights-sample-mscrm/11.png)
2. Form özelliklerini açın
   
    ![Form Özellikleri](./media/app-insights-sample-mscrm/12.png)
3. Oluşturduğunuz JavaScript web kaynağı ekleyin
   
    ![Menü ekleme](./media/app-insights-sample-mscrm/13.png)
   
    ![Web kaynağı ekleyin](./media/app-insights-sample-mscrm/14.png)
4. Kaydet ve formu özelleştirmelerinizi yayımlayın.

## <a name="metrics-captured"></a>Yakalanan ölçümleri
Şimdi form için telemetri yakalamayı oluşturdunuz. Kullanıldığında, verileri Application Insights kaynağınıza gönderilir.

Göreceğiniz veri örnekleri aşağıda verilmiştir.

#### <a name="application-health"></a>Uygulama durumu
![Örnek sayfa yükleme süresi](./media/app-insights-sample-mscrm/15.png)

![Örnek sayfa görünümleri grafik](./media/app-insights-sample-mscrm/16.png)

Tarayıcı özel durumlar:

![Tarayıcı özel durumlar grafik](./media/app-insights-sample-mscrm/17.png)

Daha fazla ayrıntı almak için grafiği tıklatın:

![Özel durumlar listesi](./media/app-insights-sample-mscrm/18.png)

#### <a name="usage"></a>Kullanım
![Kullanıcıları, oturumlar ve sayfa görünümleri](./media/app-insights-sample-mscrm/19.png)

![Sesion grafikleri](./media/app-insights-sample-mscrm/20.png)

![Tarayıcı sürümleri](./media/app-insights-sample-mscrm/21.png)

#### <a name="browsers"></a>Tarayıcılar
![Sayfa yükleme süresi dökümü](./media/app-insights-sample-mscrm/22.png)

![Tarayıcı sürümü göre oturum sayısı](./media/app-insights-sample-mscrm/23.png)

#### <a name="geolocation"></a>Coğrafi konum
![Ülkeye göre oturum sayısı](./media/app-insights-sample-mscrm/24.png)

![Oturumların ve kullanıcıların ülkeye göre](./media/app-insights-sample-mscrm/25.png)

#### <a name="inside-page-view-request"></a>İç sayfa görünümü isteği
![Sayfa görünümü özeti](./media/app-insights-sample-mscrm/26.png)

![Sayfa görünümü etkinliklerine arama](./media/app-insights-sample-mscrm/27.png)

![Benzer sayfa görünümleri](./media/app-insights-sample-mscrm/28.png)

![Sayfa görünümü özellikleri](./media/app-insights-sample-mscrm/29.png)

![Oturum başına sayfa](./media/app-insights-sample-mscrm/30.png)

## <a name="sample-code"></a>Örnek kod
[Örnek kod Gözat](https://dynamicsandappinsights.codeplex.com/).

## <a name="power-bi"></a>Power BI
Bunu bile daha derin çözümleme yapabilirsiniz, [Microsoft Power BI için verileri dışa](app-insights-export-power-bi.md).

## <a name="sample-microsoft-dynamics-crm-solution"></a>Örnek Microsoft Dynamics CRM çözümü
[Microsoft Dynamics CRM uygulanan örnek çözümü işte](https://dynamicsandappinsights.codeplex.com/).

## <a name="learn-more"></a>Daha fazla bilgi edinin
* [Application Insights nedir?](app-insights-overview.md)
* [Web sayfaları için Application Insights](app-insights-javascript.md)
* [Daha fazla örnekleri ve izlenecek yollar](app-insights-code-samples.md)
