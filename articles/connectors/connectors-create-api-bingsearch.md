---
title: Bing arama - Azure Logic Apps'i bağlama
description: Bing arama REST API'leri ve Azure Logic Apps ile haber bulma
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 05/21/2018
tags: connectors
ms.openlocfilehash: 7146e59eabf9e30fa263f957f1c546414ad0fe26
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58313558"
---
# <a name="find-news-with-bing-search-and-azure-logic-apps"></a>Bing arama ve Azure Logic Apps ile haber bulma

Bu makalede, Haberler, videolar ve diğer öğeleri Bing arama Bing arama Bağlayıcısı ile bir mantıksal uygulama içinde nasıl bulabilirsiniz gösterilmektedir. Bu şekilde, görevleri otomatik hale getiren mantıksal uygulamaları oluşturabilir ve işleme iş akışları arama sonuçları ve bu öğeler diğer eylemler için kullanılabilir hale getirir. 

Örneğin, arama ölçütlerine dayanarak haber öğelerini bulup tweetleri Twitter akışı gibi öğelerin sonrası Twitter sahip.

Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>. Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir](../logic-apps/logic-apps-overview.md) ve [hızlı başlangıç: İlk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).
Bağlayıcısı özel teknik bilgiler için bkz. <a href="https://docs.microsoft.com/connectors/bingsearch/" target="blank">Bing arama bağlayıcı başvurusu</a>.

## <a name="prerequisites"></a>Önkoşullar

* A [Bilişsel Hizmetler hesabı](../cognitive-services/cognitive-services-apis-create-account.md)

* A [Bing arama API'si anahtarı](https://azure.microsoft.com/try/cognitive-services/?api=bing-news-search-api), Bing arama API'leri, mantıksal uygulamadan erişim sağlar

* Olay Hub'ınıza erişmek istediğiniz mantıksal uygulaması. Bing arama tetikleyici ile mantıksal uygulamanızı başlatmak için gereken bir [boş mantıksal uygulama](../logic-apps/quickstart-create-first-logic-app-workflow.md).

<a name="add-trigger"></a>

## <a name="add-a-bing-search-trigger"></a>Bing arama tetikleyici ekleyin

Azure Logic Apps'te, her mantıksal uygulama ile başlamalıdır bir [tetikleyici](../logic-apps/logic-apps-overview.md#logic-app-concepts), belirli bir olay harekete geçirilir gerçekleşen veya belirli bir koşul karşılanıyorsa zaman. Her zaman tetikleyici Logic Apps altyapısı bir mantıksal uygulama örneği oluşturur ve uygulamanızın iş akışı çalışmaya başlar.

1. Azure portalı ya da Visual Studio, mantıksal Uygulama Tasarımcısı açılır bir boş mantıksal uygulama oluşturun. Bu örnek, Azure portalını kullanır.

2. Arama kutusuna filtreniz olarak "Bing arama" girin. Tetikleyiciler listesinden istediğiniz tetikleyicisini seçin.

   Bu örnek, bu tetikleyici kullanır: **Bing arama - yeni haber makaleleri**

   ![Bing arama tetikleyici Bul](./media/connectors-create-api-bing-search/add-trigger.png)

3. Bağlantı ayrıntıları için istenirse [şimdi Bing arama bağlantınızı oluşturmak](#create-connection).
Veya, bağlantınız zaten varsa, tetikleyici için gerekli bilgileri sağlayın.

   Bu örnekte, Bing arama eşleşen haber makalelerini döndürmek için ölçütleri sağlayın.

   | Özellik | Gereklidir | Value | Açıklama |
   |----------|----------|-------|-------------|
   | Search Query | Evet | <*search-words*> | Kullanmak istediğiniz arama anahtar sözcükleri girin. |
   | Market | Evet | <*Yerel ayar*> | Arama yerel ayar. Varsayılan "en-US" olduğu, ancak başka bir değer seçebilirsiniz. |
   | Safe Search | Evet | <*Arama düzeyi*> | Yetişkinlere yönelik içeriğe Dışlama filtresi düzeyi. Varsayılan "Orta" dır, ancak başka bir düzeyi seçin. |
   | Count | Hayır | <*sonuç sayısı*> | Belirtilen sonuç sayısını döndürür. Varsayılan değer 20'dir, ancak başka bir değer belirtebilirsiniz. Döndürülen sonuç gerçek sayı belirtilen sayıdan daha az olabilir. |
   | Offset | Hayır | <*atlama değeri*> | Sonuç döndürülmeden önce atlanacak sonuç sayısı |
   |||||

   Örneğin:

   ![Tetikleyici ayarlayın](./media/connectors-create-api-bing-search/bing-search-trigger.png)

4. Aralığı ve sonuçlar için denetlenecek tetikleyici hangi sıklıkta güncelleştirileceğini sıklığını seçin.

5. Tasarımcı araç çubuğunda, işiniz bittiğinde seçin **Kaydet**.

6. Şimdi mantıksal uygulamanız tetikleme sonuçlarıyla gerçekleştirmek istediğiniz görevleri için bir veya daha fazla eylem ekleyerek devam edin.

<a name="add-action"></a>

## <a name="add-a-bing-search-action"></a>Bing arama eylemi ekleme

Azure Logic apps'te bir [eylem](../logic-apps/logic-apps-overview.md#logic-app-concepts) akışınıza bir tetikleyici veya başka bir eylem izleyen bir adımdır. Bu örnekte, mantıksal uygulama belirtilen ölçütlerle eşleşen haber makalelerini döndüren bir Bing arama tetikleyici ile başlar.

1. Azure portalı ya da Visual Studio, mantıksal uygulamanızı Logic App Tasarımcısı'nda açın. Bu örnek, Azure portalını kullanır.

2. Tetikleyici veya eylem altında seçin **yeni adım** > **Eylem Ekle**.

   Bu örnek, bu tetikleyici kullanır:

   **Bing arama - yeni haber makaleleri**

   ![Eylem ekle](./media/connectors-create-api-bing-search/add-action.png)

   Var olan adımlar arasında bir eylem eklemek için bağlantı okun üzerine fareyi hareket ettirin. 
   Artı işaretini seçin (**+**), görünür ve ardından **Eylem Ekle**.

3. Arama kutusuna filtreniz olarak "Bing arama" girin.
Eylem listesinden istediğiniz eylemi seçin.

   Bu örnek, bu eylem kullanır:

   **Bing arama - sorgu tarafından listesi Haberleri**

   ![Bing arama eylemi](./media/connectors-create-api-bing-search/bing-search-select-action.png)

4. Bağlantı ayrıntıları için istenirse [şimdi Bing arama bağlantınızı oluşturmak](#create-connection). Veya, bağlantınız zaten varsa, eylem için gerekli bilgileri sağlayın.

   Bu örnekte, bir alt kümesini tetikleyicinin sonuçları döndürmek için ölçütleri sağlayın.

   | Özellik | Gereklidir | Value | Açıklama |
   |----------|----------|-------|-------------|
   | Search Query | Evet | <*search-expression*> | Tetikleyici sonuçlarını sorgulama için bir ifade girin. Dinamik içerik listesindeki alanları seçin veya bir ifade ifade Oluşturucu ile oluşturun. |
   | Market | Evet | <*Yerel ayar*> | Arama yerel ayar. Varsayılan "en-US" olduğu, ancak başka bir değer seçebilirsiniz. |
   | Safe Search | Evet | <*Arama düzeyi*> | Yetişkinlere yönelik içeriğe Dışlama filtresi düzeyi. Varsayılan "Orta" dır, ancak başka bir düzeyi seçin. |
   | Count | Hayır | <*sonuç sayısı*> | Belirtilen sonuç sayısını döndürür. Varsayılan değer 20'dir, ancak başka bir değer belirtebilirsiniz. Döndürülen sonuç gerçek sayı belirtilen sayıdan daha az olabilir. |
   | Offset | Hayır | <*atlama değeri*> | Sonuç döndürülmeden önce atlanacak sonuç sayısı |
   |||||

   Örneğin, kategori adları "teknik" sözcüğünü içeren sonuçları istediğinizi varsayalım.

   1. Tıklayın **arama sorgusu** dinamik içerik listesinde görünecek şekilde kutusu. 
   Bu listeden seçin **ifade** ifade oluşturucu görünecek şekilde. 

      ![Bing arama tetikleyicisi](./media/connectors-create-api-bing-search/bing-search-action.png)

      Şimdi ifadeniz oluşturmaya başlayabilirsiniz.

   2. İşlevleri listesinden **contains()** ardından ifade kutusunda görünen işlev. Tıklayın **dinamik içerik** böylece alan listesinde görüntülenir, ancak emin olun, imlecinizi parantez içinde kalır.

      ![Bir işlev seçin](./media/connectors-create-api-bing-search/expression-select-function.png)

   3. Alan listesinden **kategori**, bir parametre dönüştürür. 
   İlk parametre ve virgülden sonra bir virgül ekleyin, bu sözcüğü ekleyin: `'tech'` 

      ![Bir alan seçin](./media/connectors-create-api-bing-search/expression-select-field.png)

   4. İşiniz bittiğinde seçin **Tamam**.

      İfadenin artık görünür **arama sorgusu** şu biçimde kutusunda:

      ![Tamamlanmış ifadesi](./media/connectors-create-api-bing-search/resolved-expression.png)

      Kod Görünümü'nde, bu ifade şu biçimde görünür:

      `"@{contains(triggerBody()?['category'],'tech')}"`

5. Tasarımcı araç çubuğunda, işiniz bittiğinde seçin **Kaydet**.

<a name="create-connection"></a>

## <a name="connect-to-bing-search"></a>Bing arama bağlanma

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Bağlantı bilgileri sorulduğunda, bu ayrıntıları sağlayın:

   | Özellik | Gereklidir | Value | Açıklama |
   |----------|----------|-------|-------------|
   | Bağlantı Adı | Evet | <*Bağlantı adı*> | Adı, bağlantı oluşturmak için |
   | API Sürümü | Evet | <*API sürümü*> | Varsayılan olarak, Bing arama API'si sürümü geçerli sürüme ayarlanır. Gerektiğinde daha önceki bir sürümü seçebilirsiniz. |
   | API Anahtarı | Evet | <*API anahtarı*> | Daha önce aldığınız Bing arama API'si anahtarı. Bir anahtar yoksa, alma, [artık API anahtarı](https://azure.microsoft.com/try/cognitive-services/?api=bing-news-search-api). |  
   |||||  

   Örneğin:

   ![Bağlantı oluşturma](./media/connectors-create-api-bing-search/bing-search-create-connection.png)

2. İşiniz bittiğinde **Oluştur**’u seçin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler ve Eylemler sınırları, bağlayıcının Openapı'nin açıklandığı gibi teknik ayrıntılar için (önceki adıyla Swagger) dosyası, bkz: [bağlayıcının başvuru sayfası](/connectors/bingsearch/).

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)
