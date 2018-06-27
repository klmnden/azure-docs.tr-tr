---
title: Azure Application Insights verilerle özel raporlar otomatik hale getirme
description: Azure Application Insights verilerle özel haftalık/günlük/aylık raporlar otomatik hale getirme
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 06/25/2018
ms.reviewer: sdash
ms.author: mbullwin
ms.openlocfilehash: c8cff54c67ab2c9c3d09f9261617b6312cc4434a
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37025953"
---
# <a name="automate-custom-reports-with-azure-application-insights-data"></a>Azure Application Insights verilerle özel raporlar otomatik hale getirme

Dönemsel raporları kendi iş kritik hizmetleri nasıl gittiğini haberdar takım tutmak yardımcı olur. Geliştiriciler, DevOps/SRE ekipleri ve yöneticilerinin portalda oturum herkese gerek kalmadan Öngörüler güvenilir bir şekilde teslim otomatik raporlarla üretken olabilirler. Bu raporlar da aşamalı artar belirlemenize yardımcı olabilecek gecikmeleri içinde herhangi bir tetikleyebilir değil yük veya hata oranları uyarı kuralları.

Her Kuruluş kendi benzersiz raporlama gereksinimlerini gibi sahiptir: 

* Belirli yüzdebirlik toplamalar ölçümleri ya da bir rapordaki özel ölçümleri.
* Günlük, haftalık ve aylık toplamalarını veri için farklı raporları için farklı İzleyici vardır.
* Bölge veya ortam gibi özel öznitelikleri tarafından kesimleme. 
* Bunlar farklı abonelik veya kaynak grupları vb. olabilir olsa bile bazı AI kaynakları tek bir rapor içinde gruplandırın.
* Seçmeli İzleyici gönderilen hassas ölçümleri içeren ayrı raporlar.
* Portal kaynaklarına erişimi olmayabilir Paydaşlar raporlar.

> [!NOTE] 
> Haftalık Application Insights Özet e-posta hiçbir özelleştirme izin verme ve aşağıda listelenen özel seçenekleri lehinde sona erecek. 11 Haziran 2018 son Haftalık Özet e-posta gönderilir. Benzer özel raporlar (aşağıda önerilen sorgu kullanın) almak için aşağıdaki seçeneklerden birini yapılandırın.

## <a name="to-automate-custom-report-emails"></a>Özel rapor e-postaları otomatik hale getirmek için

Yapabilecekleriniz [programlı olarak Application Insights sorgu](https://dev.applicationinsights.io/) bir zamanlamaya göre özel raporlar oluşturmak için veri. Aşağıdaki seçenekler hızla başlamanıza yardımcı olabilir:

* [Microsoft Flow raporlarla otomatik hale getirme](app-insights-automate-with-flow.md)
* [Logic Apps ile raporlar otomatik hale getirme](automate-with-logic-apps.md)
* "Application Insights Zamanlanmış Özet" kullanmak [Azure işlevi](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function) izleme senaryoda şablonu. Bu işlev SendGrid e-posta teslim etmek için kullanır. 

    ![Azure işlevi şablonu](./media/automate-custom-reports/azure-function-template.png)

## <a name="sample-query-for-a-weekly-digest-email"></a>Haftalık Özet e-posta için örnek sorgu
Aşağıdaki sorgu, birden çok veri kümesi bir Haftalık Özet e-posta için rapor gibi üzerinden katılmak gösterir. Gerektiği gibi özelleştirin ve haftalık rapor otomatik hale getirmek için yukarıda listelenen seçeneklerden birini kullanın.   

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

## <a name="application-insights-scheduled-digest-report"></a>Uygulama Öngörüler Zamanlanmış Özet raporu

1. Azure portalından seçin **kaynak oluşturma** > **işlem** > **işlev uygulaması**.

   ![Bir Azure kaynak işlev uygulaması ekran oluşturma](./media/automate-custom-reports/function-app-01.png)

2. Seçin ve uygulama için uygun bilgileri girmeniz _oluşturma_. (Application Insights _üzerinde_ Application Insights ile yeni işlev uygulamanızı izlemek istiyorsanız gereklidir)

   ![Bir Azure kaynak işlevi uygulama ayarlarını ekran oluşturma](./media/automate-custom-reports/function-app-02.png)

3. Yeni işlev uygulamanız dağıtım tamamlandıktan sonra seçin **kaynağa gidin**.

4. Seçin **yeni işlev**.

   ![Yeni bir işlev ekran oluşturma](./media/automate-custom-reports/function-app-03.png)

5. Seçin  **_Application Insights Zamanlanmış Özet şablon_**.

   ![Yeni işlev Application Insights şablonunu ekran görüntüsü](./media/automate-custom-reports/function-app-04.png)

6. Seçin ve raporu için bir uygun alıcı e-posta adresi girin **oluşturma**.

   ![İşlev ayarları ekran görüntüsü](./media/automate-custom-reports/function-app-05.png)

7. Seçin, **işlev uygulaması** > **Platform özellikleri** > **uygulama ayarları**.

    ![Azure işlevi uygulama ayarları ekran görüntüsü](./media/automate-custom-reports/function-app-07.png)

8. Uygun karşılık gelen değerlerle üç yeni uygulama ayarları oluşturma ``AI_APP_ID``, ``AI_APP_KEY``, ve ``SendGridAPI``. **Kaydet**’i seçin.

     ![İşlev tümleştirme arabirimi ekran görüntüsü](./media/automate-custom-reports/function-app-08.png)
    
    (AI_ değerleri, raporlamak istediğiniz uygulama Insights kaynağı için API erişimini altında bulunabilir. Bir uygulama Öngörüler API anahtarı yoksa, bir seçenek yoktur **API anahtarı oluştur**.)
    
    * AI_APP_ID uygulama kimliği =
    * AI_APP_KEY API anahtarı =
    * SendGridAPI SendGrid API anahtarı =

    > [!NOTE]
    > SendGrid hesabınız yoksa bir tane oluşturabilirsiniz. Azure işlevleri için SendGrid'ın belgeleri [burada](https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-sendgrid). Yalnızca en az bir açıklama SendGrid Kurulum ve biri bu makalenin sonunda sağlanan bir API anahtarı oluşturmak nasıl istiyorsanız. 

9. Seçin **tümleştir** ve altında çıkışları tıklatın **SendGrid ($return)**.

     ![Çıkış ekran görüntüsü](./media/automate-custom-reports/function-app-09.png)

10. Altında **SendGridAPI anahtar uygulama ayarı**, yeni oluşturulan, uygulama ayarını seçmek **SendGridAPI**.

     ![Çalışma işlev uygulaması ekran görüntüsü](./media/automate-custom-reports/function-app-010.png)

11. Ve işlev uygulamanızı test çalıştırabilirsiniz.

     ![Test ekran görüntüsü](./media/automate-custom-reports/function-app-11.png)

12. Gönderilen ve alınan ileti başarıyla tamamlandığını doğrulamak için e-postanızı kontrol edin.

     ![E-posta konu satırı ekran görüntüsü](./media/automate-custom-reports/function-app-12.png)

## <a name="sendgrid-with-azure"></a>Azure ile SendGrid

Önceden yapılandırılmış bir SendGrid hesabınız yoksa yalnızca aşağıdaki adımları uygulayın.

1. Azure portal i seçin **kaynak oluşturma** arama **SendGrid e-posta teslimi** >'ı tıklatın **oluşturma** > ve SendGrid özel yönergeleri oluşturmak doldururken. 

     ![SendGrid kaynak ekran oluşturma](./media/automate-custom-reports/function-app-13.png)

2. SendGrid hesaplarla oluşturulduktan sonra seçin **Yönet**.

     ![Ayarları API anahtarı ekran görüntüsü](./media/automate-custom-reports/function-app-14.png)

3. Bu SendGrid'ın site başlatacak. Seçin **ayarları** > **API anahtarları**.

     ![Oluşturma ve API anahtarını uygulama ekran görüntüleme](./media/automate-custom-reports/function-app-15.png)

4. API anahtarı oluşturma > seçin **oluşturma & görünümü** (kısıtlı erişim SendGrid'ın belgeleri hangi düzeyde izinlere API anahtarınıza için uygun olduğunu belirlemek için lütfen inceleyin. Tam erişim burada yalnızca örnek amaçlıdır seçildiğinde kullanılabilir.)

   ![Tam erişim ekran görüntüsü](./media/automate-custom-reports/function-app-16.png)

5. Tüm anahtarı kopyalayın, bu değer, işlev uygulaması ayarlarınızı değeri olarak SendGridAPI için gerekenler:

   ![API anahtar ekran görüntüsü kopyalayın](./media/automate-custom-reports/function-app-17.png)

## <a name="next-steps"></a>Sonraki adımlar

* Oluşturma hakkında daha fazla bilgi [analitik sorguları](app-insights-analytics-using.md).
* Daha fazla bilgi edinmek [programlı olarak Application Insights veri sorgulama](https://dev.applicationinsights.io/)
* [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps) hakkında daha fazla bilgi edinin.
* Daha fazla bilgi edinmek [Microsoft Flow](https://ms.flow.microsoft.com).
