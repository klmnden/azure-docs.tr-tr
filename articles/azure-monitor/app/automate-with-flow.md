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
ms.topic: conceptual
ms.date: 03/12/2019
ms.author: mbullwin
ms.openlocfilehash: 15299be83758c157bf3bc7d9fb27b50763b9148e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60903654"
---
# <a name="automate-azure-application-insights-processes-with-the-connector-for-microsoft-flow"></a>Microsoft Flow için Azure Application Insights işlemleri Bağlayıcısı ile otomatik hale getirin

Hizmetinizi düzgün çalışıp çalışmadığını denetlemek için telemetri verilerini sürekli olarak çalışan aynı sorgu kendiniz bulurum? Eğilimleri ve anormallikleri bulmak için bu sorguları otomatikleştirme ve ardından etrafında kendi akışlarınızı oluşturmak istiyorsunuz? Azure Application Insights Bağlayıcısı Microsoft Flow için doğru aracı bu amaçları içindir.

Bu tümleştirme sayesinde tek bir satır kod yazmadan artık çok sayıda işlemi otomatikleştirebilirsiniz. Bir Application Insights eylemini kullanarak akış oluşturduktan sonra akış Application Insights Analytics sorgunuzun otomatik olarak çalıştırır. 

Ek Eylemler ekleyebilirsiniz. Microsoft Flow eylemleri yüzlerce kullanılabilir hale getirir. Örneğin, Microsoft Flow otomatik olarak bir e-posta bildirimi gönderin ya da Azure DevOps bir hata oluşturmak için kullanabilirsiniz. Çok birini de kullanabilirsiniz [şablonları](https://ms.flow.microsoft.com/en-us/connectors/shared_applicationinsights/?slug=azure-application-insights) , bağlayıcı için Microsoft Flow için kullanılabilir. Bu şablonlar bir akış oluşturma işlemi hızlandırır. 

<!--The Application Insights connector also works with [Azure Power Apps](https://powerapps.microsoft.com/en-us/) and [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/?v=17.23h). --> 

## <a name="create-a-flow-for-application-insights"></a>Application Insights için akış oluşturma

Bu öğreticide, verileri bir web uygulaması için Grup öznitelikleri için Analytics otomatik küme algoritması kullanan bir akış oluşturmak öğreneceksiniz. Akış, e-postayla nasıl Microsoft Flow ve Application Insights Analytics birlikte kullanabileceğiniz tek bir örnek sonuçları otomatik olarak gönderir. 

### <a name="step-1-create-a-flow"></a>1\. adım: Akış oluşturun
1. Oturum [Microsoft Flow](https://flow.microsoft.com)ve ardından **Akışlarım**.
2. Tıklayın **yeni** ardından **boş akış Oluştur**.

    ![Sıfırdan yeni akış oluştur](./media/automate-with-flow/1createflow.png)

### <a name="step-2-create-a-trigger-for-your-flow"></a>2\. adım: Akışınız için bir Tetikleyici oluşturma
1. Sekmesini seçin yapı bileşeninde **zamanlama**ve ardından **zamanlama - yinelenme**.

    ![Yapı altında seçin zamanlamak](./media/automate-with-flow/2schedule.png)

1. İçinde **aralığı** kutusuna **1**hem de **sıklığı** kutusunda **gün**.
2. Tıklayın **yeni adım**

    ![Frequency ve interval değerleriyle girme ile zamanlama yinelenme değeri ayarlama](./media/automate-with-flow/3schedulerecurrence.png)


### <a name="step-3-add-an-application-insights-action"></a>3\. adım: Application Insights Eylem Ekle
1. Arama **Azure Application Insights**.
2. Tıklayın **Azure Application Insights - görselleştirme Analytics sorgusu**.
 
    ![Eylem seçin: Azure Application Insights Analytics görselleştirme sorgusu](./media/automate-with-flow/4visualize.png)

### <a name="step-4-connect-to-an-application-insights-resource"></a>4\. Adım: Bir Application Insights kaynağına bağlanma

Bu adımı tamamlamak için kaynak için bir uygulama kimliği ve API anahtarı gerekir. Azure portalından, aşağıdaki diyagramda gösterildiği gibi geri alabilir:

![Azure portalında uygulama kimliği](./media/automate-with-flow/5apiaccess.png)

![Azure portalında API anahtarı](./media/automate-with-flow/6apikey.png)

- Uygulama kimliği ve API anahtarı ile birlikte bağlantınız için bir ad sağlayın.

    ![Microsoft Flow bağlantı penceresi](./media/automate-with-flow/7connection.png)

### <a name="step-5-specify-the-analytics-query-and-chart-type"></a>5\. Adım: Analytics sorgu ve grafik türünü belirtin
Bu örnekte sorgu, son gün içinde başarısız olan istekleri seçer ve bunları işleminin bir parçası olarak oluşan özel durumları ile ilişkilendirir. Analytics bunları operation_ıd tanımlayıcısına göre ilişkilendirir. Sorgu sonuçları autocluster algoritması kullanılarak ardından ayırır. 

Kendi sorgularınızı oluşturduğunuzda, akışınıza eklemeden önce bunlar düzgün Analytics'te çalıştığını doğrulayın.

- Aşağıdaki Analytics sorgusu ekleme ve HTML tablosu grafik türü seçin. Ardından **yeni adım**.

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
    
    ![Analytics sorgu Yapılandırması penceresi](./media/automate-with-flow/8query.png)

### <a name="step-6-configure-the-flow-to-send-email"></a>6\. Adım: E-posta gönderme akışı Yapılandır

1. Arama **Office 365 Outlook**.
2. Tıklayın **Office 365 Outlook - e-posta Gönder**.

    ![Office 365 Outlook seçim penceresi](./media/automate-with-flow/9outlookaction.png)

1. İçinde **bir e-posta** penceresinde aşağıdakileri yapın:

   a. Alıcı e-posta adresini yazın.

   b. E-posta için bir konu yazın.

   c. Herhangi bir yeri tıklatın **gövdesi** kutusuna ve ardından, sağ tarafta açılan dinamik içerik menüsünde **gövdesi**.

   d. Tıklayın **Gelişmiş Seçenekleri Göster**.

    ![Office 365 Outlook yapılandırma](./media/automate-with-flow/10sendemailbody.png)

1. Dinamik içerik menüsünde, aşağıdakileri yapın:

    a. Seçin **ek adı**.

    b. Seçin **ek içeriği**.
    
    c. İçinde **HTML'dir** kutusunda **Evet**.

    ![Office 365 e-posta Yapılandırması penceresi](./media/automate-with-flow/11emailattachment.png)

### <a name="step-7-save-and-test-your-flow"></a>7\. Adım: Akışınızı test edin ve Kaydet
- İçinde **Akış adı** kutusuna akışınız için bir ad ekleyin ve ardından **Kaydet**.

    ![Akışı adlandırın ve kaydedin](./media/automate-with-flow/12nameflow.png)

Tetikleyici spustit tuto akci bekleyebilir veya hemen göre akış çalıştırma [tetikleyici talep üzerine çalıştırmaya](https://flow.microsoft.com/blog/run-now-and-six-more-services/).

Akış çalıştırıldığında, e-posta listede belirttiğiniz alıcılara aşağıdakine benzer bir e-posta iletisi alırsınız:

![Örnek e-posta](./media/automate-with-flow/flow9.png)


## <a name="next-steps"></a>Sonraki adımlar

- Oluşturma hakkında daha fazla bilgi edinin [analiz sorguları](../../azure-monitor/log-query/get-started-queries.md).
- Daha fazla bilgi edinin [Microsoft Flow](https://ms.flow.microsoft.com).



<!--Link references-->





