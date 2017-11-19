---
title: "Dönüşümler - Azure Logic Apps ile XML verileri dönüştürmek | Microsoft Docs"
description: "Dönüşümler veya XML verileri Kurumsal tümleştirme SDK'sını kullanarak logic apps içinde biçimleri arasında dönüştürme mapps oluşturma"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: add01429-21bc-4bab-8b23-bc76ba7d0bde
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: f09819a1bfd380cd826a478471e673b6d5ff9ee7
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="enterprise-integration-with-xml-transforms"></a>XML dönüşümler ile Kurumsal tümleştirme
## <a name="overview"></a>Genel Bakış
Kurumsal tümleştirme dönüştürme bağlayıcı verileri bir biçimden başka bir biçime dönüştürür. Örneğin, YearMonthDay biçiminde geçerli tarihi içeren bir gelen ileti olabilir. Bir dönüştürme tarihi MonthDayYear biçiminde yeniden biçimlendirmek için kullanabilirsiniz.

## <a name="what-does-a-transform-do"></a>Bir dönüştürme ne yapar?
Kaynak XML Şeması (giriş) ve hedef XML Şeması (çıktı) olarak da bilinen bir eşleme olduğundan, bir dönüşüm oluşur. Farklı yerleşik işlevler yönetmek veya veri denetimi dize işlemeleri, koşullu atamaları, aritmetik ifadeler, tarih saat biçimlendiricileri dahil olmak üzere ve hatta döngü yapıları yardımcı olmak için kullanabilirsiniz.

## <a name="how-to-create-a-transform"></a>Bir dönüştürme oluşturmak nasıl?
Visual Studio kullanarak bir dönüşüm/eşlemesi oluşturabilirsiniz [Kurumsal tümleştirme SDK](https://aka.ms/vsmapsandschemas). Oluşturma ve dönüştürme test bittiğinde tümleştirme hesabınızda dönüştürme karşıya yükleyin. 

## <a name="how-to-use-a-transform"></a>Bir dönüşüm kullanma
Dönüşüm/map tümleştirme hesabınızda yükledikten sonra bir mantıksal uygulama oluşturmak için kullanabilirsiniz. Mantıksal uygulama, dönüştürmeleri mantıksal uygulama tetiklenir (ve dönüştürülmesi için gereken Giriş bir içerik olduğunda) çalışır.

**Bir dönüştürme kullanmak için adımlar şunlardır**:

### <a name="prerequisites"></a>Ön koşullar

* Bir tümleştirme hesabı oluşturun ve bir harita ekleyin  

Önkoşulların yapılan verdiğiniz olduğunuza mantıksal uygulamanızı oluşturmak için zamanı:  

1. Mantıksal uygulama oluşturma ve [tümleştirme hesabınıza bağlamak](../logic-apps/logic-apps-enterprise-integration-accounts.md "bir mantıksal uygulama için bir tümleştirme hesabı bağlamak bilgi") eşlemesi içerir.
2. Ekleme bir **isteği** mantıksal uygulamanızı tetikleyiciye  
   ![](./media/logic-apps-enterprise-integration-transforms/transform-1.png)    
3. Ekle **dönüştürme XML** ilk seçerek eylemi **Eylem Ekle**   
   ![](./media/logic-apps-enterprise-integration-transforms/transform-2.png)   
4. Sözcüğü girin *dönüştürme* kullanmak istediğiniz bir tüm eylemler filtrelemek için arama kutusuna  
   ![](./media/logic-apps-enterprise-integration-transforms/transform-3.png)  
5. Seçin **XML dönüştürme** eylem   
6. XML'in **içerik** , dönüştüren. HTTP isteği aldığınız herhangi bir XML veri kullanabilirsiniz **içerik**. Bu örnekte, mantıksal uygulama tetiklenen HTTP istek gövdesi seçin.

   > [!NOTE]
   > Emin olmak için içeriği **XML dönüştürme** XML. İçerik XML biçiminde değil veya base64 ile kodlanmış, içeriği işleyen bir ifade belirtmeniz gerekir. Örneğin, kullanabileceğiniz [işlevleri](logic-apps-workflow-definition-language.md#functions)gibi ```@base64ToBinary``` içerik kod çözme veya ```@xml``` içeriği XML olarak işlemek için.
 

7. Adını seçin **harita** dönüştürme gerçekleştirmek için kullanmak istediğiniz. Harita zaten tümleştirme hesabınız olması gerekir. Bir önceki adımda, zaten haritanızı içeren tümleştirme hesabınıza mantığı uygulama erişimi vermiş.      
   ![](./media/logic-apps-enterprise-integration-transforms/transform-4.png) 
8. Çalışmanızı kaydedin  
    ![](./media/logic-apps-enterprise-integration-transforms/transform-5.png) 

Bu noktada, map ayarlama tamamlandı. Gerçek dünya uygulamada bir LOB uygulaması SalesForce gibi dönüştürülmüş verilerin depolamak isteyebilirsiniz. Kolayca dönüşümün çıktısını Salesforce göndermek için bir eylem olarak belirtebilirsiniz. 

Artık HTTP uç noktası için bir istekte, dönüştürme test edebilirsiniz.  

## <a name="features-and-use-cases"></a>Özellikleri ve kullanım örnekleri
* Bir harita oluşturulan dönüşüm bir ad ve adres bir belgeden diğerine kopyalama gibi basit olabilir. Veya, Giden kutusu eşleme işlemleri kullanarak daha karmaşık dönüşümleri oluşturabilirsiniz.  
* Birden çok eşleme işlemleri veya İşlevler dizeler, tarih saat işlevleri vb. dahil olmak üzere kullanıma hazır.  
* Şemalar arasında doğrudan veri kopyalama yapabilirsiniz. SDK'da bulunan eşleyicisinde bu dekiler hedef şemasında ile kaynak şema öğeleri bağlayan bir çizgi çizme olarak kadar basittir.  
* Bir harita oluştururken, tüm ilişkileri ve oluşturduğunuz bağlantılarını gösterir harita grafik gösterimi görüntüleyin.
* Örnek bir XML ileti eklemek için Test eşleme özelliğini kullanın. Bir basit tıklamayla oluşturduğunuz haritasını sınamak ve üretilen çıktıyı görürsünüz.  
* Var olan eşlemeleri karşıya yükle  
* XML biçimi için destek içerir.

## <a name="learn-more"></a>Daha fazla bilgi edinin
* [Enterprise Integration Pack hakkında daha fazla bilgi](../logic-apps/logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack hakkında bilgi edinin")  
* [Eşlemeleri hakkında daha fazla bilgi](../logic-apps/logic-apps-enterprise-integration-maps.md "Kurumsal tümleştirme eşlemeleri hakkında bilgi edinin")  

