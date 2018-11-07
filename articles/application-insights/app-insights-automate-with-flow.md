---
title: Azure Application Insights işlemleri Microsoft Flow ile otomatikleştirme
description: Application Insights Bağlayıcısı'nı kullanarak tekrarlanabilir süreçlerini hızlıca otomatikleştirmek için Microsoft Flow nasıl kullanabileceğinizi öğrenin.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 06/25/2017
ms.author: mbullwin
ms.openlocfilehash: 65909e13c75ae4d2577ea29f562b841a1eb20477
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51256434"
---
# <a name="automate-azure-application-insights-processes-with-the-connector-for-microsoft-flow"></a>Microsoft Flow için Azure Application Insights işlemleri Bağlayıcısı ile otomatik hale getirin

Hizmetinizi düzgün çalışıp çalışmadığını denetlemek için telemetri verilerini sürekli olarak çalışan aynı sorgu kendiniz bulurum? Eğilimleri ve anormallikleri bulmak için bu sorguları otomatikleştirme ve ardından etrafında kendi akışlarınızı oluşturmak istiyorsunuz? Microsoft Flow için Azure Application Insights Bağlayıcısı (Önizleme) doğru araç bu amaçlar için kullanılır.

Bu tümleştirme sayesinde tek bir satır kod yazmadan artık çok sayıda işlemi otomatikleştirebilirsiniz. Bir Application Insights eylemini kullanarak akış oluşturduktan sonra akış Application Insights Analytics sorgunuzun otomatik olarak çalıştırır. 

Ek Eylemler ekleyebilirsiniz. Microsoft Flow eylemleri yüzlerce kullanılabilir hale getirir. Örneğin, Microsoft Flow otomatik olarak bir e-posta bildirimi gönderin ya da Azure DevOps bir hata oluşturmak için kullanabilirsiniz. Çok birini de kullanabilirsiniz [şablonları](https://ms.flow.microsoft.com/en-us/connectors/shared_applicationinsights/?slug=azure-application-insights) , bağlayıcı için Microsoft Flow için kullanılabilir. Bu şablonlar bir akış oluşturma işlemi hızlandırır. 

<!--The Application Insights connector also works with [Azure Power Apps](https://powerapps.microsoft.com/en-us/) and [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/?v=17.23h). --> 

## <a name="create-a-flow-for-application-insights"></a>Application Insights için akış oluşturma

Bu öğreticide, verileri bir web uygulaması için Grup öznitelikleri için Analytics otomatik küme algoritması kullanan bir akış oluşturmak öğreneceksiniz. Akış, e-postayla nasıl Microsoft Flow ve Application Insights Analytics birlikte kullanabileceğiniz tek bir örnek sonuçları otomatik olarak gönderir. 

### <a name="step-1-create-a-flow"></a>1. adım: bir akış oluşturma
1. Oturum [Microsoft Flow](https://flow.microsoft.com)ve ardından **Akışlarım**.
1. Tıklayın **sıfırdan bir akış oluşturun**.

### <a name="step-2-create-a-trigger-for-your-flow"></a>2. adım: akışınız için bir Tetikleyici oluşturma
1. Seçin **zamanlama**ve ardından **zamanlama - yinelenme**.
1. İçinde **sıklığı** kutusunda **gün**hem de **aralığı** kutusuna **1**.

    ![Microsoft Flow tetikleyici iletişim kutusu](./media/app-insights-automate-with-flow/flow1.png)


### <a name="step-3-add-an-application-insights-action"></a>3. adım: Application Insights Eylem Ekle
1. Tıklayın **yeni adım**ve ardından **Eylem Ekle**.
1. Arama **Azure Application Insights**.
1. Tıklayın **Azure Application Insights - analiz görselleştirme sorgu Önizleme**.

    ![Analiz sorgusu penceresi](./media/app-insights-automate-with-flow/flow2.png)

### <a name="step-4-connect-to-an-application-insights-resource"></a>4. adım: bir Application Insights kaynağına bağlanma

Bu adımı tamamlamak için kaynak için bir uygulama kimliği ve API anahtarı gerekir. Azure portalından, aşağıdaki diyagramda gösterildiği gibi geri alabilir:

![Azure portalında uygulama kimliği](./media/app-insights-automate-with-flow/appid.png) 

- Uygulama kimliği ve API anahtarı ile birlikte bağlantınız için bir ad sağlayın.

    ![Microsoft Flow bağlantı penceresi](./media/app-insights-automate-with-flow/flow3.png)

### <a name="step-5-specify-the-analytics-query-and-chart-type"></a>5. adım: Analytics sorgu ve grafik türünü belirtin
Bu örnekte sorgu, son gün içinde başarısız olan istekleri seçer ve bunları işleminin bir parçası olarak oluşan özel durumları ile ilişkilendirir. Analytics bunları operation_ıd tanımlayıcısına göre ilişkilendirir. Sorgu sonuçları autocluster algoritması kullanılarak ardından ayırır. 

Kendi sorgularınızı oluşturduğunuzda, akışınıza eklemeden önce bunlar düzgün Analytics'te çalıştığını doğrulayın.

- Aşağıdaki Analytics sorgusu ekleyin ve ardından HTML tablosu grafik türü seçin. 

    ```
    requests
    | where timestamp > ago(1d)
    | where success == "False"
    | project name, operation_Id
    | join ( exceptions
        | project problemId, outerMessage, operation_Id
    ) on operation_Id
    | evaluate autocluster()
    ```
    
    ![Analytics sorgu Yapılandırması penceresi](./media/app-insights-automate-with-flow/flow4.png)

### <a name="step-6-configure-the-flow-to-send-email"></a>6. adım: e-posta gönderme akışı Yapılandır

1. Tıklayın **yeni adım**ve ardından **Eylem Ekle**.
1. Arama **Office 365 Outlook**.
1. Tıklayın **Office 365 Outlook - e-posta Gönder**.

    ![Office 365 Outlook seçim penceresi](./media/app-insights-automate-with-flow/flow2b.png)

1. İçinde **bir e-posta** penceresinde aşağıdakileri yapın:

   a. Alıcı e-posta adresini yazın.

   b. E-posta için bir konu yazın.

   c. Herhangi bir yeri tıklatın **gövdesi** kutusuna ve ardından, sağ tarafta açılan dinamik içerik menüsünde **gövdesi**.

   d. Tıklayın **Gelişmiş Seçenekleri Göster**.

    ![Office 365 Outlook yapılandırma](./media/app-insights-automate-with-flow/flow5.png)

1. Dinamik içerik menüsünde, aşağıdakileri yapın:

    a. Seçin **ek adı**.

    b. Seçin **ek içeriği**.
    
    c. İçinde **HTML'dir** kutusunda **Evet**.

    ![Office 365 e-posta Yapılandırması penceresi](./media/app-insights-automate-with-flow/flow7.png)

### <a name="step-7-save-and-test-your-flow"></a>7. adım: Kaydedin ve akışınızı test edin
- İçinde **Akış adı** kutusuna akışınız için bir ad ekleyin ve ardından **akış oluşturma**.

    ![Akış oluşturma penceresi](./media/app-insights-automate-with-flow/flow8.png)

Tetikleyici spustit tuto akci bekleyebilir veya hemen göre akış çalıştırma [tetikleyici talep üzerine çalıştırmaya](https://flow.microsoft.com/blog/run-now-and-six-more-services/).

Akış çalıştırıldığında, e-posta listede belirttiğiniz alıcılara aşağıdakine benzer bir e-posta iletisi alırsınız:

![Örnek e-posta](./media/app-insights-automate-with-flow/flow9.png)


## <a name="next-steps"></a>Sonraki adımlar

- Oluşturma hakkında daha fazla bilgi edinin [analiz sorguları](../log-analytics/query-language/get-started-queries.md).
- Daha fazla bilgi edinin [Microsoft Flow](https://ms.flow.microsoft.com).



<!--Link references-->





