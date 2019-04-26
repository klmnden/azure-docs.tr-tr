---
title: XML - Azure Logic Apps biçimleri arasında dönüştürme | Microsoft Docs
description: Dönüşümler ya da Azure Logic Apps Enterprise Integration Pack ile biçimlerde arasında XML dönüştürme eşlemeleri oluşturma
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: add01429-21bc-4bab-8b23-bc76ba7d0bde
ms.date: 07/08/2016
ms.openlocfilehash: 4ebd96613378bbd907beb5109343a2427b1300b0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60427287"
---
# <a name="create-maps-that-transform-xml-between-formats-in-azure-logic-apps-with-enterprise-integration-pack"></a>XML Azure Logic Apps Enterprise Integration Pack ile biçimlerde arasındaki dönüştürme eşlemeleri oluşturma

Kurumsal tümleştirme dönüştürme Bağlayıcısı verileri bir biçimden başka bir biçime dönüştürür. Örneğin, YearMonthDay biçiminde geçerli tarihi içeren gelen ileti olabilir. Bir dönüştürme tarihi MonthDayYear biçimde yeniden biçimlendirmek için kullanabilirsiniz.

## <a name="what-does-a-transform-do"></a>Dönüşüm ne yapar?
Olarak da bilinen bir eşlemesi olduğundan, bir dönüştürme kaynak XML Şeması (giriş) ve hedef XML Şeması (çıkış) oluşur. Farklı yerleşik işlevler, yönetmek veya veri denetimi gibi dize işlemeleri, koşullu atamaları, aritmetik ifadeler, tarih saat biçimlendiricileri ve hatta döngü yapıları yardımcı olmak için kullanabilirsiniz.

## <a name="how-to-create-a-transform"></a>Dönüşümü oluşturmak nasıl?
Visual Studio kullanarak bir dönüşüm/map oluşturabilirsiniz [Kurumsal tümleştirme SDK'sı](https://aka.ms/vsmapsandschemas). Oluşturma ve sınama dönüştürme işlemini tamamladığınızda dönüştürme tümleştirme hesabınıza yükleyin. 

## <a name="how-to-use-a-transform"></a>Dönüşüm kullanma
Tümleştirme hesabınızda dönüştürme/map karşıya yüklenmesinin ardından, mantıksal uygulama oluşturmak için kullanabilirsiniz. Mantıksal uygulama, mantıksal uygulama tetiklenir (ve dönüştürülmesi için gereken giriş içerik olduğunda) Bağlantılarınızdaki çalıştırır.

**Dönüşüm kullanmak için adımlar şunlardır**:

### <a name="prerequisites"></a>Önkoşullar

* Tümleştirme hesabı oluşturma ve bir eşleme ekleyin  

Geçen Önkoşullar sizin göre, mantıksal uygulamanızı oluşturmak için zaman verilmiştir:  

1. Mantıksal uygulama oluşturma ve [tümleştirme hesabınıza bağlayın](../logic-apps/logic-apps-enterprise-integration-accounts.md "öğrenmek için mantıksal uygulama tümleştirme hesabı bağlamak") harita içeren.
2. Ekleme bir **istek** mantıksal uygulamanızın tetikleyicisi  
   ![](./media/logic-apps-enterprise-integration-transforms/transform-1.png)    
3. Ekle **XML dönüştürme** ilk seçerek eylem **Eylem Ekle**   
   ![](./media/logic-apps-enterprise-integration-transforms/transform-2.png)   
4. Sözcük girin *dönüştürme* kullanmak istediğiniz bir tüm eylemleri filtrelemek için arama kutusuna  
   ![](./media/logic-apps-enterprise-integration-transforms/transform-3.png)  
5. Seçin **XML dönüştürme** eylemi   
6. XML'in **içerik** , dönüştüren. HTTP isteği aldığınız herhangi bir XML veri kullanabileceğiniz **içerik**. Bu örnekte, mantıksal uygulama tetiklenir HTTP isteği gövdesinin seçin.

   > [!NOTE]
   > Emin olun içeriğini **XML dönüştürme** XML'dir. İçerik XML içinde değil veya base64 ile kodlanmış içeriği işleyen bir ifade belirtmeniz gerekir. Örneğin, kullanabileceğiniz [işlevleri](logic-apps-workflow-definition-language.md#functions)gibi ```@base64ToBinary``` içeriği kod çözme için veya ```@xml``` içeriği XML olarak işlemek için.
 

7. Adını seçin **harita** dönüştürmeyi gerçekleştirmek için kullanmak istediğiniz. Harita, tümleştirme hesabında zaten olması gerekir. Daha önceki bir adımda zaten haritanızı içeren, tümleştirme hesabı, mantıksal uygulama erişimi getirdi.      
   ![](./media/logic-apps-enterprise-integration-transforms/transform-4.png) 
8. Çalışmanızı kaydedin  
    ![](./media/logic-apps-enterprise-integration-transforms/transform-5.png) 

Bu noktada, map ayarlama tamamlandı. Gerçek bir uygulamada, SalesForce gibi bir LOB uygulaması dönüştürülmüş verileri depolamak isteyebilirsiniz. Kolayca Salesforce'a dönüşümün çıkış göndermek için bir eylem olarak belirtebilirsiniz. 

Artık HTTP uç noktası için bir istekte, dönüştürme test edebilirsiniz.  


## <a name="features-and-use-cases"></a>Özellikler ve kullanım örnekleri
* Bir eşlem içinde oluşturulan dönüştürme bir ad ve adres bir belgeden diğerine kopyalama gibi basit olabilir. Veya, kullanıma hazır eşleme işlemleri kullanarak daha karmaşık dönüştürmeler oluşturabilirsiniz.  
* Birden çok eşleme işlemleri veya işlevleri dizeleri, tarih saat işlevleri ve benzeri gibi kullanıma hazır.  
* Şemalar arasında doğrudan veri kopyasını yapabilirsiniz. SDK'da bulunan eşleyicisinde karşılıkları hedef şema ile kaynak şema öğeleri bağlayan bir çizgi çizme olarak basit budur.  
* Bir eşleme oluştururken, tüm bağlantıları, oluşturma ve ilişkileri gösteren haritayı grafik gösterimi görüntüleyin.
* Örnek XML iletisi eklemek için testi Haritası özelliğini kullanın. Bir basit tıklamayla oluşturduğunuz harita test edin ve oluşturulan çıktıyı görürsünüz.  
* Var olan eşlemeleri karşıya yükleme  
* XML biçimi için destek içerir.

## <a name="advanced-features"></a>Gelişmiş özellikler

### <a name="reference-assembly-or-custom-code-from-maps"></a>Başvuru bütünleştirilmiş kodu veya özel kod eşlemeleri 
Dönüştürme eylem ayrıca haritalarını destekler veya başvuru içeren dış bütünleştirilmiş dönüştürür. Bu özellik, XSLT eşlemeleri doğrudan çağrıları özel .NET kodu için sağlar. Maps'a derleme kullanmak için Önkoşullar aşağıda verilmiştir.

* Harita ve harita gereksinimlerini olmasını başvurduğu derlemenin [tümleştirme hesabına yüklediniz](./logic-apps-enterprise-integration-maps.md). 

  > [!NOTE]
  > Belirli bir sırada yüklenecek Haritası ve derleme gereklidir. Derlemenin başvurduğu harita karşıya yüklemeden önce derlemeyi yüklemeniz gerekir.

* Harita, bu öznitelikler ve derleme koduna çağrı içeren CDATA bölümü de sahip olmanız gerekir:

    * **adı** özel bütünleştirilmiş kod adı.
    * **ad alanı** özel kodunu içeren bütünleştirilmiş kodunuzda ad alanındadır.

  Bu örnek, çağrı "XslUtilitiesLib" adlı bir derlemeye başvuran bir harita gösterir `circumreference` derlemesinden yöntemi.

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform" xmlns:msxsl="urn:schemas-microsoft-com:xslt" xmlns:user="urn:my-scripts">
  <msxsl:script language="C#" implements-prefix="user">
    <msxsl:assembly name="XsltHelperLib"/>
    <msxsl:using namespace="XsltHelpers"/>
    <![CDATA[public double circumference(int radius){ XsltHelper helper = new XsltHelper(); return helper.circumference(radius); }]]>
  </msxsl:script>
  <xsl:template match="data">
     <circles>
        <xsl:for-each select="circle">
            <circle>
                <xsl:copy-of select="node()"/>
                    <circumference>
                        <xsl:value-of select="user:circumference(radius)"/>
                    </circumference>
            </circle>
        </xsl:for-each>
     </circles>
    </xsl:template>
    </xsl:stylesheet>
  ```


### <a name="byte-order-mark"></a>Bayt sırası işareti
Varsayılan olarak, dönüştürme yanıttan bayt sırası işareti (BOM) ile başlar. Kod Görünümü düzenleyicide çalışırken bu işlevselliğe erişebilirsiniz. Bu işlev devre dışı bırakmak için belirtin `disableByteOrderMark` için `transformOptions` özelliği:

```json
"Transform_XML": {
    "inputs": {
        "content": "@{triggerBody()}",
        "integrationAccount": {
            "map": {
                "name": "TestMap"
            }
        },
        "transformOptions": "disableByteOrderMark"
    },
    "runAfter": {},
    "type": "Xslt"
}
```





## <a name="learn-more"></a>Daha fazla bilgi edinin
* [Enterprise Integration Pack hakkında daha fazla bilgi](../logic-apps/logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack hakkında bilgi edinin")  
* [Eşlemeleri hakkında daha fazla bilgi](../logic-apps/logic-apps-enterprise-integration-maps.md "Kurumsal tümleştirme eşlemeleri hakkında bilgi edinin")  

