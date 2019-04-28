---
title: Liquid dönüşümler - Azure Logic Apps ile JSON verileri dönüştürme | Microsoft Docs
description: Dönüşümler veya Logic Apps ve Liquid şablonunu kullanarak gelişmiş JSON dönüştürmeleri için harita oluşturma
services: logic-apps
ms.service: logic-apps
author: divyaswarnkar
ms.author: divswa
ms.reviewer: estfan, LADocs
ms.suite: integration
ms.topic: article
ms.date: 08/16/2018
ms.openlocfilehash: 5472a8ce2670a34174d6d39f0d90faca8a7002ad
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61467586"
---
# <a name="perform-advanced-json-transformations-with-liquid-templates-in-azure-logic-apps"></a>Azure Logic Apps'te Liquid şablonları ile Gelişmiş JSON dönüştürmeleri gerçekleştirebilirsiniz

Temel JSON dönüştürmeleri mantıksal uygulamalarınızda yerel veri işlem eylemlerle gibi gerçekleştirebileceğiniz **Compose** veya **JSON Ayrıştır**. Gelişmiş JSON dönüştürmeleri gerçekleştirmek için şablonları veya eşlemeleriyle oluşturabilirsiniz [Liquid](https://shopify.github.io/liquid/), esnek web apps için bir açık kaynak şablonu dil olduğu. Liquid şablon JSON çıkışını dönüştürme hakkında tanımlar ve yinelemeler, denetim akışı, değişkenler vb. gibi daha karmaşık JSON dönüştürmeleri destekler. 

Mantıksal uygulamanızda Liquid dönüştürme gerçekleştirmek için önce önce JSON JSON eşlemesi bir harita Liquid şablonu ve depolama ile tümleştirme hesabınızdaki tanımlamanız gerekir. Bu makalede oluşturma ve bu Liquid şablon ya da harita kullanma gösterilmektedir. 

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Bir aboneliğiniz yoksa [ücretsiz bir Azure hesabı ile başlayabilirsiniz](https://azure.microsoft.com/free/). Veya, [için Kullandıkça Öde aboneliğine kaydolun](https://azure.microsoft.com/pricing/purchase-options/).

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Temel bir [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md)

* Hakkında temel bilgilere [Liquid şablon dili.](https://shopify.github.io/liquid/)

## <a name="create-liquid-template-or-map-for-your-integration-account"></a>Liquid şablonu veya tümleştirme hesabı için harita oluşturma

1. Bu örnekte, bu adımda açıklandığı gibi örnek Liquid şablonu oluşturun. Liquid şablonunuzda kullanabilirsiniz [sıvı filtreler](https://shopify.github.io/liquid/basics/introduction/#filters), kullanım [DotLiquid](https://dotliquidmarkup.org/) ve C# adlandırma kuralları. 

   > [!NOTE]
   > Kullandığınız filtre adlar emin *cümle* şablonunuzdaki. Aksi takdirde, filtreleri çalışmaz.

   ```json
   {%- assign deviceList = content.devices | Split: ', ' -%}
   
   {
      "fullName": "{{content.firstName | Append: ' ' | Append: content.lastName}}",
      "firstNameUpperCase": "{{content.firstName | Upcase}}",
      "phoneAreaCode": "{{content.phone | Slice: 1, 3}}",
      "devices" : [
         {%- for device in deviceList -%}
            {%- if forloop.Last == true -%}
            "{{device}}"
            {%- else -%}
            "{{device}}",
            {%- endif -%}
        {%- endfor -%}
        ]
   }
   ```

2. [Azure Portal](https://portal.azure.com) oturum açın. Ana Azure menüsünde **tüm kaynakları**. Arama kutusuna bulmak ve tümleştirme hesabınızı seçin.

   ![Tümleştirme hesabı seçin](./media/logic-apps-enterprise-integration-liquid-transform/select-integration-account.png)

3.  Altında **bileşenleri**seçin **haritalar**.

    ![MAPS'ı seçin](./media/logic-apps-enterprise-integration-liquid-transform/add-maps.png)

4. Seçin **Ekle** ve haritanızı için bu ayrıntıları sağlayın:

   | Özellik | Değer | Açıklama | 
   |----------|-------|-------------|
   | **Ad** | JsonToJsonTemplate | Bu örnekte "JsonToJsonTemplate" olan haritanızı adı | 
   | **Eşleme türü** | **Sıvı** | Haritanızı türü. JSON dönüştürme için JSON için seçmelisiniz **liquid**. | 
   | **Harita** | "SimpleJsonToJsonTemplate.liquid" | Bu örnekte "SimpleJsonToJsonTemplate.liquid" olan bir dönüştürme için kullanılacak bir mevcut Liquid şablonu veya eşleme dosyası. Bu dosyayı bulmak için dosya seçiciyi kullanabilirsiniz. |
   ||| 

   ![Liquid şablonu Ekle](./media/logic-apps-enterprise-integration-liquid-transform/add-liquid-template.png)
    
## <a name="add-the-liquid-action-for-json-transformation"></a>JSON dönüştürme için sıvı eylemi ekleme

1. Azure portalında aşağıdaki adımları [boş mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

2. Logic Apps Tasarımcısı'nda ekleme [istek tetikleyicisi](../connectors/connectors-native-reqres.md#use-the-http-request-trigger) mantıksal uygulamanız için.

3. Tetikleyici altında seçin **yeni adım**. 
   Arama kutusuna filtreniz olarak "Sıvı" girin ve şu eylemi seçin: **Sıvı olan JSON - JSON dönüştürmesi Uygula**

   ![Bulma ve Liquid eylemini seçin](./media/logic-apps-enterprise-integration-liquid-transform/search-action-liquid.png)

4. İçine tıklayın **içeriği** dinamik içerik listesinde görünmesi kutusunda ve seçin **gövdesi** belirteci.
  
   ![Gövde seçin](./media/logic-apps-enterprise-integration-liquid-transform/select-body.png)
 
5. Gelen **harita** listesinde, bu örnekte "JsonToJsonTemplate" olan, sıvı şablonunuzu seçin.

   ![Harita Seç](./media/logic-apps-enterprise-integration-liquid-transform/select-map.png)

   Haritalar liste boşsa, büyük olasılıkla, mantıksal uygulama tümleştirme hesabınıza bağlı değil. 
   Mantıksal uygulamanızı Liquid şablon veya harita olan tümleştirme hesabı bağlamak için şu adımları izleyin:

   1. Mantıksal uygulama menüsünde seçin **iş akışı ayarları**.

   2. Gelen **tümleştirme hesabı seçin** listesinde, tümleştirme hesabınızı seçin ve seçin **Kaydet**.

      ![Tümleştirme hesabı için mantıksal uygulamayı Bağla](./media/logic-apps-enterprise-integration-liquid-transform/link-integration-account.png)

## <a name="test-your-logic-app"></a>Mantıksal uygulamanızı test edin

Gönderi mantıksal uygulamanızdan JSON girişi [Postman](https://www.getpostman.com/postman) veya benzer bir araç. Dönüştürülen JSON çıktısı, mantıksal uygulamanız şu örnekteki gibi görünür:
  
![Örnek çıktı](./media/logic-apps-enterprise-integration-liquid-transform/example-output-jsontojson.png)

## <a name="more-liquid-action-examples"></a>Liquid eylem ile ilgili daha fazla örnek
Sıvı yalnızca JSON dönüştürmeleri için sınırlı değildir. Sıvı kullanan diğer kullanılabilir dönüştürme Eylemler aşağıda verilmiştir.

* JSON metnine Dönüştür
  
  Bu örnek için kullanılan Liquid şablonu aşağıda verilmiştir:
   
   ``` json
   {{content.firstName | Append: ' ' | Append: content.lastName}}
   ```
   Örnek giriş ve çıkışları şunlardır:
  
   ![Metin çıktı JSON örneği](./media/logic-apps-enterprise-integration-liquid-transform/example-output-jsontotext.png)

* XML-JSON dönüştürmesi uygula
  
  Bu örnek için kullanılan Liquid şablonu aşağıda verilmiştir:
   
   ``` json
   [{% JSONArrayFor item in content -%}
        {{item}}
    {% endJSONArrayFor -%}]
   ```
   Örnek giriş ve çıkışları şunlardır:

   ![Örnek Çıktı XML-JSON](./media/logic-apps-enterprise-integration-liquid-transform/example-output-xmltojson.png)

* XML metni dönüştürme
  
  Bu örnek için kullanılan Liquid şablonu aşağıda verilmiştir:

   ``` json
   {{content.firstName | Append: ' ' | Append: content.lastName}}
   ```

   Örnek giriş ve çıkışları şunlardır:

   ![Metin çıktı XML örneği](./media/logic-apps-enterprise-integration-liquid-transform/example-output-xmltotext.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Enterprise Integration Pack hakkında daha fazla bilgi](../logic-apps/logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack hakkında bilgi edinin")  
* [Eşlemeleri hakkında daha fazla bilgi](../logic-apps/logic-apps-enterprise-integration-maps.md "Kurumsal tümleştirme eşlemeleri hakkında bilgi edinin")  

