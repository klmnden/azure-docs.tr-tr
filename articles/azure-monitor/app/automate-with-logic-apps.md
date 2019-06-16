---
title: Logic Apps'i kullanarak Azure Application Insights süreçlerini otomatikleştirin.
description: Nasıl hızlı bir şekilde yinelenebilir işlemler mantıksal uygulamanıza Application Insights Bağlayıcısı ekleyerek otomatikleştirebilirsiniz öğrenin.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 03/11/2019
ms.author: mbullwin
ms.openlocfilehash: 61215adc2aee5cef3693d119bf0efb36526d748b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60904839"
---
# <a name="automate-application-insights-processes-by-using-logic-apps"></a>Logic Apps'i kullanarak Application Insights süreçlerini otomatikleştirin

Hizmetinizi düzgün çalışıp çalışmadığını denetlemek için telemetri verilerini sürekli olarak çalışan aynı sorgu kendiniz bulurum? Eğilimleri ve anormallikleri bulmak için bu sorguları otomatikleştirme ve ardından etrafında kendi akışlarınızı oluşturmak istiyorsunuz? Logic Apps için Azure Application Insights Bağlayıcısı sağ araç bu amaç için kullanılır.

Bu tümleştirme sayesinde, tek satırlık bir kod yazmaya gerek kalmadan çok sayıda işlemi otomatikleştirebilirsiniz. Herhangi bir Application Insights işlem hızlı bir şekilde otomatikleştirmek için Application Insights Bağlayıcısı ile bir mantıksal uygulama oluşturabilirsiniz. 

Ek Eylemler ekleyebilirsiniz. Azure App Service'in Logic Apps özelliği, Eylemler yüzlerce kullanılabilir hale getirir. Örneğin, bir mantıksal uygulama kullanarak, otomatik olarak bir e-posta bildirimi gönder veya Azure DevOps bir hata oluşturun. Kullanılabilir çok birini de kullanabilirsiniz [şablonları](https://docs.microsoft.com/azure/logic-apps/logic-apps-use-logic-app-templates) mantıksal uygulamanızı oluşturma sürecini hızlandırmak amacıyla. 

## <a name="create-a-logic-app-for-application-insights"></a>Application Insights için bir mantıksal uygulama oluşturma

Bu öğreticide, verileri bir web uygulaması için Grup öznitelikleri için Analytics autocluster algoritması kullanan bir mantıksal uygulama oluşturma konusunda bilgi edinin. Akış, e-postayla nasıl Application Insights Analytics ve Logic Apps birlikte kullanabileceğiniz tek bir örnek sonuçları otomatik olarak gönderir. 

### <a name="step-1-create-a-logic-app"></a>1\. adım: Mantıksal uygulama oluşturma
1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Tıklayın **kaynak Oluştur**seçin **Web + mobil**ve ardından **mantıksal uygulama**.

    ![Yeni bir mantıksal uygulama penceresi](./media/automate-with-logic-apps/1createlogicapp.png)

### <a name="step-2-create-a-trigger-for-your-logic-app"></a>2\. adım: Mantıksal uygulamanız için bir Tetikleyici oluşturma
1. İçinde **mantıksal Uygulama Tasarımcısı** penceresinin altında **sık kullanılan bir tetikleyici ile başlayın**seçin **yinelenme**.

    ![Mantıksal Uygulama Tasarımcısı penceresi](./media/automate-with-logic-apps/2logicappdesigner.png)

1. İçinde **aralığı** kutusuna **1** ve**sıklığı** kutusunda **gün**.

    ![Mantıksal Uygulama Tasarımcısı "Yinelenme" penceresi](./media/automate-with-logic-apps/3recurrence.png)

### <a name="step-3-add-an-application-insights-action"></a>3\. adım: Application Insights Eylem Ekle
1. Tıklayın **yeni adım**.

1. İçinde **eylem seçin** arama kutusuna **Azure Application Insights**.

1. Altında **eylemleri**, tıklayın **Azure Application Insights - görselleştirme Analytics sorgusu**.

    ![Mantıksal Uygulama Tasarımcısı penceresinde "eylem seçin"](./media/automate-with-logic-apps/4visualize.png)

### <a name="step-4-connect-to-an-application-insights-resource"></a>4\. Adım: Bir Application Insights kaynağına bağlanma

Bu adımı tamamlamak için kaynak için bir uygulama kimliği ve API anahtarı gerekir. Azure portalından, aşağıdaki diyagramda gösterildiği gibi geri alabilir:

![Azure portalında uygulama kimliği](./media/automate-with-logic-apps/5apiaccess.png)

![Azure portalında uygulama kimliği](./media/automate-with-logic-apps/6apikey.png)

Bağlantı, uygulama kimliği ve API anahtarı için bir ad sağlayın.

![Mantıksal Uygulama Tasarımcısı akış bağlantı penceresi](./media/automate-with-logic-apps/7connection.png)

### <a name="step-5-specify-the-analytics-query-and-chart-type"></a>5\. Adım: Analytics sorgu ve grafik türünü belirtin
Aşağıdaki örnekte, sorgu başarısız isteklerin son gün içinde seçer ve bunları işleminin bir parçası olarak oluşan özel durumları ile ilişkilendirir. Analytics operation_ıd tanımlayıcısına göre başarısız olan istekleri ilişkilendirir. Sorgu sonuçları autocluster algoritması kullanılarak ardından ayırır. 

Kendi sorgularınızı oluşturduğunuzda, akışınıza eklemeden önce bunlar düzgün Analytics'te çalıştığını doğrulayın.

1. İçinde **sorgu** kutusunda, aşağıdaki Analytics sorgusu ekleyin:

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

1. İçinde **grafik türü** kutusunda **Html tablosu**.

    ![Analytics sorgu Yapılandırması penceresi](./media/automate-with-logic-apps/8query.png)

### <a name="step-6-configure-the-logic-app-to-send-email"></a>6\. Adım: Mantıksal uygulama, e-posta göndermek için yapılandırma

1. Tıklayın **yeni adım**.

1. Arama kutusuna **Office 365 Outlook**.

1. Tıklayın **Office 365 Outlook - e-posta Gönder**.

    ![Office 365 Outlook seçimi](./media/automate-with-logic-apps/9sendemail.png)

1. İçinde **bir e-posta** penceresinde aşağıdakileri yapın:

   a. Alıcı e-posta adresini yazın.

   b. E-posta için bir konu yazın.

   c. Herhangi bir yeri tıklatın **gövdesi** kutusuna ve ardından, sağ tarafta açılan dinamik içerik menüsünde **gövdesi**.
    
   d. Tıklayın **yeni parametre Ekle** açılır listesine tıklayıp ekler ve HTML'dir.

      ![Office 365 Outlook yapılandırma](./media/automate-with-logic-apps/10emailbody.png)

      ![Office 365 Outlook yapılandırma](./media/automate-with-logic-apps/11emailparameter.png)

1. Dinamik içerik menüsünde, aşağıdakileri yapın:

    a. Seçin **ek adı**.

    b. Seçin **ek içeriği**.
    
    c. İçinde **HTML'dir** kutusunda **Evet**.

      ![Office 365 e-posta yapılandırma ekranında](./media/automate-with-logic-apps/12emailattachment.png)

### <a name="step-7-save-and-test-your-logic-app"></a>7\. Adım: Kaydet ve mantıksal uygulamanızı test edin
* Tıklayın **Kaydet** yaptığınız değişiklikleri kaydedin.

Mantıksal uygulamayı çalıştırmak tetikleyici için bekleyebilir veya seçerek, hemen bir mantıksal uygulama çalıştırabilirsiniz **çalıştırma**.

![Logic app oluşturma ekranı](./media/automate-with-logic-apps/13save.png)

Mantıksal uygulamanız çalıştığında, e-posta listesinde belirtilen alıcılara aşağıdakine benzer bir e-posta alırsınız:

![Mantıksal uygulama e-posta iletisi](./media/automate-with-logic-apps/flow9.png)

## <a name="next-steps"></a>Sonraki adımlar

- Oluşturma hakkında daha fazla bilgi edinin [analiz sorguları](../../azure-monitor/log-query/get-started-queries.md).
- [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps) hakkında daha fazla bilgi edinin.



<!--Link references-->





