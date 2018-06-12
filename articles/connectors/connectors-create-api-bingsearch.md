---
title: Bing arama - Azure mantıksal uygulamaları bağlanma | Microsoft Docs
description: Bing arama REST API'leri ve Azure Logic Apps ile haber bulma
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 05/21/2018
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 87045d5dbbc1221a770e44bd9e9cf2451a9ac522
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35295545"
---
# <a name="find-news-with-bing-search-and-azure-logic-apps"></a>Bing arama ve Azure Logic Apps ile haber bulma 

Bu makalede, haber, videolar ve diğer öğeleri Bing aramadan aracılığıyla Bing arama Bağlayıcısı ile bir mantıksal uygulama içinde nasıl bulabilirsiniz gösterilmektedir. Bu şekilde, görevleri otomatikleştirmek mantıksal uygulamalar oluşturabilir ve işleme için iş akışlarını arama sonuçları ve bu öğeler diğer eylemler için kullanılabilir hale. 

Örneğin, arama ölçütleri temel alarak haber öğeleri bulmak ve Twitter, Twitter tweet'leri akış gibi öğelerin sonrası sahip.

Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>. Logic apps yeniyseniz, gözden [Azure Logic Apps nedir](../logic-apps/logic-apps-overview.md) ve [hızlı başlangıç: ilk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).
Bağlayıcı özgü teknik bilgi için bkz: <a href="https://docs.microsoft.com/connectors/bingsearch/" target="blank">Bing arama bağlayıcı başvurusu</a>.

## <a name="prerequisites"></a>Önkoşullar

* A [Bilişsel hizmetleri hesabı](../cognitive-services/cognitive-services-apis-create-account.md)

* A [Bing arama API anahtarı](https://azure.microsoft.com/try/cognitive-services/?api=bing-news-search-api), Bing arama API'leri mantığı uygulamanızdan erişim sağlar 

* Olay Hub'ınızı erişmek istediğiniz mantıksal uygulama. Bing arama tetikleyici ile mantıksal uygulamanızı başlatmak için gereken bir [boş mantıksal uygulama](../logic-apps/quickstart-create-first-logic-app-workflow.md). 

<a name="add-trigger"></a>

## <a name="add-a-bing-search-trigger"></a>Bing arama tetikleyici Ekle

Azure Logic Apps içinde her mantıksal uygulama başlamalı ve bir [tetikleyici](../logic-apps/logic-apps-overview.md#logic-app-concepts), belirli bir olay olduğunda etkinleşir gerçekleşen veya belirli bir koşul karşılanıyorsa zaman. Her tetikleyici ateşlenir Logic Apps altyapısı bir mantıksal uygulama örneği oluşturur ve uygulamanızın iş akışı çalışmaya başlar.

1. Azure portal ya da Visual Studio mantığı Uygulama Tasarımcısı'nı açar boş mantıksal uygulama oluşturun. Bu örnek, Azure portalını kullanır.

2. Arama kutusuna "Bing arama", filtre olarak girin. Tetikleyiciler listeden istediğiniz Tetikleyici seçin. 

   Bu örnek, bu tetikleyici kullanır: **Bing arama - yeni haber makaleleri**

   ![Bing arama tetikleyici bulma](./media/connectors-create-api-bing-search/add-trigger.png)

3. Bağlantı ayrıntılarını istenirse [şimdi, Bing arama bağlantısı oluşturma](#create-connection). Ya da bağlantı zaten varsa, tetikleyici için gerekli bilgileri sağlayın.

   Bu örnekte, Bing aramadan eşleşen haber makalelerini döndürmek için ölçüt sağlar.

   | Özellik | Gerekli | Değer | Açıklama | 
   |----------|----------|-------|-------------| 
   | Arama Sorgusu | Evet | <*sözcük arama*> | Kullanmak istediğiniz arama anahtar sözcükleri girin. |
   | Market | Evet | <*Yerel ayar*> | Arama yerel ayar. Varsayılan değer, "en-US" olmakla birlikte başka bir değer seçin. | 
   | Güvenli arama | Evet | <*Arama düzeyi*> | Yetişkinlere yönelik içeriğe dışlamak için filtre düzeyi. Varsayılan değer "Orta" dır, ancak başka bir düzeyini seçin. | 
   | Sayı | Hayır | <*sonuçları sayısı*> | Belirtilen sonuç sayısını döndürür. Varsayılan değer 20'dir, ancak başka bir değer belirtebilirsiniz. Gerçek döndürülen sonuç sayısı belirtilen sayıdan daha az olabilir. | 
   | Uzaklık | Hayır | <*atlama değeri*> | Sonuç döndürmeden önce atlamak için sonuç sayısı | 
   ||||| 

   Örneğin:

   ![Tetikleyecek](./media/connectors-create-api-bing-search/bing-search-trigger.png)

4. Aralığı ve sonuçlar için denetlemek için tetikleyici hangi sıklıkta güncelleştirileceğini sıklığını seçin.

5. Tasarımcı araç çubuğunda bitirdiniz seçin **kaydetmek**. 

6. Şimdi mantıksal uygulamanızı tetikleyici sonuçlarıyla gerçekleştirmek istediğiniz görevlerle ilgili bir veya daha fazla eylem ekleme devam edin.

<a name="add-action"></a>

## <a name="add-a-bing-search-action"></a>Bing arama Eylem Ekle

Azure mantıksal uygulamaları içinde bir [eylem](../logic-apps/logic-apps-overview.md#logic-app-concepts) tetikleyicinin veya başka bir eylem izler, iş akışınızı bir adımdır. Bu örnekte, mantıksal uygulama belirtilen ölçütlerle eşleşen haber makalelerini döndüren Bing arama tetikleyici ile başlar. 

1. Azure portal ya da Visual Studio mantığı Uygulama Tasarımcısı'nda mantıksal uygulamanızı açın. Bu örnek, Azure portalını kullanır.

2. Tetikleyici veya eylem altında seçin **yeni adım** > **Eylem Ekle**.

   Bu örnek, bu tetikleyici kullanır: **Bing arama - yeni haber makaleleri**

   ![Eylem Ekle](./media/connectors-create-api-bing-search/add-action.png)

   Varolan adımlar arasındaki bir eylem eklemek amacıyla bağlanan oku fareyi hareket ettirin. 
   Artı işaretini seçin (**+**) görünür ve ardından **Eylem Ekle**.

3. Arama kutusuna "Bing arama", filtre olarak girin.
Eylemler listesinden istediğiniz eylemi seçin.

   Bu eylem Bu örnek kullanır: **Bing arama - liste haberleri sorgu tarafından**

   ![Bing arama eylem bulunamıyor](./media/connectors-create-api-bing-search/bing-search-select-action.png)

4. Bağlantı ayrıntılarını istenirse [şimdi, Bing arama bağlantısı oluşturma](#create-connection). Ya da bağlantı zaten varsa, eylem için gerekli bilgileri sağlayın. 

   Bu örnekte, bir alt kümesini tetikleyici sonuçları döndürmek için ölçütleri belirtin.

   | Özellik | Gerekli | Değer | Açıklama | 
   |----------|----------|-------|-------------| 
   | Arama Sorgusu | Evet | <*Arama ifadesi*> | Tetikleyici sonuçları sorgulama için bir ifade girin. Dinamik içerik listesi alanları om seçin veya bir ifade ifade Oluşturucu ile oluşturun. |
   | Market | Evet | <*Yerel ayar*> | Arama yerel ayar. Varsayılan değer, "en-US" olmakla birlikte başka bir değer seçin. | 
   | Güvenli arama | Evet | <*Arama düzeyi*> | Yetişkinlere yönelik içeriğe dışlamak için filtre düzeyi. Varsayılan değer "Orta" dır, ancak başka bir düzeyini seçin. | 
   | Sayı | Hayır | <*sonuçları sayısı*> | Belirtilen sonuç sayısını döndürür. Varsayılan değer 20'dir, ancak başka bir değer belirtebilirsiniz. Gerçek döndürülen sonuç sayısı belirtilen sayıdan daha az olabilir. | 
   | Uzaklık | Hayır | <*atlama değeri*> | Sonuç döndürmeden önce atlamak için sonuç sayısı | 
   ||||| 

   Örneğin, kategori adı "teknik" sözcüğünü içeren bu sonuçlar istediğinizi varsayalım. 
   
   1. ' I tıklatın **arama sorgusu** dinamik içerik listesi görünmesi kutusu. 
   Bu listeden seçin **ifade** nedenle ifade oluşturucusu görünür. 

      ![Bing arama tetikleyici](./media/connectors-create-api-bing-search/bing-search-action.png)

      Şimdi, ifadesi oluşturma başlatabilirsiniz.

   2. İşlevler listesinden **contains()** ifade kutusunda görünür işlevi. Tıklatın **dinamik içerik** böylece alan listesi görüntülenir, ancak emin olun, imlecinizi parantez içinde kalır.

      ![Bir işlevi seçin](./media/connectors-create-api-bing-search/expression-select-function.png)

   3. Alan listesinden **kategori**, bir parametre dönüştürür. 
   İlk parametre ve virgül sonrasında virgül eklemek için bu word ekleyin: `'tech'` 

      ![Bir alan seçin](./media/connectors-create-api-bing-search/expression-select-field.png)
   
   4. İşiniz bittiğinde seçin **Tamam**.

      İfade artık görünür **arama sorgusu** bu biçimde kutusunda:

      ![Tamamlanmış ifade](./media/connectors-create-api-bing-search/resolved-expression.png)

      Kod görünümünde Bu ifade şöyle görünür:

      `"@{contains(triggerBody()?['category'],'tech')}"`

5. Tasarımcı araç çubuğunda bitirdiniz seçin **kaydetmek**.

<a name="create-connection"></a>

## <a name="connect-to-bing-search"></a>Bing arama Bağlan

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)] 

1. Bağlantı bilgilerini istendiğinde, bu bilgileri sağlayın:

   | Özellik | Gerekli | Değer | Açıklama | 
   |----------|----------|-------|-------------| 
   | Bağlantı Adı | Evet | <*Bağlantı adı*> | Ad, bağlantı oluşturmak için |
   | API Sürümü | Evet | <*API sürümü*> | Varsayılan olarak, Bing arama API sürümü geçerli sürüme ayarlanır. Önceki bir sürümünü gerektiği şekilde seçebilirsiniz. | 
   | API Anahtarı | Evet | <*API anahtarı*> | Daha önce aldığınız Bing arama API anahtarı. Bir anahtar yoksa, alma, [şimdi API anahtarı](https://azure.microsoft.com/try/cognitive-services/?api=bing-news-search-api). |  
   |||||  

   Örneğin:

   ![Bağlantı oluşturma](./media/connectors-create-api-bing-search/bing-search-create-connection.png)

2. İşiniz bittiğinde **Oluştur**’u seçin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcı'nın Swagger dosyası tarafından açıklandığı gibi tetikleyiciler, Eylemler ve sınırları, gibi teknik ayrıntılar için bkz [bağlayıcı başvuru sayfası](/connectors/bingsearch/). 

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](http://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında bilgi edinin [Logic Apps bağlayıcılar](../connectors/apis-list.md)