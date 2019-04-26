---
title: Çağrı, tetikleyici veya iç içe HTTP uç noktaları - Azure Logic Apps ile iş akışlarını | Microsoft Docs
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
ms.openlocfilehash: c58b39f8e2d49eeb3e64c7ffce1d34d7a7b7b780
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60304272"
---
# <a name="call-trigger-or-nest-workflows-with-http-endpoints-in-logic-apps"></a>Çağrı, tetikleyici veya iç içe iş akışları ile HTTP uç noktalarını logic apps'teki

Tetiklemek ya da URL aracılığıyla, mantıksal uygulamaları çağırma logic apps'te tetikleyici olarak yerel olarak zaman uyumlu bir HTTP uç noktalarını kullanıma sunabilirsiniz. Çağrılabilir uç noktalar desenini kullanarak logic apps iş akışları da yerleştirebilirsiniz.

HTTP uç noktaları oluşturmak için logic apps, gelen istekleri alabilmesini sağlamak bu Tetikleyiciler ekleyebilirsiniz:

* [İstek](../connectors/connectors-native-reqres.md)

* [API bağlantısı Web kancası](../logic-apps/logic-apps-workflow-actions-triggers.md#apiconnection-trigger)

* [HTTP Web kancası](../connectors/connectors-native-webhook.md)

   > [!NOTE]
   > Bu örneklerde rağmen **istek** tetikleyici listelenen HTTP Tetikleyicileri dilediğinizi kullanabilirsiniz ve tüm ilkeler diğer tetikleyici türleriyle aynı şekilde geçerlidir.

## <a name="set-up-an-http-endpoint-for-your-logic-app"></a>Mantıksal uygulamanız için bir HTTP uç noktası ayarlama

Bir HTTP uç noktası oluşturmak için gelen istekleri alabilecek bir tetikleyici ekleme.

1. [Azure portalı](https://portal.azure.com "Azure portalı") oturumunu açın. Mantıksal uygulamanıza gidin ve mantıksal Uygulama Tasarımcısı'nı açın.

2. Mantıksal uygulamanız gelen istekleri almaya olanak tanıyan bir tetikleyici ekleme. Örneğin, ekleme **istek** mantıksal uygulamanızın tetikleyicisi.

3.  Altında **istek gövdesi JSON şeması**, isteğe bağlı olarak bir JSON şeması almak için tetikleyici beklediğiniz Yükü (veriler) girebilirsiniz.

    Tasarımcı, mantıksal uygulamanızı kullanmak, ayrıştırma ve tetikleme, iş akışı aracılığıyla veri geçirmek için kullanabileceğiniz belirteçleri oluşturmak için bu şema kullanır. 
    Hakkında daha fazla bilgi [JSON şemalardan oluşturulan belirteçleri](#generated-tokens).

    Bu örnekte, şema Tasarımcısı'nda gösterilen girin:

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
    > Gibi bir araçla gelen bir örnek JSON yükü için bir şema oluşturmak [jsonschema.net](https://jsonschema.net/), veya **istek** seçerek tetikleyici **şema oluşturmak için örnek yük kullanma**. 
    > Örnek yükünüzü girin ve **Bitti**.

    Örneğin, bu örnek yük:

    ```json
    {
       "address": "21 2nd Street, New York, New York"
    }
    ```

    Bu şema oluşturur:

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

4.  Mantıksal uygulamanızı kaydedin. Altında **bu URL için HTTP POST**, artık bir geri çağırma URL'si oluşturuldu, bu örnekte olduğu gibi bulmanız gerekir:

    ![Uç noktası için oluşturulan geri çağırma URL'si](./media/logic-apps-http-endpoint/generated-endpoint-url.png)

    Bu URL'yi kimlik doğrulaması için kullanılan sorgu parametreleri bir paylaşılan erişim imzası (SAS) anahtarı içerir. 
    Azure portalında, mantıksal Uygulama'ya genel bakış sayfasından HTTP uç nokta URL'sini de edinebilirsiniz. Altında **tetikleyici geçmişi**, tetikleyici seçin:

    ![Azure portalından Get HTTP uç nokta URL'si][2]

    Ya da bu çağrıyı yaparak URL'sini alabilirsiniz:

    ```
    POST https://management.azure.com/{logic-app-resourceID}/triggers/{myendpointtrigger}/listCallbackURL?api-version=2016-06-01
    ```

## <a name="change-the-http-method-for-your-trigger"></a>Tetikleyicinize ait HTTP yöntemini değiştirme

Varsayılan olarak, **isteği** tetikleyici, bir HTTP POST isteği bekliyor, ancak farklı bir HTTP yöntemini kullanabilirsiniz. 

> [!NOTE]
> Yalnızca bir yöntem türü belirtebilirsiniz.

1. Üzerinde **istek** tetikleme öğesini **Gelişmiş Seçenekleri Göster**.

2. Açık **yöntemi** listesi. Bu örnekte, seçin **alma** böylece daha sonra HTTP uç noktasının URL'sini test edebilirsiniz.

    > [!NOTE]
    > Herhangi bir HTTP yöntemini seçin veya kendi mantıksal uygulama için özel bir yönteme belirtin.

    ![HTTP yöntemini değiştirme](./media/logic-apps-http-endpoint/change-method.png)

## <a name="accept-parameters-through-your-http-endpoint-url"></a>HTTP uç nokta URL'nizi aracılığıyla parametreleri kabul eder

Parametreler kabul etmek için HTTP uç nokta URL'nizi istediğinizde, tetikleyici göreli yol özelleştirin.

1. Üzerinde **istek** tetikleme öğesini **Gelişmiş Seçenekleri Göster**. 

2. Altında **yöntemi**, isteğiniz kullanmak istediğiniz HTTP yöntemini belirtin. Bu örnekte, seçin **alma** HTTP uç noktasının URL'sini test edebilmeniz, henüz kaydolmadıysanız yöntemi.

      > [!NOTE]
      > Tetikleyiciniz için göreli bir yol belirttiğinizde, tetikleyici için bir HTTP yöntemini de açıkça belirtmeniz gerekir.

3. Altında **göreli yol**, URL'niz, örneğin, kabul etmelidir parametresi göreli yolunu belirtin `customers/{customerID}`.

    ![HTTP yöntemi ve parametresi için göreli yolunu belirtin](./media/logic-apps-http-endpoint/relativeurl.png)

4. Parametresini kullanmak için ekleme bir **yanıt** mantıksal uygulamanız için eylem. (Tetikleyicinizin altında seçin **yeni adım** > **Eylem Ekle** > **yanıt**) 

5. İçinde yanıtın **gövdesi**, tetikleyicinin göreli yolda belirtilen parametresi için belirteci içerir.

    Örneğin, döndürülecek `Hello {customerID}`, güncelleştirme, yanıtın **gövdesi** ile `Hello {customerID token}`. 
    Dinamik içerik listesi görüntülenir ve Göster `customerID` seçebilmeniz için belirteç.

    ![Yanıt gövdesi için parametre Ekle](./media/logic-apps-http-endpoint/relativeurlresponse.png)

    **Gövdesi** şu örnekteki gibi görünmelidir:

    ![Parametresi ile yanıt gövdesi](./media/logic-apps-http-endpoint/relative-url-with-parameter.png)

6. Mantıksal uygulamanızı kaydedin. 

    HTTP uç nokta URL'nizi artık göreli yol, örneğin içerir: 

    https&#58;//prod-00.southcentralus.logic.azure.com/workflows/f90cb66c52ea4e9cabe0abf4e197deff/triggers/manual/paths/invoke/customers/{customerID}...

7. HTTP uç noktanızı test etmek için kopyalayın ve başka bir tarayıcı penceresine güncelleştirilmiş URL'yi yapıştırın, ancak değiştirin `{customerID}` ile `123456`, ve Enter tuşuna basın.

    Tarayıcınız bu metin göstermelidir: 

    `Hello 123456`

<a name="generated-tokens"></a>

### <a name="tokens-generated-from-json-schemas-for-your-logic-app"></a>Mantıksal uygulamanızın JSON şemalarının oluşturulan belirteçleri

Bir JSON şemasında verdiğiniz zaman, **istek** tetikleyici, Logic Apps Tasarımcısı'Bu şemada özellikleri için belirteçleri oluşturur. Ardından, mantıksal uygulama iş akışınızı aracılığıyla veri geçirmek için bu belirteçleri kullanabilirsiniz.

Bu örnekte, eklerseniz `title` ve `name` özellikleri, JSON şemasında belirteçleriyle artık sonraki iş akışı adımlarda kullanmak için kullanılabilir. 

Tam JSON şeması şu şekildedir:

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

## <a name="create-nested-workflows-for-logic-apps"></a>Logic apps için iç içe geçmiş iş akışları oluşturun

Diğer mantıksal uygulamalar, istekleri alabilen ekleyerek mantıksal uygulamanızı iş akışları iç içe yerleştirebilirsiniz. Bu mantıksal uygulamalar'ı dahil etmek için ekleme **Azure Logic Apps - Logic Apps iş akışı seçin** tetikleyicinize eylem. Ardından, uygun logic apps'ten da seçebilirsiniz.

![Başka bir mantıksal uygulama ekleme](./media/logic-apps-http-endpoint/choose-logic-apps-workflow.png)

## <a name="call-or-trigger-logic-apps-through-http-endpoints"></a>Çağrı yapma veya HTTP uç noktaları aracılığıyla mantıksal uygulamalar'ı tetikleyicisi

HTTP uç noktanızı oluşturmanızın ardından mantıksal uygulamanızı tetikleyecek bir `POST` yöntemi için tam URL. Logic apps, doğrudan erişim uç noktaları için yerleşik desteği vardır.

> [!NOTE] 
> Mantıksal uygulama kod görünümü ya da mantıksal Uygulama Tasarımcısı araç çubuğunda herhangi bir zamanda bir mantıksal uygulama'el ile çalıştırmayı **çalıştırma**.

## <a name="reference-content-from-an-incoming-request"></a>Gelen bir istek başvuru içeriği

İçeriğin türü ise `application/json`, gelen istekte özelliklerine başvurabilirsiniz. Aksi takdirde, içeriği, diğer API'ler geçirebileceğiniz tek bir ikili birim olarak kabul edilir. Bu içerik iş akışı içinde başvurmak için bu içeriği dönüştürmeniz gerekir. Örneğin, geçirirseniz `application/xml` içeriği kullanabilirsiniz `@xpath()` bir XPath ayıklama için veya `@json()` XML, JSON değerine dönüştürmek için. Hakkında bilgi edinin [içerik türleri ile çalışmaktan](../logic-apps/logic-apps-content-type.md).

Gelen isteğinden çıktısını almak için kullanabileceğiniz `@triggerOutputs()` işlevi. Çıktı şu örnekteki gibi görünebilir:

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

## <a name="respond-to-requests"></a>İsteklerini yanıtlama

Bir mantıksal uygulama, içeriği çağırana döndürerek Başlat belirli isteklerine yanıt vermek isteyebilirsiniz. Durum kodu, üst bilgi ve yanıt için gövde oluşturmak için kullanabileceğiniz **yanıt** eylem. Bu eylem yalnızca sonunda, iş akışınızı, mantıksal uygulamanızı herhangi bir yerde görünebilir.

> [!NOTE] 
> Mantıksal uygulamanızı içermiyorsa bir **yanıt**, HTTP uç noktasına yanıt *hemen* ile bir **202 kabul edildi** durumu. Ayrıca, özgün istek yanıt almak yanıt için gerekli tüm adımları içinde bitmesi gereken [istek zaman aşımı sınırı](./logic-apps-limits-and-config.md) olarak iç içe geçmiş mantıksal uygulama iş akışı çağırmadığınız sürece. Bu sınırı içinde yanıt durum olursa, gelen istek zaman aşımına uğrar ve HTTP yanıtı alır **408 istemci zaman aşımı**. İç içe mantıksal uygulamalar için üst mantıksal uygulama, ne kadar zaman gerekli bağımsız olarak tamamlanana kadar yanıt beklemesi devam eder.

### <a name="construct-the-response"></a>Yanıt oluşturun

Yanıt gövdesi içinde birden fazla üst bilgi ve herhangi bir türde içerik içerebilir. Yanıtın içerik türü var. örnek yanıt olarak, üst bilgiyi belirtir `application/json`. ve gövdesinde `title` ve `name`için daha önce güncelleştirilmiş JSON şeması göre **istek** tetikleyici.

![HTTP yanıt eylemi][3]

Yanıt şu özelliklere sahiptir:

| Özellik | Açıklama |
| --- | --- |
| statusCode |Gelen istek için yanıt için HTTP durum kodu belirtir. Bu kod, 2xx, 4xx veya 5xx ile başlayan herhangi bir geçerli durum kodu olabilir. Ancak, 3xx durum kodları izin verilmez. |
| Üst bilgileri |Yanıta eklenecek üstbilgi herhangi bir sayıda tanımlar. |
| body |Bir dize olabilir bir gövde nesnesi, JSON nesnesi veya bir önceki adımda başvurulan bile ikili içeriği belirtir. |

İşte JSON şeması artık nasıl göründüğünü **yanıt** eylem:

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
> Mantıksal Uygulama Tasarımcısı mantıksal uygulamanız için tam JSON tanımı görüntülemek için seçin **kod görünümü**.

## <a name="q--a"></a>Soru-Cevap

#### <a name="q-what-about-url-security"></a>S: URL güvenlik hakkında neler diyeceksiniz?

Y: Azure paylaşılan erişim imzası (SAS) kullanarak mantıksal uygulama geri çağırma URL'leri güvenli bir şekilde oluşturur. Bu imza aracılığıyla bir sorgu parametresi olarak geçirir ve mantıksal uygulamanızı tetikleyebilir önce doğrulanması gerekir. Azure eşsiz bir bileşimiyle mantıksal uygulama, tetikleyici adını ve gerçekleştirilen işlem başına gizli bir anahtar kullanarak imzayı oluşturur. Bu nedenle birisi gizli mantıksal uygulama anahtarına erişime sahip olmadığı sürece, geçerli bir imzaya oluşturulamıyor.

   > [!IMPORTANT]
   > Üretim ve güvenlik sistemleri için mantıksal uygulamanızı doğrudan tarayıcıdan çünkü çağırma karşı öneririz:
   > 
   > * Paylaşılan erişim anahtarı URL'SİNDE görünür.
   > * Mantıksal uygulama müşteriler arasında paylaşılan etki alanları nedeniyle güvenli içerik ilkelerini yönetemez.

#### <a name="q-can-i-configure-http-endpoints-further"></a>S: Daha fazla HTTP uç noktalarını yapılandırabilirim?

Y: Evet, HTTP uç noktaları aracılığıyla daha gelişmiş yapılandırma desteği [ **API Management**](../api-management/api-management-key-concepts.md). Bu hizmet, tutarlı bir şekilde mantıksal uygulamalar da dahil olmak üzere tüm API'leri, yönetme, özel etki alanı adlarını ayarlama, daha fazla kimlik doğrulama yöntemleri ve diğer örneğin kullanmak özelliği de sunar:

* [Değişiklik isteği yöntemi](https://docs.microsoft.com/azure/api-management/api-management-advanced-policies#SetRequestMethod)
* [İsteğin URL kesimleri değiştirme](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#RewriteURL)
* API Management etki alanlarınızı ayarlama [Azure portalında](https://portal.azure.com/ "Azure portalı")
* Temel kimlik doğrulaması için denetlenecek İlkesi ayarlama

#### <a name="q-what-changed-when-the-schema-migrated-from-the-december-1-2014-preview"></a>S: 1 Aralık 2014'ten Önizleme şema geçişi olduğunda ne değişti?

Y: Bu değişiklikler ile ilgili bir özeti aşağıda verilmiştir:

| 1 Aralık 2014'ten preview | 1 Haziran 2016 |
| --- | --- |
| Tıklayın **HTTP dinleyicisi** API uygulaması |Tıklayın **el ile tetikleyici** (herhangi bir API uygulaması gereklidir) |
| HTTP dinleyicisi ayarı "*yanıt otomatik olarak gönderir*" |Ya da içeren bir **yanıt** eylem veya iş akışı tanımı içinde değil |
| Temel veya OAuth kimlik doğrulamasını yapılandırma |API Management |
| HTTP yöntemini yapılandırma |Altında **Gelişmiş Seçenekleri Göster**, bir HTTP yöntemini seçin |
| Göreli yolunu Yapılandır |Altında **Gelişmiş Seçenekleri Göster**, göreli bir yol ekleyin |
| Üzerinden gelen gövdesi başvuru `@triggerOutputs().body.Content` |Aracılığıyla başvurusu `@triggerOutputs().body` |
| **HTTP yanıt gönderme** HTTP dinleyicisi eylemi |Tıklayın **HTTP isteğine yanıt** (herhangi bir API uygulaması gereklidir) |

## <a name="get-help"></a>Yardım alın

Sorular sormak, soruları yanıtlamak ve diğer Azure Logic Apps kullanıcılarının neler yaptığını öğrenmek için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.

Azure Logic Apps ve bağlayıcıları geliştirmeye yardımcı olmak için, [Azure Logic Apps kullanıcı geri bildirim sitesinde](https://aka.ms/logicapps-wish) oy kullanın veya fikirlerinizi paylaşın.

## <a name="next-steps"></a>Sonraki adımlar

* [Mantıksal uygulama tanımları yazma](./logic-apps-author-definitions.md)
* [Hataları ve özel durumları işleme](./logic-apps-exception-handling.md)

[1]: ./media/logic-apps-http-endpoint/manualtrigger.png
[2]: ./media/logic-apps-http-endpoint/manualtriggerurl.png
[3]: ./media/logic-apps-http-endpoint/response.png