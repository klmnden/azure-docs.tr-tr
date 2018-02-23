---
title: "Logic Apps kullanarak Azure Application Insights işlemlerini otomatik hale."
description: "Nasıl hızlı bir şekilde yinelenebilir işlemler mantığı uygulamanıza Application Insights Bağlayıcısı'nı ekleyerek otomatikleştirebilirsiniz öğrenin."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: mbullwin
ms.openlocfilehash: e17d8076a00cab2cf608fe1a690e4a780a69d56f
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="automate-application-insights-processes-by-using-logic-apps"></a>Logic Apps kullanarak Application Insights işlemlerini otomatik hale

Kendiniz hizmetinizi düzgün çalışıp çalışmadığını denetlemek için telemetri verileri sürekli olarak çalışan aynı sorgu bulurum? Eğilimler ve daha fazla bilgi bulmak için bu sorguları otomatik hale getirme ve ardından etrafında kendi iş akışları oluşturmak istiyorsunuz? Azure Application Insights (Önizleme) Logic Apps için doğru aracı bu amaçla Bağlayıcıdır.

İle tümleştirme, tek satırlık bir kod yazmak zorunda kalmadan çeşitli işlemleri otomatik hale getirebilirsiniz. Hızlı bir şekilde tüm Application Insights işlemini otomatikleştirmek için Application Insights Bağlayıcısı ile bir mantıksal uygulama oluşturabilirsiniz. 

Ek Eylemler ekleyebilirsiniz. Azure App Service Logic Apps özelliğidir Eylemler yüzlerce kullanılabilir hale getirir. Örneğin, bir mantıksal uygulama kullanarak, yapabilir otomatik olarak bir e-posta bildirim göndermek veya bir hata Visual Studio Team Services içinde oluşturabilirsiniz. Kullanılabilir çok birini de kullanabilirsiniz [şablonları](https://docs.microsoft.com/azure/logic-apps/logic-apps-use-logic-app-templates) mantıksal uygulamanızı oluşturma işlemi hızlandırmak yardımcı olacak. 

## <a name="create-a-logic-app-for-application-insights"></a>Application Insights için bir mantıksal uygulama oluşturma

Bu öğreticide, verileri bir web uygulaması için Grup öznitelikleri Analytics autocluster algoritmasını kullanan bir mantıksal uygulama oluşturma öğrenin. Akış sonuçları e-posta ile tek bir örneği nasıl uygulama Öngörüler analizi ve Logic Apps birlikte kullanabileceğiniz otomatik olarak gönderir. 

### <a name="step-1-create-a-logic-app"></a>1. adım: bir mantıksal uygulama oluşturma
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Tıklatın **kaynak oluşturma**seçin **Web + mobil**ve ardından **mantıksal uygulama**.

    ![Yeni mantıksal uygulama penceresi](./media/automate-with-logic-apps/logicapp1.png)

### <a name="step-2-create-a-trigger-for-your-logic-app"></a>2. adım: mantıksal uygulamanız için bir Tetikleyici oluşturma
1. İçinde **mantığı Uygulama Tasarımcısı** penceresi altında **Başlat sahip ortak bir tetikleyici**seçin **yineleme**.

    ![Mantıksal Uygulama Tasarımcısı penceresi](./media/automate-with-logic-apps/logicapp2.png)

2. İçinde **sıklığı** kutusunda **gün** , daha sonra **aralığı** kutusuna **1**.

    ![Mantıksal Uygulama Tasarımcısı "Recurrence" penceresi](./media/automate-with-logic-apps/step2b.png)

### <a name="step-3-add-an-application-insights-action"></a>3. adım: Application Insights Eylem Ekle
1. Tıklatın **yeni adım**ve ardından **Eylem Ekle**.

2. İçinde **bir eylem seçin** arama kutusuna **Azure Application Insights**.

3. Altında **Eylemler**, tıklatın **Azure Application Insights – görselleştirmek Analytics sorgu Önizleme**.

    ![Mantıksal Uygulama Tasarımcısı penceresinin "bir eylem seçin"](./media/automate-with-logic-apps/flow2.png)

### <a name="step-4-connect-to-an-application-insights-resource"></a>4. adım: bir Application Insights kaynağına bağlanma

Bu adımı tamamlamak için kaynak için bir uygulama kimliği ve bir API anahtarı gerekir. Bunları Azure portalından, aşağıdaki çizimde gösterildiği gibi alabilirsiniz:

![Azure portalında uygulama kimliği](./media/automate-with-logic-apps/appid.png) 

Bağlantınızı, uygulama kimliği ve API anahtarı için bir ad sağlayın.

![Mantıksal Uygulama Tasarımcısı akış bağlantısı penceresi](./media/automate-with-logic-apps/flow3.png)

### <a name="step-5-specify-the-analytics-query-and-chart-type"></a>5. adım: Analytics sorgu ve grafik türünü belirtin
Aşağıdaki örnekte, sorgu başarısız isteklerin son gün içinde seçer ve bunları işleminin bir parçası oluşan özel durumları ile karşılık gelen. Analytics operation_Id tanımlayıcısına göre başarısız isteklerin hatalarla ilintilidir. Sorgu sonuçları autocluster algoritması kullanılarak sonra kesim. 

Kendi sorguları oluşturduğunuzda, akışınızı eklemeden önce bunlar düzgün analizleri çalıştığını doğrulayın.

1. İçinde **sorgu** kutusunda, aşağıdaki Analytics sorgu ekleyin: 

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

2. İçinde **grafik türü** kutusunda **Html tablosu**.

    ![Analytics sorgu yapılandırma penceresi](./media/automate-with-logic-apps/flow4.png)

### <a name="step-6-configure-the-logic-app-to-send-email"></a>6. adım: e-posta göndermek için mantıksal uygulama yapılandırma

1. Tıklatın **yeni adım**ve ardından **Eylem Ekle**.

2. Arama kutusuna **Office 365 Outlook**.

3. Tıklatın **Office 365 Outlook – bir e-posta Gönder**.

    ![Office 365 Outlook seçimi](./media/automate-with-logic-apps/flow2b.png)

4. İçinde **bir e-posta Gönder** penceresinde aşağıdakileri yapın:

   a. Alıcı e-posta adresini yazın.

   b. E-posta için bir konu yazın.

   c. Herhangi bir yeri tıklatın **gövde** kutusuna ve ardından sağ tarafta açılan dinamik içerik menüsünde seçin **gövde**.

   d. Tıklatın **Gelişmiş Seçenekleri Göster**.

      ![Office 365 Outlook yapılandırma](./media/automate-with-logic-apps/flow5.png)

5. Dinamik içerik menüsünde, aşağıdakileri yapın:

    a. Seçin **ek adı**.

    b. Seçin **ek içerik**.
    
    c. İçinde **HTML'dir** kutusunda **Evet**.

      ![Office 365 e-posta yapılandırma ekranında](./media/automate-with-logic-apps/flow7.png)

### <a name="step-7-save-and-test-your-logic-app"></a>7. adım: Kaydedin ve mantıksal uygulamanızı test etme
* Tıklatın **kaydetmek** yaptığınız değişiklikleri kaydetmek için.

Tetikleyicinin mantığını uygulamayı çalıştırmak bekleyebilir veya seçerek mantıksal uygulamayı hemen çalıştırabilirsiniz **çalıştırmak**.

![Logic app oluşturma ekran](./media/automate-with-logic-apps/step7.png)

Mantıksal uygulamanızı çalıştığında, e-posta listesinde belirtilen alıcılara aşağıdakine benzer bir e-posta alırsınız:

![Mantıksal uygulama e-posta iletisi](./media/automate-with-logic-apps/flow9.png)

## <a name="next-steps"></a>Sonraki adımlar

- Oluşturma hakkında daha fazla bilgi [analitik sorguları](app-insights-analytics-using.md).
- Daha fazla bilgi edinmek [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps).



<!--Link references-->





