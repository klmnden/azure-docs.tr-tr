---
title: Microsoft Dynamics CRM ve Azure Application Insights | Microsoft Docs
description: Microsoft Dynamics CRM Application Insights kullanarak Online'dan telemetri alın. Kurulum, veri, Görselleştirme ve dışarı aktarma Başlama Kılavuzu.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 04c66338-687e-49e5-9975-be935f98f156
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 03/16/2018
ms.reviewer: mazhar
ms.author: mbullwin
ms.openlocfilehash: 6119f1116d255f7cd2a2bfc20e86eeca9e5dfe82
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60523107"
---
# <a name="walkthrough-enabling-telemetry-for-microsoft-dynamics-crm-online-using-application-insights"></a>Çözüm: Microsoft Dynamics CRM Online Application Insights ile Telemetri etkinleştirme
Bu makalede telemetri verilerinin alınacağı gösterilmektedir [Microsoft Dynamics CRM Online](https://www.dynamics.com/) kullanarak [Azure Application Insights](https://azure.microsoft.com/services/application-insights/). Veri ve veri görselleştirme yakalama uygulamanıza Application Insights betiğini ekleme tam işlemi gösterilecektir.

> [!NOTE]
> [Örnek çözüm Gözat](https://dynamicsandappinsights.codeplex.com/).
> 
> 

## <a name="add-application-insights-to-new-or-existing-crm-online-instance"></a>Yeni veya mevcut CRM Online örneğine Application Insights ekleyin
Uygulamanızı izlemek için bir Application Insights SDK'sını uygulamanıza ekleyin. SDK'sı telemetri gönderir [Application Insights portalında](https://portal.azure.com), bizim güçlü analiz ve tanılama araçlarını kullanın, veya reddedebileceğiniz verilerini depolamaya aktar.

### <a name="create-an-application-insights-resource-in-azure"></a>Azure'da Application Insights kaynağı oluşturma
1. Alma [Microsoft azure'da bir hesap](https://azure.com/pricing). 
2. Oturum [Azure portalında](https://portal.azure.com) ve yeni bir Application Insights kaynağı ekleyin. Verilerin işlenmesi ve görüntülenen budur.

    ![Tıklama +, geliştirici Hizmetleri, Application Insights.](./media/sample-mscrm/01.png)

    Uygulama türü olarak ASP.NET’i seçin.
3. Yönergelerini izleyin [uygulamanız için JavaScript SDK'sı betik alma](../../azure-monitor/app/javascript.md#set-up-application-insights-for-your-web-page), JavaScript kod parçacığını kopyalayın ve izleme anahtarı, Application Insights kaynağı için geçerli bir değer ile değiştirdiğinizden emin olun.

### <a name="create-a-javascript-web-resource-in-microsoft-dynamics-crm"></a>Microsoft Dynamics CRM JavaScript web kaynak oluştur
1. Oturum açma ve CRM Online örneği yönetici ayrıcalıklarıyla açın.
2. Açık Microsoft Dynamics CRM ayarları, özelleştirmeler, sistemi Özelleştir

    ![Microsoft Dynamics CRM ayarları](./media/sample-mscrm/00001.png)

    ![Ayarlar > özelleştirmeleri](./media/sample-mscrm/00002.png)

1. JavaScript kaynak oluşturun.

    ![Yeni bir Web kaynağı iletişim kutusu](./media/sample-mscrm/07.png)

    Seçim bir ad verin **betik (JScript)** ve metin düzenleyicisini açın.

    ![Metin Düzenleyicisi'ni açın](./media/sample-mscrm/00004.png)
2. Application Insights JavaScript SDK'ı izleme anahtarınızı önce yapılandırdığınız kodu kopyalayın. Kopyalarken, komut dosyası etiketlerini yoksaymak emin olun. Aşağıdaki ekran görüntüsüne bakın:

    ![Uygulamanızın izleme anahtarını ayarlayın](./media/sample-mscrm/000005.png)

    Kod, Application ınsights kaynağınızı tanımlayan izleme anahtarını içerir.
3. Kaydedin ve yayımlayın.

    ![Kaydet ve Yayımla](./media/sample-mscrm/00006.png)

### <a name="instrument-forms"></a>Gereç formlar
1. Microsoft CRM Online içinde hesabı formunu açın

    ![Hesabı formunu](./media/sample-mscrm/00007.png)
2. Formun özellikleri açın

    ![Form Özellikleri](./media/sample-mscrm/00008.png)
3. Oluşturduğunuz bir JavaScript web kaynağı ekleyin

    ![Menü ekleme](./media/sample-mscrm/13.png)

4. Kaydet ve formu özelleştirme yayımlayın.

## <a name="metrics-captured"></a>Yakalanan ölçümleri
Form için telemetri yakalamayı artık ayarlamış. Kullanıldığında, veriler Application Insights kaynağınıza gönderilir.

Gördüğünüz verilerin örnekleri aşağıda verilmiştir.

#### <a name="application-health"></a>Uygulama durumu
![Örnek sayfa yükleme süresi](./media/sample-mscrm/15.png)

![Sayfa görünümleri grafik örneği](./media/sample-mscrm/16.png)

Tarayıcı özel durumları:

![Tarayıcı özel durumları grafiği](./media/sample-mscrm/17.png)

Daha fazla ayrıntı almak için grafiği tıklatın:

![Özel durum listesine](./media/sample-mscrm/18.png)

#### <a name="usage"></a>Kullanım
![Kullanıcılar, oturumlar ve sayfa görüntülemeleri](./media/sample-mscrm/19.png)

![Oturum grafikleri](./media/sample-mscrm/20.png)

![Tarayıcı sürümleri](./media/sample-mscrm/21.png)

#### <a name="browsers"></a>Tarayıcılar
![Sayfa yükleme süresi dökümü](./media/sample-mscrm/22.png)

![Tarayıcı sürümüne göre oturum sayısı](./media/sample-mscrm/23.png)

#### <a name="geolocation"></a>Coğrafi Konum
![Ülkeye göre oturum sayısı](./media/sample-mscrm/24.png)

![Oturumlar ve kullanıcılar ülkeye göre](./media/sample-mscrm/25.png)

#### <a name="inside-page-view-request"></a>İçinde bir sayfa isteğini görüntüle
![Sayfa görünümü özeti](./media/sample-mscrm/26.png)

![Sayfa görünümü etkinliklerine üzerinde arama yapın](./media/sample-mscrm/27.png)

![Benzer sayfa görüntülemeler](./media/sample-mscrm/28.png)

![Sayfa görünümü özellikleri](./media/sample-mscrm/29.png)

![Oturum başına sayfa sayısı](./media/sample-mscrm/30.png)

## <a name="sample-code"></a>Örnek kod
[Örnek kod Gözat](https://dynamicsandappinsights.codeplex.com/).

## <a name="power-bi"></a>Power BI
Bunu daha da derin analizi yapabilirsiniz, [Microsoft Power BI için verileri dışarı aktarma](../../azure-monitor/app/export-power-bi.md ).

## <a name="sample-microsoft-dynamics-crm-solution"></a>Örnek Microsoft Dynamics CRM çözüm
[Microsoft Dynamics CRM'de uygulanan örnek çözüm işte](https://dynamicsandappinsights.codeplex.com/).

## <a name="learn-more"></a>Daha fazla bilgi edinin
* [Application Insights nedir?](../../azure-monitor/app/app-insights-overview.md)
* [Web sayfaları için Application Insights](../../azure-monitor/app/javascript.md)
* [Daha fazla örnekleri ve izlenecek yollar](../../azure-monitor/app/app-insights-overview.md)
