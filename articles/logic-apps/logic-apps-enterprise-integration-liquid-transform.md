---
title: "JSON veri sıvı dönüşümler - Azure Logic Apps ile Dönüştür | Microsoft Docs"
description: "Dönüşümler veya Logic Apps ve sıvı şablonunu kullanarak gelişmiş JSON dönüştürmeleri için eşlemeleri oluşturun."
services: logic-apps
documentationcenter: 
author: divyaswarnkar
manager: anneta
editor: 
ms.assetid: 
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: LADocs; divswa
ms.openlocfilehash: cd177b1ebcb5d236ce265dc153ee6a02125a69df
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2017
---
# <a name="advanced-json-transformations-using-liquid-template"></a>Sıvı şablonu kullanarak gelişmiş JSON dönüşümleri
Azure mantıksal uygulamaları oluşturma veya ayrıştırma JSON gibi yerel veri işlemi eylemleri aracılığıyla temel JSON dönüşümleri destekler. Logic Apps şimdi sıvı şablonları ile Gelişmiş JSON dönüştürmeler de destekler. [Sıvı](https://shopify.github.io/liquid/) esnek web uygulamaları için açık kaynak şablonu dilidir.
 
Bu makalede, sıvı harita veya yineleme, Denetim akışları, değişkenler vb. gibi daha karmaşık JSON dönüşümleri destekleyebilir şablonu nasıl kullanılacağını öğrenin. Bu sıvı eşlemesi ile JSON eşleme için JSON tanımlayın ve mantıksal uygulamanızı sıvı dönüştürme gerçekleştirmeden önce bu Haritası tümleştirme hesabınızda depo gerekir.

## <a name="prerequisites"></a>Ön koşullar
Sıvı eylem kullanmak için Önkoşullar şunlardır:

* Azure aboneliği. Bir aboneliğiniz yoksa [ücretsiz bir Azure hesabı ile başlayabilirsiniz](https://azure.microsoft.com/free/). Ya da [Kullandıkça Öde aboneliğine kaydolabilirsiniz](https://azure.microsoft.com/pricing/purchase-options/).

* Hakkındaki temel bilgileri [logic apps oluşturmak nasıl](../logic-apps/logic-apps-create-a-logic-app.md).

* Temel bir [tümleştirme hesabını](logic-apps-enterprise-integration-create-integration-account.md).


## <a name="create-and-add-liquid-template-or-map-to-integration-account"></a>Oluşturma ve sıvı şablonu eklemek veya tümleştirme hesabına eşleme

1. Bu örnek için örnek sıvı şablon oluşturun. Sıvı şablonu burada açıklandığı gibi JSON girişi dönüştürmek nasıl tanımlar:

   ```
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
    > [!NOTE]
    > Sıvı şablonunuzu herhangi kullanıyorsa [filtreleri](https://shopify.github.io/liquid/basics/introduction/#filters), bu filtreler büyük harf ile başlamalıdır. 

2. [Azure Portal](https://portal.azure.com) oturum açın.

3. Azure ana menüde seçin **tüm kaynakları**. 

4. Arama kutusuna, tümleştirme hesabınızı sağlar. Hesabınızı seçin.

   ![Tümleştirme hesabı seçin](./media/logic-apps-enterprise-integration-liquid-transform/select-integration-account.png)

5.  Tümleştirme hesap kutucuğunu seçin **eşlemeleri**.

   ![Eşlemeleri](./media/logic-apps-enterprise-integration-liquid-transform/add-maps.png)

6. Seçin **Ekle** ve bu bilgi için eşlemenizi sağlar:
  * **Ad**: Bu örnekte "JsontoJsonTemplate" dir haritanızı adı.
  * **Eşleme türü**: haritanızı türü. JSON dönüştürme için JSON seçmelisiniz **sıvı**.
  * **Harita**: Bu örnekte "SimpleJsonToJsonTemplate.liquid" olan bir dönüştürme için kullanılacak bir varolan sıvı şablon ya da eşlem dosyası. Bu dosyayı bulmak için dosya seçiciyi kullanın.

    ![Sıvı şablonu Ekle](./media/logic-apps-enterprise-integration-liquid-transform/add-liquid-template.png)

    
## <a name="add-the-liquid-action-to-transform-json-in-your-logic-app"></a>JSON mantıksal uygulamanızı dönüştürmek için sıvı Eylem Ekle

1. [Mantıksal uygulama oluşturma](logic-apps-create-a-logic-app.md).

2. Ekleme [isteği tetikleyici](../connectors/connectors-native-reqres.md#use-the-http-request-trigger) mantığı uygulamanıza.

3. Seçin **+ yeni adım > Eylem Ekle**. Arama *sıvı* arama kutusuna. Seçin **sıvı - dönüştürme JSON JSON**.

  ![Arama eylemi Sıvı](./media/logic-apps-enterprise-integration-liquid-transform/search-action-liquid.png)

4. İçinde **içerik** kutusunda **gövde** dinamik içerik listesi veya parametre listesi, hangi görüntülenir. 
  
  ![Gövde seçin](./media/logic-apps-enterprise-integration-liquid-transform/select-body.png)
 
5. Gelen **harita** aşağı açılan listesinde, bu örnekte JsonToJsonTemplate olduğundan, sıvı şablonu seçin.

  ![Harita seçin](./media/logic-apps-enterprise-integration-liquid-transform/select-map.png)

   Liste boşsa, mantıksal uygulamanızı olasılıkla tümleştirme hesabınıza bağlı değil. Mantıksal uygulamanızı sıvı şablon ya da eşleme içeren tümleştirme hesabınıza bağlamak için aşağıdaki adımları izleyin:

   1. Mantıksal uygulama menünüzde seçin **iş akışı ayarları**. 
   2. Gelen **tümleştirme hesabı seçme** listesinde, tümleştirme hesabınızı seçin ve **kaydetmek**.

     ![Bağlantı mantıksal uygulama tümleştirmesi hesabı](./media/logic-apps-enterprise-integration-liquid-transform/link-integration-account.png)


## <a name="test-your-logic-app"></a>Mantıksal uygulamanızı test etme

   POST JSON giriş mantığı uygulamanıza [Postman](https://www.getpostman.com/postman) veya benzer bir aracı. Mantıksal uygulamanızı dönüştürülen JSON çıktısını aşağıdaki gibi görünür:
  
   ![Örnek çıktı](./media/logic-apps-enterprise-integration-liquid-transform/example-output.png)


## <a name="next-steps"></a>Sonraki adımlar
* [Enterprise Integration Pack hakkında daha fazla bilgi](../logic-apps/logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack hakkında bilgi edinin")  
* [Eşlemeleri hakkında daha fazla bilgi](../logic-apps/logic-apps-enterprise-integration-maps.md "Kurumsal tümleştirme eşlemeleri hakkında bilgi edinin")  

