---
title: Microsoft Flow ile Azure Application Insights işlemlerini otomatik hale getirme
description: Hızlı bir şekilde yinelenebilir işlemler Application Insights Bağlayıcısı'nı kullanarak otomatik hale getirmek için Microsoft Flow nasıl kullanabileceğinizi öğrenin.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 06/25/2017
ms.author: mbullwin
ms.openlocfilehash: a1d2787626ed8fa71e3e4e9921ffb8a4680014cb
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34807790"
---
# <a name="automate-azure-application-insights-processes-with-the-connector-for-microsoft-flow"></a>İçin Microsoft Flow connector ile Azure Application Insights süreçleri otomatik hale getirme

Kendiniz hizmetinizi düzgün çalışıp çalışmadığını denetlemek için telemetri verileri sürekli olarak çalışan aynı sorgu bulurum? Eğilimler ve daha fazla bilgi bulmak için bu sorguları otomatik hale getirme ve ardından etrafında kendi iş akışları oluşturmak istiyorsunuz? Azure Application Insights (Önizleme) Microsoft Flow için doğru aracı bu amaçlar için Bağlayıcıdır.

İle tümleştirme, tek satırlık bir kod yazmak zorunda kalmadan artık çok sayıda süreçlerini otomatikleştirebilirsiniz. Application Insights eylemini kullanarak bir akış oluşturduktan sonra akış uygulama Öngörüler Analytics sorgunuzu otomatik olarak çalıştırılır. 

Ek Eylemler ekleyebilirsiniz. Microsoft Flow Eylemler yüzlerce kullanılabilir hale getirir. Örneğin, otomatik olarak bir e-posta bildirim göndermek veya Visual Studio Team Services içinde oluşturma için Microsoft Flow kullanabilirsiniz. Çok birini de kullanabilirsiniz [şablonları](https://ms.flow.microsoft.com/en-us/connectors/shared_applicationinsights/?slug=azure-application-insights) Microsoft Flow için bağlayıcı için kullanılabilir. Bu şablonları bir akış oluşturma işlemi hızlandırmak. 

<!--The Application Insights connector also works with [Azure Power Apps](https://powerapps.microsoft.com/en-us/) and [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/?v=17.23h). --> 

## <a name="create-a-flow-for-application-insights"></a>Bir akış için Application Insights oluşturma

Bu öğreticide, verileri bir web uygulaması için Grup öznitelikleri Analytics otomatik küme algoritmasını kullanan bir akışı oluşturmak öğreneceksiniz. Akış sonuçları e-posta ile tek bir örneği nasıl Microsoft Flow ve uygulama Öngörüler Analytics birlikte kullanabileceğiniz otomatik olarak gönderir. 

### <a name="step-1-create-a-flow"></a>1. adım: bir akışı oluşturma
1. Oturum [Microsoft Flow](http://flow.microsoft.com)ve ardından **My akar**.
2. Tıklatın **bir akışı boş iken oluşturmak**.

### <a name="step-2-create-a-trigger-for-your-flow"></a>2. adım: akışınız için bir Tetikleyici oluşturma
1. Seçin **zamanlama**ve ardından **çizelgesi - yinelenme**.
2. İçinde **sıklığı** kutusunda **gün**hem de **aralığı** kutusuna **1**.

    ![Microsoft Flow tetikleyici iletişim kutusu](./media/app-insights-automate-with-flow/flow1.png)


### <a name="step-3-add-an-application-insights-action"></a>3. adım: Application Insights Eylem Ekle
1. Tıklatın **yeni adım**ve ardından **Eylem Ekle**.
2. Arama **Azure Application Insights**.
3. Tıklatın **Azure Application Insights – görselleştirmek Analytics sorgu Önizleme**.

    ![Analytics sorgu penceresi](./media/app-insights-automate-with-flow/flow2.png)

### <a name="step-4-connect-to-an-application-insights-resource"></a>4. adım: bir Application Insights kaynağına bağlanma

Bu adımı tamamlamak için kaynak için bir uygulama kimliği ve bir API anahtarı gerekir. Bunları Azure portalından, aşağıdaki çizimde gösterildiği gibi alabilirsiniz:

![Azure portalında uygulama kimliği](./media/app-insights-automate-with-flow/appid.png) 

- Uygulama kimliği ve API anahtarı ile birlikte bağlantınız için bir ad sağlayın.

    ![Microsoft Flow bağlantı penceresi](./media/app-insights-automate-with-flow/flow3.png)

### <a name="step-5-specify-the-analytics-query-and-chart-type"></a>5. adım: Analytics sorgu ve grafik türünü belirtin
Bu örnek sorgu son gün içinde başarısız olan istekleri seçer ve bunları işleminin bir parçası oluşan özel durumları ile karşılık gelen. Analytics bunları operation_Id tanımlayıcısına göre hatalarla ilintilidir. Sorgu sonuçları autocluster algoritması kullanılarak sonra kesim. 

Kendi sorguları oluşturduğunuzda, akışınızı eklemeden önce bunlar düzgün analizleri çalıştığını doğrulayın.

- Aşağıdaki Analytics sorgu ekleyin ve ardından HTML tablo grafik türü seçin. 

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
    
    ![Analytics sorgu yapılandırma penceresi](./media/app-insights-automate-with-flow/flow4.png)

### <a name="step-6-configure-the-flow-to-send-email"></a>6. adım: e-posta göndermek için akışı Yapılandır

1. Tıklatın **yeni adım**ve ardından **Eylem Ekle**.
2. Arama **Office 365 Outlook**.
3. Tıklatın **Office 365 Outlook – bir e-posta Gönder**.

    ![Office 365 Outlook seçim penceresi](./media/app-insights-automate-with-flow/flow2b.png)

4. İçinde **bir e-posta Gönder** penceresinde aşağıdakileri yapın:

   a. Alıcı e-posta adresini yazın.

   b. E-posta için bir konu yazın.

   c. Herhangi bir yeri tıklatın **gövde** kutusuna ve ardından sağ tarafta açılan dinamik içerik menüsünde seçin **gövde**.

   d. Tıklatın **Gelişmiş Seçenekleri Göster**.

    ![Office 365 Outlook yapılandırma](./media/app-insights-automate-with-flow/flow5.png)

5. Dinamik içerik menüsünde, aşağıdakileri yapın:

    a. Seçin **ek adı**.

    b. Seçin **ek içerik**.
    
    c. İçinde **HTML'dir** kutusunda **Evet**.

    ![Office 365 e-posta yapılandırma penceresi](./media/app-insights-automate-with-flow/flow7.png)

### <a name="step-7-save-and-test-your-flow"></a>7. adım: Kaydetmek ve akışınız test
- İçinde **Akış adı** kutusuna akışınız için bir ad ekleyin ve ardından **akışı oluşturmak**.

    ![Akış oluşturma penceresi](./media/app-insights-automate-with-flow/flow8.png)

Bu eylem tetikleyici için bekleyebilir veya akış hemen göre çalıştırabilirsiniz [tetikleyici talep üzerine çalıştırmaya](https://flow.microsoft.com/blog/run-now-and-six-more-services/).

Akışı çalıştığında, e-posta listesinde belirtilen alıcılara aşağıdakine benzer bir e-posta iletisini alıyorsunuz:

![Örnek e-posta](./media/app-insights-automate-with-flow/flow9.png)


## <a name="next-steps"></a>Sonraki adımlar

- Oluşturma hakkında daha fazla bilgi [analitik sorguları](app-insights-analytics-using.md).
- Daha fazla bilgi edinmek [Microsoft Flow](https://ms.flow.microsoft.com).



<!--Link references-->





