---
title: Azure Application Insights verilerle özel raporları otomatikleştirme
description: Azure Application Insights verilerle özel günlük/haftalık/aylık raporları otomatikleştirme
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/25/2018
ms.reviewer: sdash
ms.author: mbullwin
ms.openlocfilehash: f57d80adc7c77f2d874d13a68214cd638a2ac2a0
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65957294"
---
# <a name="automate-custom-reports-with-azure-application-insights-data"></a>Azure Application Insights verilerle özel raporları otomatikleştirme

Dönemsel raporlar, kendi iş kritik hizmetleri nasıl yaptığını haberdar bir ekibi yardımcı olur. Geliştiriciler, DevOps/SRE takımlar ve yöneticilerinin portalda oturum herkese gerek kalmadan ınsights güvenilir bir şekilde teslim otomatik raporları ile birlikte verimli olmaya başlayabilirsiniz. Bu raporları ayrıca aşamalı bir artış belirlemeye yardımcı olabilecek gecikme süreleri, uyarı kuralları herhangi tetiklemeyebilir yük veya hata oranları.

Her Kurumsal gibi kendi özel raporlama gereksinimlerine sahiptir: 

* Ölçüm veya özel ölçümleri raporundaki toplamalarının belirli yüzdebirlik.
* Farklı Hedef Kitleleri için farklı raporlar için günlük, haftalık ve aylık toplamalarını veri var.
* Bölge veya ortam gibi özel öznitelikler segmentasyon. 
* Bunlar farklı aboneliklere veya kaynak grupları vb. olsa bile bazı AI kaynaklar tek bir raporda gruplandırın.
* Seçmeli dinleyicilere gönderilen önemli ölçümleri içeren ayrı raporlar.
* Portal kaynaklara erişimi olmayabilir hissedarlar raporlar.

> [!NOTE] 
> Haftalık Application Insights Özet e-posta, herhangi bir özelleştirme izin vermedi ve aşağıda listelenen özel seçenekleri yerine durdurulacaktır. 11 Haziran 2018 tarihinde son Haftalık Özet e-postası gönderilir. Benzer özel raporları (aşağıda önerilen sorgu kullanın) almak için aşağıdaki seçeneklerden birini yapılandırın.

## <a name="to-automate-custom-report-emails"></a>Özel rapor e-postaları otomatik hale getirmek için

Yapabilecekleriniz [programlı olarak Application Insights sorgu](https://dev.applicationinsights.io/) bir zamanlamaya göre özel raporlar oluşturmak için veri. Aşağıdaki seçenekler, hızlıca çalışmaya başlamanıza yardımcı olabilir:

* [Raporları, Microsoft Flow ile otomatikleştirme](automate-with-flow.md)
* [Logic Apps ile raporları otomatikleştirme](automate-with-logic-apps.md)
* "Application Insights Zamanlanmış Özet" kullanmak [Azure işlevi](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function) izleme senaryosunda şablonu. Bu işlev, SendGrid e-posta sunmak için kullanır. 

    ![Azure işlev şablonu](./media/automate-custom-reports/azure-function-template.png)

## <a name="sample-query-for-a-weekly-digest-email"></a>Haftalık Özet e-posta için örnek sorgu
Aşağıdaki sorgu, birden fazla veri kümesi için bir Haftalık Özet e-posta gibi rapor arasında birleştirme gösterir. Gereken şekilde özelleştirin ve haftalık rapor otomatik hale getirmek için yukarıda listelenen seçeneklerden birini kullanın.   

```AIQL
let period=7d;
requests
| where timestamp > ago(period)
| summarize Row = 1, TotalRequests = sum(itemCount), FailedRequests = sum(toint(success == 'False')),
    RequestsDuration = iff(isnan(avg(duration)), '------', tostring(toint(avg(duration) * 100) / 100.0))
| join (
dependencies
| where timestamp > ago(period)
| summarize Row = 1, TotalDependencies = sum(itemCount), FailedDependencies = sum(success == 'False'),
    DependenciesDuration = iff(isnan(avg(duration)), '------', tostring(toint(avg(duration) * 100) / 100.0))
) on Row | join (
pageViews
| where timestamp > ago(period)
| summarize Row = 1, TotalViews = sum(itemCount)
) on Row | join (
exceptions
| where timestamp > ago(period)
| summarize Row = 1, TotalExceptions = sum(itemCount)
) on Row | join (
availabilityResults
| where timestamp > ago(period)
| summarize Row = 1, OverallAvailability = iff(isnan(avg(toint(success))), '------', tostring(toint(avg(toint(success)) * 10000) / 100.0)),
    AvailabilityDuration = iff(isnan(avg(duration)), '------', tostring(toint(avg(duration) * 100) / 100.0))
) on Row
| project TotalRequests, FailedRequests, RequestsDuration, TotalDependencies, FailedDependencies, DependenciesDuration, TotalViews, TotalExceptions, OverallAvailability, AvailabilityDuration
```

## <a name="application-insights-scheduled-digest-report"></a>Application Insights Zamanlanmış Özet rapor

1. Azure portalından seçin **kaynak Oluştur** > **işlem** > **işlev uygulaması**.

   ![Bir Azure kaynak işlev uygulaması ekran oluşturma](./media/automate-custom-reports/function-app-01.png)

2. Seçin ve uygulama için uygun bilgileri girmeniz _Oluştur_. (Application Insights _üzerinde_ yalnızca yeni işlev uygulamanızı Application Insights ile izlemek istiyorsanız gereklidir)

   ![Bir Azure kaynak işlev uygulaması ayarları ekran oluşturma](./media/automate-custom-reports/function-app-02.png)

3. Yeni işlev uygulamanızı dağıtım tamamlandıktan sonra seçin **kaynağa Git**.

4. Seçin **yeni işlev**.

   ![Yeni işlev ekran oluşturma](./media/automate-custom-reports/function-app-03.png)

5. Seçin  **_Application Insights Zamanlanmış Özet şablon_**.

     > [!NOTE]
     > Varsayılan olarak, çalışma zamanı sürümü ile oluşturulan işlev uygulamaları 2.x. Yapmanız gerekenler [hedef Azure işlevleri çalışma zamanı sürümü](https://docs.microsoft.com/azure/azure-functions/set-runtime-version) Application ınsights'ı kullanmak için bir 1.x Zamanlanmış Özet şablonu.

   ![Yeni işlev Application Insights şablonu ekran görüntüsü](./media/automate-custom-reports/function-app-04.png)

6. Rapor ve seçin için uygun alıcı e-mailovou adresu **Oluştur**.

   ![İşlev ayarları ekran görüntüsü](./media/automate-custom-reports/function-app-05.png)

7. Seçin, **işlev uygulaması** > **Platform özellikleri** > **uygulama ayarları**.

    ![Azure işlev uygulaması ayarları ekran görüntüsü](./media/automate-custom-reports/function-app-07.png)

8. Uygun karşılık gelen değerlerle üç yeni uygulama ayarları oluşturma ``AI_APP_ID``, ``AI_APP_KEY``, ve ``SendGridAPI``. **Kaydet**’i seçin.

     ![İşlev tümleştirme arabirimi ekran görüntüsü](./media/automate-custom-reports/function-app-08.png)
    
    (AI_ değerleri raporlamak istediğiniz Application Insights kaynağı için API erişimi altında bulunabilir. Bir Application Insights API anahtarı yoksa seçeneği yoktur **API anahtarı oluştur**.)
    
   * AI_APP_ID uygulama kimliği =
   * AI_APP_KEY API anahtarı =
   * SendGridAPI SendGrid API anahtarı =

     > [!NOTE]
     > SendGrid hesabı yoksa bir tane oluşturabilirsiniz. Azure işlevleri için SendGrid belgelere [burada](https://docs.microsoft.com/azure/azure-functions/functions-bindings-sendgrid). Yalnızca SendGrid Kurulum ve bu makalenin sonunda sağlanan bir API anahtarı oluşturmak en az bir açıklama istiyorsanız. 

9. Seçin **tümleştir** altında çıkışları tıklatın **SendGrid ($return)**.

     ![Çıkış ekran görüntüsü](./media/automate-custom-reports/function-app-09.png)

10. Altında **SendGridAPI anahtarı uygulama ayarı**, seçmek için yeni oluşturulan uygulama ayarı **SendGridAPI**.

     ![İşlev uygulamasının ekran görüntüsü çalıştırma](./media/automate-custom-reports/function-app-010.png)

11. Çalıştırın ve işlev uygulamanızı test edin.

     ![Test ekran görüntüsü](./media/automate-custom-reports/function-app-11.png)

12. Gönderilen ve alınan iletinin başarılı olduğunu onaylamak için e-postanızı kontrol edin.

     ![E-posta konu satırını ekran görüntüsü](./media/automate-custom-reports/function-app-12.png)

## <a name="sendgrid-with-azure"></a>Azure ile SendGrid

Bu adımlar, yalnızca yapılandırılmış SendGrid hesabı yoksa, geçerlidir.

1. Azure portal seçin **kaynak Oluştur** arama **SendGrid e-posta teslimi** > tıklatın **Oluştur** > SendGrid belirli yönergelerinizi oluşturmak doldurun. 

     ![SendGrid kaynak ekran oluşturma](./media/automate-custom-reports/function-app-13.png)

2. SendGrid hesapları altında oluşturulan seçin **Yönet**.

     ![Ayarları API anahtarı ekran görüntüsü](./media/automate-custom-reports/function-app-14.png)

3. Bu, SendGrid site başlatılır. Seçin **ayarları** > **API anahtarları**.

     ![Oluşturun ve API anahtarı uygulama ekran görüntüleyin](./media/automate-custom-reports/function-app-15.png)

4. API anahtarı oluşturma > seçin **oluştur & görünümü** (Lütfen hangi izin düzeyini API anahtarınız için uygun olduğunu belirlemek için SendGrid belgeleri kısıtlı erişim gözden geçirin. Tam erişim burada yalnızca örnek amaçlıdır seçilir.)

   ![Tam erişim ekran görüntüsü](./media/automate-custom-reports/function-app-16.png)

5. Tüm anahtarı kopyalayın, bu değer, işlev uygulaması ayarları değeri olarak SendGridAPI için gerekenler

   ![API anahtarı ekran görüntüsünü Kopyala](./media/automate-custom-reports/function-app-17.png)

## <a name="next-steps"></a>Sonraki adımlar

* Oluşturma hakkında daha fazla bilgi edinin [analiz sorguları](../../azure-monitor/log-query/get-started-queries.md).
* Daha fazla bilgi edinin [programlı olarak Application Insights verilerini sorgulama](https://dev.applicationinsights.io/)
* [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps) hakkında daha fazla bilgi edinin.
* Daha fazla bilgi edinin [Microsoft Flow](https://ms.flow.microsoft.com).
