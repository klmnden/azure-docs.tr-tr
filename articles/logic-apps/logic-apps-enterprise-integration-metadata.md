---
title: Tümleştirme hesabı yapıt meta verileri - Azure Logic Apps yönetme | Microsoft Docs
description: Ekleme veya Azure Logic Apps Enterprise Integration Pack ile tümleştirme hesapları yapıt meta verileri alma
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.date: 01/17/2019
ms.openlocfilehash: 5ebdf45bec4e7cfceb75354af40c7a21c22c6eef
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60846234"
---
# <a name="manage-artifact-metadata-in-integration-accounts-with-azure-logic-apps-and-enterprise-integration-pack"></a>Azure Logic Apps ve Enterprise Integration Pack ile tümleştirme hesaplarındaki yapıt meta verileri yönetme

Tümleştirme hesapları yapıtlar için özel meta verileri tanımlamak ve kullanılacak mantıksal uygulamanızın çalışma zamanı sırasında bu meta verilerini al. Örneğin, meta verileri, iş ortakları, sözleşmeler, şemalar ve haritalar - gibi yapıtlar için anahtar-değer çiftleri kullanarak tüm depolama meta verileri sağlayabilirsiniz. 

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Aboneliğiniz yoksa, <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>.

* Temel bir [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) meta verileri, örneğin eklemek istediğiniz yapıtlar vardır: 

  * [İş ortağı](logic-apps-enterprise-integration-partners.md)
  * [Anlaşma](logic-apps-enterprise-integration-agreements.md)
  * [Şema](logic-apps-enterprise-integration-schemas.md)
  * [Harita](logic-apps-enterprise-integration-maps.md)

* Kullanmak istediğiniz bir mantıksal uygulama tümleştirme hesabı ve yapıt meta verilere bağlıdır. Mantıksal uygulamanız zaten bağlı değilse, bilgi [tümleştirme hesapları için logic apps'i bağlama](logic-apps-enterprise-integration-create-integration-account.md#link-account). 

  Mantıksal uygulama henüz sahip değilseniz, bilgi [mantıksal uygulamalar oluşturmak nasıl](../logic-apps/quickstart-create-first-logic-app-workflow.md). 
  Tetikleyici ve Eylemler yapıt meta verileri yönetmek için kullanmak istediğiniz ekleyin. Veya yalnızca kendimiz denemek için bir tetikleyici gibi ekleme **istek** veya **HTTP** mantıksal uygulamanız için.

## <a name="add-metadata-to-artifacts"></a>Yapıtlar için meta verileri ekleme

1. Azure hesabınızın kimlik bilgileriyle <a href="https://portal.azure.com" target="_blank">Azure portalında</a> oturum açın. Bulun ve tümleştirme hesabınızı açın.

1. Seçmek istediğiniz meta verileri ekleyin ve yapı **Düzenle**. Meta veri ayrıntıları bu yapıt için örneğin girin:

   ![Meta verileri girin](media/logic-apps-enterprise-integration-metadata/add-partner-metadata.png)

1. İşiniz bittiğinde seçin **Tamam**.

1. Tümleştirme hesabı için JavaScript nesne gösterimi (JSON) tanımında bu meta verileri görüntülemek için seçin **JSON olarak Düzenle** böylece JSON Düzenleyicisi açılır: 

   ![İş ortağı meta veriler için JSON](media/logic-apps-enterprise-integration-metadata/partner-metadata.png)

## <a name="get-artifact-metadata"></a>Yapıt meta verilerini al

1. İstediğiniz tümleştirme hesabına bağlı olduğunda mantıksal uygulama Azure Portalı'nda açın. 

1. Logic Apps Tasarımcısı'nda iş akışında tetikleyici veya son eylem altında meta veri alma adımı ekliyorsanız seçin **yeni adım** > **Eylem Ekle**. 

1. Arama kutusuna "tümleştirme hesabı" girin. Arama kutusunun altındaki seçin **tüm**. Eylem listesinden şu eylemi seçin: **Tümleştirme hesabı Yapıt arama - tümleştirme hesabı**

   !["Tümleştirme hesabı Yapıt arama" seçin](media/logic-apps-enterprise-integration-metadata/integration-account-artifact-lookup.png)

1. Bulmak istediğiniz bu bilgileri yapıtı sağlayın:

   | Özellik | Gereklidir | Value | Açıklama | 
   |----------|---------|-------|-------------| 
   | **Yapıt türü** | Evet | **Şema**, **harita**, **iş ortağı**, **sözleşmesi**, ya da özel bir tür | İstediğiniz yapıt türü | 
   | **Yapıt adı** | Evet | <*yapıt adı*> | İstediğiniz yapıt adı | 
   ||| 

   Örneğin, bir ticaret iş ortağı yapıt meta verilerini almak istediğinizi varsayalım:

   ![Yapıt türü seçin ve yapıt adı girin](media/logic-apps-enterprise-integration-metadata/artifact-lookup-information.png)

1. Örneğin, meta verilerin işlenmesi için istediğiniz eylemi ekleyin:

   1. Altında **tümleştirme hesabı Yapıt arama** eylemi seçin **sonraki adım**seçip **Eylem Ekle**. 

   1. Arama kutusuna "http" girin. Arama kutusunun altındaki seçin **yerleşik olanları**ve şu eylemi seçin: **HTTP - HTTP**

      ![HTTP Eylem Ekle](media/logic-apps-enterprise-integration-metadata/http-action.png)

   1. Yapıt meta verileri için yönetmek istediğiniz bilgileri sağlayın. 

      Örneğin, almak istediğiniz varsayalım `routingUrl` bu konuda daha önce eklemiş meta verileri. Belirttiğiniz özellik değerleri şunlardır: 

      | Özellik | Gereklidir | Value | Açıklama | 
      |----------|----------|-------|-------------| 
      | **Yöntem** | Evet | <*işlem çalıştırma*> | Yapıt üzerinde çalıştırmak için HTTP işlemi. Örneğin, bu HTTP eylemi kullanır **alma** yöntemi. | 
      | **URI** | Evet | <*meta verilerinin konumu*> | Erişim için `routingUrl` yapıdan meta veri değeri alınmış bir ifade, örneğin kullanabilirsiniz: <p>`@{outputs('Integration_Account_Artifact_Lookup')['properties']['metadata']['routingUrl']}` | 
      | **Üst Bilgiler** | Hayır | <*Üstbilgi değerleri*> | Herhangi bir üst bilgisi, HTTP eyleme geçirmek istediğiniz tetikleyiciden çıkarır. Örneğin, tetikleyicinin içinde geçirilecek `headers` özellik değeri: Örneğin bir ifade kullanabilirsiniz: <p>`@triggeroutputs()['headers']` | 
      | **Gövde** | Hayır | <*Gövde içeriği*> | HTTP eylem geçirmek istediğiniz diğer içerikleri `body` özelliği. Bu örnek yapı'nın geçirir `properties` HTTP eyleme değerleri: <p>1. İçine tıklayın **gövdesi** özelliğini dinamik içerik listesi görüntülenir. Hiçbir özellik görünüyorsa, seçin **daha fazla bilgi bkz**. <br>2. Dinamik içerik listesinden, altında **tümleştirme hesabı Yapıt arama**seçin **özellikleri**. | 
      |||| 

      Örneğin:

      ![HTTP eylemi için ifadeleri ve değerleri belirtir](media/logic-apps-enterprise-integration-metadata/add-http-action-values.png)

   1. HTTP eylemi için sağlanan bilgileri denetlemek için mantıksal uygulamanızın JSON tanımını görüntüleme. Mantıksal Uygulama Tasarımcısı araç çubuğunda **kod görünümü** nedenle uygulamanın JSON tanımı görünür, örneğin:

      ![Mantıksal uygulama JSON tanımı](media/logic-apps-enterprise-integration-metadata/finished-logic-app-definition.png)

      Geri mantıksal Uygulama Tasarımcısı'nı geçtikten sonra kullandığınız tüm ifadeleri artık örneğin çözümlenmiş olarak görünür:

      ![Çözümlenen ifadelerinde mantıksal Uygulama Tasarımcısı](media/logic-apps-enterprise-integration-metadata/resolved-expressions.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Sözleşmeleri hakkında daha fazla bilgi edinin](logic-apps-enterprise-integration-agreements.md)
