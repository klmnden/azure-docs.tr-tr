---
title: Çağrı, tetikleyici veya HTTP uç noktaları - Azure Logic Apps ile iş akışı iç içe | Microsoft Docs
description: Çağrı, tetikleyici veya iş akışları için Azure Logic Apps iç içe için HTTP uç noktaları ayarlama
services: logic-apps
keywords: İş akışları, HTTP uç noktaları
author: jeffhollan
manager: jeconnoc
editor: ''
documentationcenter: ''
ms.assetid: 73ba2a70-03e9-4982-bfc8-ebfaad798bc2
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.custom: H1Hack27Feb2017
ms.date: 03/31/2017
ms.author: klam; LADocs
ms.openlocfilehash: 9c02a0f540f52007412a850b9d69e337743fc35f
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35300207"
---
# <a name="call-trigger-or-nest-workflows-with-http-endpoints-in-logic-apps"></a>HTTP uç noktaları logic apps içinde iş akışlarıyla iç içe veya, tetikleyici çağırın

Böylece tetiklemek veya mantıksal uygulamalarınızı bir URL yoluyla çağrı logic apps Tetikleyicileri olarak zaman uyumlu HTTP uç noktaları yerel getirebilir. Ayrıca, iş akışları aranabilir uç noktaları desenini kullanarak logic apps içinde yerleştirebilirsiniz.

HTTP uç noktaları oluşturmak için mantıksal uygulamalarınızı gelen istekleri alabilmesi tetikler ekleyebilirsiniz:

* [İstek](../connectors/connectors-native-reqres.md)

* [API bağlantı Web kancası](../logic-apps/logic-apps-workflow-actions-triggers.md#apiconnection-trigger)

* [HTTP Web kancası](../connectors/connectors-native-webhook.md)

   > [!NOTE]
   > Bu örnekler kullansa da **isteği** tetikleyici, listelenen HTTP Tetikleyicileri birini kullanabilirsiniz ve tüm ilkeler özdeş diğer tetikleyici türlerine uygulanır.

## <a name="set-up-an-http-endpoint-for-your-logic-app"></a>Mantıksal uygulamanız için bir HTTP uç nokta ayarlama

Bir HTTP uç noktası oluşturmak için gelen istekleri alabilen tetikleyici ekleyin.

1. [Azure portalı](https://portal.azure.com "Azure portalı") oturumunu açın. Mantıksal uygulamanızı gidin ve mantığı Uygulama Tasarımcısı'nı açın.

2. Gelen istekleri almak mantıksal uygulamanızı sağlayan bir tetikleyici ekleyin. Örneğin, ekleyin **isteği** mantıksal uygulamanızı tetikleyiciye.

3.  Altında **iste gövde JSON şeması**, isteğe bağlı olarak bir JSON şeması almak için tetikleyici beklediğiniz için Yükü (veri) girebilirsiniz.

    Tasarımcı mantıksal uygulamanızı kullanmak, ayrıştırma ve tetikleyici, iş akışı aracılığıyla veri geçirmek için kullanabileceğiniz belirteçleri oluşturmak için bu şemayı kullanır. 
    Hakkında daha fazla bilgi [JSON şemaları oluşturulan belirteçleri](#generated-tokens).

    Bu örnekte, Tasarımcısı'nda gösterilen şema girin:

    ```json
    {
      "type": "object",
      "properties": {
        "address": {
          "type": "string"
        }
      },
      "required": [
        "address"
      ]
    }
    ```

    ![İstek Eylem Ekle][1]

    > [!TIP]
    > 
    > Örnek bir JSON yükü için bir şema gibi bir araçtan oluşturabileceğiniz [jsonschema.net](http://jsonschema.net/), veya **isteği** seçerek tetikleyici **şema üretmek için kullanım örnek yük**. 
    > Örnek yükünüzü girin ve seçin **Bitti**.

    Örneğin, bu örnek yükü:

    ```json
    {
       "address": "21 2nd Street, New York, New York"
    }
    ```

    Bu şemayı oluşturur:

    ```json
    {
       "type": "object",
       "properties": {
          "address": {
             "type": "string" 
          }
       }
    }
    ```

4.  Mantıksal uygulamanızı kaydedin. Altında **bu URL için HTTP POST**, artık bu örnek gibi bir oluşturulan geri çağırma URL'si bulmanız gerekir:

    ![Uç noktası için oluşturulan geri çağırma URL'si](./media/logic-apps-http-endpoint/generated-endpoint-url.png)

    Bu URL'yi bir paylaşılan erişim imzası (SAS) anahtarı kimlik doğrulaması için kullanılan sorgu parametrelerini içerir. 
    Azure portalında, mantığı uygulama genel bakış gelen HTTP uç noktası URL'si de alabilirsiniz. Altında **tetikleyici geçmişi**, tetikleyici seçin:

    ![Azure Portalı'ndan HTTP uç noktasının URL'sini alın][2]

    Veya bu çağrı yaparak URL'yi elde edebilirsiniz:

    ```
    POST https://management.azure.com/{logic-app-resourceID}/triggers/{myendpointtrigger}/listCallbackURL?api-version=2016-06-01
    ```

## <a name="change-the-http-method-for-your-trigger"></a>Tetikleyicinizin HTTP yöntemini değiştirme

Varsayılan olarak, **isteği** tetikleyici bir HTTP POST isteği bekliyor, ancak farklı bir HTTP yöntemini kullanabilirsiniz. 

> [!NOTE]
> Yalnızca bir yöntem türü belirtebilirsiniz.

1. Üzerinde **isteği** tetikleyin, seçin **Gelişmiş Seçenekleri Göster**.

2. Açık **yöntemi** listesi. Bu örnek için select **almak** böylece HTTP uç noktanın URL'sini daha sonra test edebilirsiniz.

    > [!NOTE]
    > Herhangi bir HTTP yöntemini seçin veya özel bir yöntem için kendi mantıksal uygulama belirtin.

    ![HTTP yöntemini değiştirme](./media/logic-apps-http-endpoint/change-method.png)

## <a name="accept-parameters-through-your-http-endpoint-url"></a>HTTP uç nokta URL'nizi aracılığıyla parametreleri kabul et

Parametreler kabul etmek için HTTP uç nokta URL'nizi istediğinizde, tetikleyicinin göreli yol özelleştirin.

1. Üzerinde **isteği** tetikleyin, seçin **Gelişmiş Seçenekleri Göster**. 

2. Altında **yöntemi**, isteğiniz kullanmak istediğiniz HTTP yöntemini belirtin. Bu örnek için select **almak** böylece HTTP uç noktanın URL'sini test, yapmadıysanız yöntemi.

      > [!NOTE]
      > Tetikleyici için göreli bir yol belirttiğinizde, tetikleyici için bir HTTP yöntemini de açıkça belirtmeniz gerekir.

3. Altında **göreli yol**, URL'niz, örneğin, kabul etmelidir parametresi göreli yolunu belirtin `customers/{customerID}`.

    ![Parametre için göreli yolu ve HTTP yöntemini belirtin](./media/logic-apps-http-endpoint/relativeurl.png)

4. Parametre kullanmak için ekleyin bir **yanıt** mantığı uygulamanıza eylem. (Seçin, tetikleyici altında **yeni adım** > **Eylem Ekle** > **yanıt**) 

5. Yanıt 's **gövde**, belirtecin, tetikleyicinin göreli yolda belirtilen parametresi için içerir.

    Örneğin, döndürülecek `Hello {customerID}`, yanıtın güncelleştirme **gövde** ile `Hello {customerID token}`. 
    Dinamik içerik listesi görüntülenir ve Göster `customerID` seçebilmeniz için belirteç.

    ![Yanıt gövdesi için parametre ekleme](./media/logic-apps-http-endpoint/relativeurlresponse.png)

    **Gövde** bu örnekteki gibi görünmelidir:

    ![Yanıt gövdesi parametresi](./media/logic-apps-http-endpoint/relative-url-with-parameter.png)

6. Mantıksal uygulamanızı kaydedin. 

    HTTP uç nokta URL'nizi şimdi Örneğin, göreli yol içerir: 

    HTTPS&#58;/ / prod-00.southcentralus.logic.azure.com/workflows/f90cb66c52ea4e9cabe0abf4e197deff/triggers/manual/paths/invoke/customers/{customerID}...

7. HTTP uç noktanızı sınamak için kopyalamak ve güncelleştirilmiş URL başka bir tarayıcı penceresine yapıştırın, ancak yerine `{customerID}` ile `123456`, ve Enter tuşuna basın.

    Tarayıcınız bu metin göstermesi gerekir: 

    `Hello 123456`

<a name="generated-tokens"></a>

### <a name="tokens-generated-from-json-schemas-for-your-logic-app"></a>Mantıksal uygulamanız için JSON şemaları üretilen belirteçleri

Bir JSON şeması sağladığınız zaman, **isteği** tetikleyici, mantığı Uygulama Tasarımcısı'nı bu şemada özellikleri için belirteçleri oluşturur. Mantıksal uygulama akışı aracılığıyla veri geçirme daha sonra bu belirteçleri kullanabilirsiniz.

Bu örneğin eklerseniz, `title` ve `name` özellikleri JSON şeması için kendi belirteçleri şimdi sonraki iş akışı adımlarda kullanmak için kullanılabilir. 

Tam JSON şeması şöyledir:

```json
{
   "type": "object",
   "properties": {
      "address": {
         "type": "string"
      },
      "title": {
         "type": "string"
      },
      "name": {
         "type": "string"
      }
   },
   "required": [
      "address",
      "title",
      "name"
   ]
}
```

## <a name="create-nested-workflows-for-logic-apps"></a>Logic apps iç içe geçmiş iş akışları oluşturmak

İstekleri alabilen diğer mantıksal uygulamalar ekleyerek mantıksal uygulamanızı iş akışları yerleştirebilirsiniz. Bu mantıksal uygulamalar dahil etmek için ekleyin **Azure Logic Apps - Logic Apps iş akışı seçin** , tetikleyici için eylem. Daha sonra uygun mantığı uygulamalardan seçebilirsiniz.

![Başka bir mantıksal uygulama Ekle](./media/logic-apps-http-endpoint/choose-logic-apps-workflow.png)

## <a name="call-or-trigger-logic-apps-through-http-endpoints"></a>HTTP uç noktaları aracılığıyla logic apps tetiklemek veya çağırın

HTTP uç noktanızı oluşturduktan sonra mantıksal uygulamanızı aracılığıyla tetikleyebilir bir `POST` yöntemi tam URL. Logic apps doğrudan erişimli uç noktaları için yerleşik destek vardır.

> [!NOTE] 
> El ile bir mantıksal uygulama mantığını Uygulama Tasarımcısı veya mantığı uygulama kod görünümü araç çubuğunda, herhangi bir anda çalıştırmayı seçin **çalıştırmak**.

## <a name="reference-content-from-an-incoming-request"></a>Gelen istek başvuru içeriği

İçeriğin türü ise `application/json`, gelen istekte özelliklerine başvurabilirsiniz. Aksi takdirde, içeriği diğer API'leri geçirebilirsiniz tek bir ikili birim olarak kabul edilir. Bu içerik iş akışı içinde başvurmak için bu içeriği dönüştürmeniz gerekir. Örneğin, geçirdiğiniz `application/xml` içerik, kullanabileceğiniz `@xpath()` bir XPath ayıklama için veya `@json()` XML JSON değerine dönüştürmek için. Hakkında bilgi edinin [içerik türleriyle çalışma](../logic-apps/logic-apps-content-type.md).

Gelen bir istekte çıkış almak için kullanabileceğiniz `@triggerOutputs()` işlevi. Çıktı aşağıdaki örnekte olduğu gibi görünebilir:

```json
{
    "headers": {
        "content-type" : "application/json"
    },
    "body": {
        "myProperty" : "property value"
    }
}
```

Erişim için `body` özelliği özellikle kullanabileceğiniz `@triggerBody()` kısayol. 

## <a name="respond-to-requests"></a>İsteklerine yanıt

Bir mantıksal uygulama çağırana içerik döndürerek Başlat belirli isteklere yanıt verecek şekilde isteyebilirsiniz. Durum kodu, başlığı ve yanıt için gövde oluşturmak için kullanabileceğiniz **yanıt** eylem. Bu eylem yalnızca, iş akışınızı sonunda mantıksal uygulamanızı herhangi bir yere görüntülenebilir.

> [!NOTE] 
> Mantıksal uygulamanızı içermiyorsa bir **yanıt**, HTTP uç noktası yanıt *hemen* ile bir **202 kabul edilen** durumu. Ayrıca, özgün istek yanıt almak için yanıt için gereken tüm adımları içinde bitmesi gereken [istek zaman aşımı sınırı](./logic-apps-limits-and-config.md) iş akışı iç içe geçmiş mantıksal uygulama olarak gerektirmediği sürece. Yanıt bu sınırı içinde olursa, gelen istek zaman aşımına uğradı ve HTTP yanıtı alır **408 istemci zaman aşımı**. İç içe geçmiş logic apps için üst mantıksal uygulama yanıt ne kadar zaman gerekli olduğuna bakılmaksızın, tamamlanana kadar beklemeye devam eder.

### <a name="construct-the-response"></a>Yapı yanıt

Yanıt gövdesi içinde birden fazla üst bilgi ve içerik herhangi bir türde içerebilir. Yanıtın içerik türü var. örnek yanıt üstbilgisini belirtir `application/json`. ve gövde içeren `title` ve `name`bağlı için daha önce güncelleştirilmiş JSON şeması olarak **isteği** tetikleyici.

![HTTP yanıtının eylem][3]

Yanıtları bu özelliklere sahiptir:

| Özellik | Açıklama |
| --- | --- |
| statusCode |Gelen istek için yanıt için HTTP durum kodu belirtir. Bu kod 2xx, 4xx veya 5xx ile başlayan tüm geçerli durum kodu olabilir. Ancak, 3xx durum kodları izin verilmez. |
| headers |Yanıta eklenecek üstbilgi herhangi bir sayıda tanımlar. |
| body |Bir dize olabilir bir gövde nesnesi, JSON nesnesi veya bir önceki adımda başvurulan bile ikili içerik belirtir. |

İşte JSON şeması nasıl şimdi göründüğünü **yanıt** eylem:

``` json
"Response": {
   "inputs": {
      "body": {
         "title": "@{triggerBody()?['title']}",
         "name": "@{triggerBody()?['name']}"
      },
      "headers": {
           "content-type": "application/json"
      },
      "statusCode": 200
   },
   "runAfter": {},
   "type": "Response"
}
```

> [!TIP]
> Mantıksal uygulamanız için tam JSON tanımını mantığı uygulama Designer'ı görüntülemek için seçin **kod görünümü**.

## <a name="q--a"></a>Soru-Cevap

#### <a name="q-what-about-url-security"></a>S: ne hakkında URL güvenlik?

Y: azure güvenli bir şekilde bir paylaşılan erişim imzası (SAS) kullanarak logic app geri çağırma URL'leri oluşturur. Bu imza sorgu parametresi olarak geçtiği ve mantıksal uygulamanızı tetikleyebilir önce doğrulanması gerekir. Azure mantıksal uygulama, tetikleyici adı ve gerçekleştirilir işlemi başına gizli bir anahtar benzersiz bir birleşimini kullanarak imza oluşturur. Bu nedenle birisi gizli mantığı uygulama anahtarı erişimi sürece, geçerli bir imzası oluşturulamaz.

   > [!IMPORTANT]
   > Üretim ve güvenli sistemler için mantıksal uygulamanızı doğrudan tarayıcıdan çünkü çağırma karşı önerilir:
   > 
   > * Paylaşılan erişim anahtarı URL'de görünür.
   > * Mantıksal uygulama müşterileri arasında paylaşılan etki alanları nedeniyle güvenli içerik ilkeleri yönetemez.

#### <a name="q-can-i-configure-http-endpoints-further"></a>S: daha fazla HTTP uç noktaları yapılandırabilir?

A: Evet HTTP uç noktaları aracılığıyla daha gelişmiş yapılandırma desteği [ **API Management**](../api-management/api-management-key-concepts.md). Bu hizmet, tutarlı bir şekilde mantıksal uygulamalar dahil olmak üzere tüm Apı'lerinizi, yönetme, özel etki alanı adlarını ayarlama, daha fazla kimlik doğrulama yöntemleri ve daha fazla bilgi, örneğin kullanmak özelliği de sunar:

* [Değişiklik isteği yöntemi](https://docs.microsoft.com/azure/api-management/api-management-advanced-policies#SetRequestMethod)
* [İsteğin URL kesimleri değiştirme](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#RewriteURL)
* API Management alanlarında ayarlama [Azure portal](https://portal.azure.com/ "Azure portalı")
* Temel kimlik doğrulaması için denetlenecek ilkesini ayarlayın

#### <a name="q-what-changed-when-the-schema-migrated-from-the-december-1-2014-preview"></a>S: şema 1 Aralık 2014 Önizlemesi'nden geçirilen ne değiştirilir?

A: Bu değişiklikler hakkında bir özeti aşağıda verilmiştir:

| 1 Aralık 2014 Önizleme | 1 Haziran 2016 |
| --- | --- |
| Tıklatın **HTTP dinleyicisi** API uygulaması |Tıklatın **el ile tetikleyici** (hiçbir API uygulaması gereklidir) |
| HTTP dinleyicisi ayarı "*yanıt otomatik olarak gönderir*" |Ya da dahil bir **yanıt** eylem veya iş akışı tanımı içinde değil |
| Temel veya OAuth kimlik doğrulamasını yapılandırma |API Yönetimi |
| HTTP yöntemini yapılandırma |Altında **Gelişmiş Seçenekleri Göster**, bir HTTP yöntemini seçin |
| Göreli yolunu Yapılandır |Altında **Gelişmiş Seçenekleri Göster**, göreli bir yol ekleme |
| Üzerinden gelen gövde başvurusu `@triggerOutputs().body.Content` |Aracılığıyla başvurusu `@triggerOutputs().body` |
| **HTTP yanıt gönderme** HTTP dinleyicisi eylemi |Tıklatın **HTTP isteğine yanıt** (hiçbir API uygulaması gereklidir) |

## <a name="get-help"></a>Yardım alın

Sorular sormak, soruları yanıtlamak ve diğer Azure Logic Apps kullanıcılarının neler yaptığını öğrenmek için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.

Azure Logic Apps ve bağlayıcıları geliştirmeye yardımcı olmak için, [Azure Logic Apps kullanıcı geri bildirim sitesinde](http://aka.ms/logicapps-wish) oy kullanın veya fikirlerinizi paylaşın.

## <a name="next-steps"></a>Sonraki adımlar

* [Mantıksal uygulama tanımları yazma](./logic-apps-author-definitions.md)
* [Hataları ve özel durumları işleme](./logic-apps-exception-handling.md)

[1]: ./media/logic-apps-http-endpoint/manualtrigger.png
[2]: ./media/logic-apps-http-endpoint/manualtriggerurl.png
[3]: ./media/logic-apps-http-endpoint/response.png